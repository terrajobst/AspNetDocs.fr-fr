---
uid: whitepapers/request-validation
title: Validation des demandes-prévention des attaques de script | Microsoft Docs
author: rick-anderson
description: Ce document décrit la fonctionnalité de validation de la demande de ASP.NET où, par défaut, l’application n’est pas en mesure de traiter le contenu HTML non encodé submitt...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 807cccd6fe1acdd6359b014387abd3878840d4cd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78640846"
---
# <a name="request-validation---preventing-script-attacks"></a><span data-ttu-id="28772-103">Validation des demandes - Prévention des attaques par script</span><span class="sxs-lookup"><span data-stu-id="28772-103">Request Validation - Preventing Script Attacks</span></span>

> <span data-ttu-id="28772-104">Ce document décrit la fonctionnalité de validation de la demande de ASP.NET où, par défaut, l’application n’est pas en mesure de traiter le contenu HTML non encodé envoyé au serveur.</span><span class="sxs-lookup"><span data-stu-id="28772-104">This paper describes the request validation feature of ASP.NET where, by default, the application is prevented from processing unencoded HTML content submitted to the server.</span></span> <span data-ttu-id="28772-105">Cette fonctionnalité de validation de requête peut être désactivée lorsque l’application a été conçue pour traiter en toute sécurité des données HTML.</span><span class="sxs-lookup"><span data-stu-id="28772-105">This request validation feature can be disabled when the application has been designed to safely process HTML data.</span></span>
> 
> <span data-ttu-id="28772-106">S’applique à ASP.NET 1,1 et ASP.NET 2,0.</span><span class="sxs-lookup"><span data-stu-id="28772-106">Applies to ASP.NET 1.1 and ASP.NET 2.0.</span></span>

<span data-ttu-id="28772-107">La fonctionnalité de validation des demandes, disponible dans ASP.NET depuis la version 1.1, empêche le serveur d’accepter les contenus intégrant du code HTML non encodé.</span><span class="sxs-lookup"><span data-stu-id="28772-107">Request validation, a feature of ASP.NET since version 1.1, prevents the server from accepting content containing un-encoded HTML.</span></span> <span data-ttu-id="28772-108">Cette fonctionnalité est destinée à éviter certaines attaques par injection de script dans lesquelles un code de script client ou HTML peut être, à l’insu de tous, envoyé à un serveur, stocké, puis présenté à d’autres utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="28772-108">This feature is designed to help prevent some script-injection attacks whereby client script code or HTML can be unknowingly submitted to a server, stored, and then presented to other users.</span></span> <span data-ttu-id="28772-109">Nous vous recommandons vivement de valider toutes les données d’entrée et de les encoder en HTML s’il y a lieu.</span><span class="sxs-lookup"><span data-stu-id="28772-109">We still strongly recommend that you validate all input data and HTML encode it when appropriate.</span></span>

<span data-ttu-id="28772-110">Par exemple, vous créez une page Web qui demande l’adresse de messagerie d’un utilisateur, puis stocke cette adresse de messagerie dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="28772-110">For example, you create a Web page that requests a user's email address and then stores that email address in a database.</span></span> <span data-ttu-id="28772-111">Si l’utilisateur entre &lt;SCRIPT&gt;alerte (« Bonjour à partir du script »)&lt;/SCRIPT&gt; au lieu d’une adresse e-mail valide, lorsque ces données sont présentées, ce script peut être exécuté si le contenu n’a pas été correctement encodé.</span><span class="sxs-lookup"><span data-stu-id="28772-111">If the user enters &lt;SCRIPT&gt;alert("hello from script")&lt;/SCRIPT&gt; instead of a valid email address, when that data is presented, this script can be executed if the content was not properly encoded.</span></span> <span data-ttu-id="28772-112">La fonctionnalité de validation de la demande de ASP.NET empêche ce problème.</span><span class="sxs-lookup"><span data-stu-id="28772-112">The request validation feature of ASP.NET prevents this from happening.</span></span>

## <a name="why-this-feature-is-useful"></a><span data-ttu-id="28772-113">Pourquoi cette fonctionnalité est utile</span><span class="sxs-lookup"><span data-stu-id="28772-113">Why this feature is useful</span></span>

<span data-ttu-id="28772-114">De nombreux sites ne savent pas qu’ils sont ouverts aux attaques par injection de script simples.</span><span class="sxs-lookup"><span data-stu-id="28772-114">Many sites are not aware that they are open to simple script injection attacks.</span></span> <span data-ttu-id="28772-115">Que le but de ces attaques soit de défaire le site en affichant du code HTML ou d’exécuter potentiellement le script client pour rediriger l’utilisateur vers le site d’un pirate, les attaques par injection de script sont un problème que les développeurs Web doivent faire face.</span><span class="sxs-lookup"><span data-stu-id="28772-115">Whether the purpose of these attacks is to deface the site by displaying HTML, or to potentially execute client script to redirect the user to a hacker's site, script injection attacks are a problem that Web developers must contend with.</span></span>

<span data-ttu-id="28772-116">Les attaques par injection de script sont un problème de tous les développeurs Web, qu’ils utilisent ASP.NET, ASP ou d’autres technologies de développement Web.</span><span class="sxs-lookup"><span data-stu-id="28772-116">Script injection attacks are a concern of all web developers, whether they are using ASP.NET, ASP, or other web development technologies.</span></span>

<span data-ttu-id="28772-117">La fonctionnalité de validation des demandes ASP.NET empêche ces attaques en n’autorisant pas le traitement du contenu HTML non encodé par le serveur, sauf si le développeur décide d’autoriser ce contenu.</span><span class="sxs-lookup"><span data-stu-id="28772-117">The ASP.NET request validation feature proactively prevents these attacks by not allowing unencoded HTML content to be processed by the server unless the developer decides to allow that content.</span></span>

## <a name="what-to-expect-error-page"></a><span data-ttu-id="28772-118">À quoi s’attendre : page d’erreur</span><span class="sxs-lookup"><span data-stu-id="28772-118">What to expect: Error Page</span></span>

<span data-ttu-id="28772-119">La capture d’écran ci-dessous montre un exemple de code ASP.NET :</span><span class="sxs-lookup"><span data-stu-id="28772-119">The screen shot below shows some sample ASP.NET code:</span></span>

![](request-validation/_static/image1.png)

<span data-ttu-id="28772-120">L’exécution de ce code génère une page simple qui vous permet d’entrer du texte dans la zone de texte, de cliquer sur le bouton et d’afficher le texte dans le contrôle Label :</span><span class="sxs-lookup"><span data-stu-id="28772-120">Running this code results in a simple page that allows you to enter some text in the textbox, click the button, and display the text in the label control:</span></span>

![](request-validation/_static/image2.png)

<span data-ttu-id="28772-121">Toutefois, il s’agissait de JavaScript, par exemple `<script>alert("hello!")</script>` à entrer et soumis, nous obtenons une exception :</span><span class="sxs-lookup"><span data-stu-id="28772-121">However, were JavaScript, such as `<script>alert("hello!")</script>` to be entered and submitted we would get an exception:</span></span>

![](request-validation/_static/image3.png)

<span data-ttu-id="28772-122">Le message d’erreur indique qu’une valeur de demande de formulaire potentiellement dangereuse a été détectée et fournit des détails supplémentaires dans la description pour savoir exactement ce qui s’est produit et comment modifier le comportement.</span><span class="sxs-lookup"><span data-stu-id="28772-122">The error message states that a 'potentially dangerous Request.Form value was detected' and provides more details in the description as to exactly what occurred and how to change the behavior.</span></span> <span data-ttu-id="28772-123">Exemple :</span><span class="sxs-lookup"><span data-stu-id="28772-123">For example:</span></span>

<span data-ttu-id="28772-124">La validation de la demande a détecté une valeur d’entrée client potentiellement dangereuse et le traitement de la requête a été abandonné.</span><span class="sxs-lookup"><span data-stu-id="28772-124">Request validation has detected a potentially dangerous client input value, and processing of the request has been aborted.</span></span> <span data-ttu-id="28772-125">Cette valeur peut indiquer une tentative de compromettre la sécurité de votre application, telle qu’une attaque de script entre sites.</span><span class="sxs-lookup"><span data-stu-id="28772-125">This value may indicate an attempt to compromise the security of your application, such as a cross-site scripting attack.</span></span> <span data-ttu-id="28772-126">Vous pouvez désactiver la validation de la demande en définissant `validateRequest=false` dans la directive de page ou dans la section de configuration.</span><span class="sxs-lookup"><span data-stu-id="28772-126">You can disable request validation by setting `validateRequest=false` in the Page directive or in the configuration section.</span></span> <span data-ttu-id="28772-127">Toutefois, il est fortement recommandé que votre application vérifie explicitement toutes les entrées dans ce cas.</span><span class="sxs-lookup"><span data-stu-id="28772-127">However, it is strongly recommended that your application explicitly check all inputs in this case.</span></span>

## <a name="disabling-request-validation-on-a-page"></a><span data-ttu-id="28772-128">Désactivation de la validation de requête sur une page</span><span class="sxs-lookup"><span data-stu-id="28772-128">Disabling request validation on a page</span></span>

<span data-ttu-id="28772-129">Pour désactiver la validation de la demande sur une page, vous devez définir l’attribut `validateRequest` de la directive page sur `false`:</span><span class="sxs-lookup"><span data-stu-id="28772-129">To disable request validation on a page you must set the `validateRequest` attribute of the Page directive to `false`:</span></span>

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> <span data-ttu-id="28772-130">Lorsque la validation de la demande est désactivée, le contenu peut être soumis à une page ; Il incombe au développeur de page de s’assurer que le contenu est correctement encodé ou traité.</span><span class="sxs-lookup"><span data-stu-id="28772-130">When request validation is disabled, content can be submitted to a page; it is the responsibility of the page developer to ensure that content is properly encoded or processed.</span></span>

## <a name="disabling-request-validation-for-your-application"></a><span data-ttu-id="28772-131">Désactivation de la validation de demande pour votre application</span><span class="sxs-lookup"><span data-stu-id="28772-131">Disabling request validation for your application</span></span>

<span data-ttu-id="28772-132">Pour désactiver la validation de la demande pour votre application, vous devez modifier ou créer un fichier Web. config pour votre application et définir l’attribut validateRequest de la section `<pages />` sur `false`:</span><span class="sxs-lookup"><span data-stu-id="28772-132">To disable request validation for your application, you must modify or create a Web.config file for your application and set the validateRequest attribute of the `<pages />` section to `false`:</span></span>

[!code-xml[Main](request-validation/samples/sample2.xml)]

<span data-ttu-id="28772-133">Si vous souhaitez désactiver la validation de la demande pour toutes les applications sur votre serveur, vous pouvez apporter cette modification à votre fichier machine. config.</span><span class="sxs-lookup"><span data-stu-id="28772-133">If you wish to disable request validation for all applications on your server, you can make this modification to your Machine.config file.</span></span>

> [!CAUTION]
> <span data-ttu-id="28772-134">Lorsque la validation de la demande est désactivée, le contenu peut être soumis à votre application. Il incombe au développeur de l’application de s’assurer que le contenu est correctement encodé ou traité.</span><span class="sxs-lookup"><span data-stu-id="28772-134">When request validation is disabled, content can be submitted to your application; it is the responsibility of the application developer to ensure that content is properly encoded or processed.</span></span>

<span data-ttu-id="28772-135">Le code ci-dessous est modifié pour désactiver la validation de la demande :</span><span class="sxs-lookup"><span data-stu-id="28772-135">The code below is modified to turn off request validation:</span></span>

![](request-validation/_static/image4.png)

<span data-ttu-id="28772-136">Maintenant, si le code JavaScript suivant a été entré dans la zone de texte `<script>alert("hello!")</script>` le résultat serait :</span><span class="sxs-lookup"><span data-stu-id="28772-136">Now if the following JavaScript was entered into the textbox `<script>alert("hello!")</script>` the result would be:</span></span>

![](request-validation/_static/image5.png)

<span data-ttu-id="28772-137">Pour éviter cela, si la validation de la demande est désactivée, nous devons coder le contenu en HTML.</span><span class="sxs-lookup"><span data-stu-id="28772-137">To prevent this from happening, with request validation turned off, we need to HTML encode the content.</span></span>

## <a name="how-to-html-encode-content"></a><span data-ttu-id="28772-138">Codage HTML du contenu</span><span class="sxs-lookup"><span data-stu-id="28772-138">How to HTML encode content</span></span>

<span data-ttu-id="28772-139">Si vous avez désactivé la validation des demandes, il est recommandé d’encoder le contenu HTML qui sera stocké pour une utilisation ultérieure.</span><span class="sxs-lookup"><span data-stu-id="28772-139">If you have disabled request validation, it is good practice to HTML-encode content that will be stored for future use.</span></span> <span data-ttu-id="28772-140">L’encodage HTML remplace automatiquement tout'&lt;'ou'&gt;' (avec plusieurs autres symboles) par leur représentation HTML encodée correspondante.</span><span class="sxs-lookup"><span data-stu-id="28772-140">HTML encoding will automatically replace any ‘&lt;' or ‘&gt;' (together with several other symbols) with their corresponding HTML encoded representation.</span></span> <span data-ttu-id="28772-141">Par exemple, «&lt;» est remplacé par «&amp;lt ; » et «&gt;» est remplacé par «&amp;gt ; ».</span><span class="sxs-lookup"><span data-stu-id="28772-141">For example, ‘&lt;' is replaced by ‘&amp;lt;' and ‘&gt;' is replaced by ‘&amp;gt;'.</span></span> <span data-ttu-id="28772-142">Les navigateurs utilisent ces codes spéciaux pour afficher le «&lt;» ou le «&gt;» dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="28772-142">Browsers use these special codes to display the ‘&lt;' or ‘&gt;' in the browser.</span></span>

<span data-ttu-id="28772-143">Le contenu peut être facilement encodé en HTML sur le serveur à l’aide de l’API `Server.HtmlEncode(string)`.</span><span class="sxs-lookup"><span data-stu-id="28772-143">Content can be easily HTML-encoded on the server using the `Server.HtmlEncode(string)` API.</span></span> <span data-ttu-id="28772-144">Le contenu peut également être facilement décodé en HTML, c’est-à-dire revenir au code HTML standard à l’aide de la méthode `Server.HtmlDecode(string)`.</span><span class="sxs-lookup"><span data-stu-id="28772-144">Content can also be easily HTML-decoded, that is, reverted back to standard HTML using the `Server.HtmlDecode(string)` method.</span></span>

![](request-validation/_static/image6.png)

<span data-ttu-id="28772-145">Ce qui donne :</span><span class="sxs-lookup"><span data-stu-id="28772-145">Resulting in:</span></span>

![](request-validation/_static/image7.png)
