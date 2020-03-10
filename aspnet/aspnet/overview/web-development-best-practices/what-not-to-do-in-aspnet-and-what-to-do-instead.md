---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: Que ne pas faire dans ASP.NET et que faire à la place | Microsoft Docs
author: Rick-Anderson
description: Cette rubrique décrit plusieurs erreurs courantes que les gens font dans des projets Web ASP.NET. Il fournit des recommandations pour ce que vous devez faire pour éviter ces actions...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 980d3544df70643043391e6573803ce21b3a824f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616990"
---
# <a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a><span data-ttu-id="6782a-104">Ce qu’il ne faut pas faire dans ASP.NET et ce qu’il faut faire à la place</span><span class="sxs-lookup"><span data-stu-id="6782a-104">What not to do in ASP.NET, and what to do instead</span></span>

> <span data-ttu-id="6782a-105">Cette rubrique décrit plusieurs erreurs courantes que les gens font dans des projets Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6782a-105">This topic describes several common mistakes people make within ASP.NET web projects.</span></span> <span data-ttu-id="6782a-106">Il fournit des recommandations pour ce que vous devez faire pour éviter ces erreurs courantes.</span><span class="sxs-lookup"><span data-stu-id="6782a-106">It provides recommendations for what you should do to avoid these common mistakes.</span></span> <span data-ttu-id="6782a-107">Elle est basée sur une [Présentation](http://vimeo.com/68390507) de **Damian Edwards** à la Conférence des développeurs norvégiens.</span><span class="sxs-lookup"><span data-stu-id="6782a-107">It is based on a [presentation](http://vimeo.com/68390507) by **Damian Edwards** at Norwegian Developers Conference.</span></span>

## <a name="disclaimer"></a><span data-ttu-id="6782a-108">Exclusion de responsabilité</span><span class="sxs-lookup"><span data-stu-id="6782a-108">Disclaimer</span></span>

<span data-ttu-id="6782a-109">Cette rubrique n’est pas un guide complet pour garantir la sécurité et l’efficacité de votre application.</span><span class="sxs-lookup"><span data-stu-id="6782a-109">This topic is not intended as a complete guide to ensure your application is secure and efficient.</span></span> <span data-ttu-id="6782a-110">Vous devez toujours suivre les meilleures pratiques en matière de sécurité et de performances qui ne sont pas décrites dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="6782a-110">You still need to follow best practices for security and performance that are not outlined in this topic.</span></span> <span data-ttu-id="6782a-111">Elle suggère uniquement comment éviter les erreurs courantes liées aux processus et aux classes .NET.</span><span class="sxs-lookup"><span data-stu-id="6782a-111">It only suggests how to avoid common mistakes related to .NET classes and processes.</span></span>

## <a name="overview"></a><span data-ttu-id="6782a-112">Présentation</span><span class="sxs-lookup"><span data-stu-id="6782a-112">Overview</span></span>

<span data-ttu-id="6782a-113">Cette rubrique contient les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="6782a-113">This topic contains the following sections:</span></span>

- [<span data-ttu-id="6782a-114">Conformité aux normes</span><span class="sxs-lookup"><span data-stu-id="6782a-114">Standards Compliance</span></span>](#standards)

    - [<span data-ttu-id="6782a-115">Adaptateurs de contrôle</span><span class="sxs-lookup"><span data-stu-id="6782a-115">Control Adapters</span></span>](#adapters)
    - [<span data-ttu-id="6782a-116">Propriétés de style sur les contrôles</span><span class="sxs-lookup"><span data-stu-id="6782a-116">Style Properties on Controls</span></span>](#styleprop)
    - [<span data-ttu-id="6782a-117">Rappels de page et de contrôle</span><span class="sxs-lookup"><span data-stu-id="6782a-117">Page and Control Callbacks</span></span>](#callback)
    - [<span data-ttu-id="6782a-118">Détection des fonctionnalités du navigateur</span><span class="sxs-lookup"><span data-stu-id="6782a-118">Browser Capability Detection</span></span>](#browsercap)
- [<span data-ttu-id="6782a-119">Sécurité</span><span class="sxs-lookup"><span data-stu-id="6782a-119">Security</span></span>](#security)

    - [<span data-ttu-id="6782a-120">Validation de la demande</span><span class="sxs-lookup"><span data-stu-id="6782a-120">Request Validation</span></span>](#validation)
    - [<span data-ttu-id="6782a-121">Authentification par formulaire et session sans cookies</span><span class="sxs-lookup"><span data-stu-id="6782a-121">Cookieless Forms Authentication and Session</span></span>](#cookieless)
    - [<span data-ttu-id="6782a-122">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="6782a-122">EnableViewStateMac</span></span>](#viewstatemac)
    - [<span data-ttu-id="6782a-123">Confiance moyenne</span><span class="sxs-lookup"><span data-stu-id="6782a-123">Medium Trust</span></span>](#medium)
    - [<span data-ttu-id="6782a-124">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="6782a-124">&lt;appSettings&gt;</span></span>](#appsettings)
    - [<span data-ttu-id="6782a-125">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="6782a-125">UrlPathEncode</span></span>](#urlpathencode)
- [<span data-ttu-id="6782a-126">Fiabilité et performances</span><span class="sxs-lookup"><span data-stu-id="6782a-126">Reliability and Performance</span></span>](#performance)

    - [<span data-ttu-id="6782a-127">PreSendRequestHeaders et PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="6782a-127">PreSendRequestHeaders and PreSendRequestContent</span></span>](#presend)
    - [<span data-ttu-id="6782a-128">Événements de page asynchrones avec Web Forms</span><span class="sxs-lookup"><span data-stu-id="6782a-128">Asynchronous Page Events with Web Forms</span></span>](#asyncevents)
    - [<span data-ttu-id="6782a-129">Travail d’incendie et d’oubli</span><span class="sxs-lookup"><span data-stu-id="6782a-129">Fire-and-Forget Work</span></span>](#fire)
    - [<span data-ttu-id="6782a-130">Corps d’entité de la requête</span><span class="sxs-lookup"><span data-stu-id="6782a-130">Request Entity Body</span></span>](#requestentity)
    - [<span data-ttu-id="6782a-131">Response. Redirect et Response. end</span><span class="sxs-lookup"><span data-stu-id="6782a-131">Response.Redirect and Response.End</span></span>](#redirect)
    - [<span data-ttu-id="6782a-132">EnableViewState et ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="6782a-132">EnableViewState and ViewStateMode</span></span>](#viewstatemode)
    - [<span data-ttu-id="6782a-133">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="6782a-133">SqlMembershipProvider</span></span>](#sqlprovider)
    - [<span data-ttu-id="6782a-134">Longues demandes (> 110 secondes)</span><span class="sxs-lookup"><span data-stu-id="6782a-134">Long Running Requests (>110 seconds)</span></span>](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a><span data-ttu-id="6782a-135">Conformité aux normes</span><span class="sxs-lookup"><span data-stu-id="6782a-135">Standards Compliance</span></span>

<a id="adapters"></a>

### <a name="control-adapters"></a><span data-ttu-id="6782a-136">Adaptateurs de contrôle</span><span class="sxs-lookup"><span data-stu-id="6782a-136">Control adapters</span></span>

<span data-ttu-id="6782a-137">Recommandation : Arrêtez d’utiliser des adaptateurs de contrôle pour le rendu adaptatif et utilisez à la place des requêtes de média CSS et du code HTML conforme aux normes.</span><span class="sxs-lookup"><span data-stu-id="6782a-137">Recommendation: Stop using control adapters for adaptive rendering, and instead use CSS media queries and standards-compliant HTML.</span></span>

<span data-ttu-id="6782a-138">Les adaptateurs de contrôles ont été introduits dans .NET 2,0 pour afficher le code de présentation qui a été personnalisé pour différents appareils et environnements.</span><span class="sxs-lookup"><span data-stu-id="6782a-138">Controls Adapters were introduced in .NET 2.0 to render presentation code that was customized for different devices and environments.</span></span> <span data-ttu-id="6782a-139">À présent, ce rendu adaptatif peut être effectué avec CSS et HTML.</span><span class="sxs-lookup"><span data-stu-id="6782a-139">Now, this adaptive rendering can be accomplished with CSS and HTML.</span></span> <span data-ttu-id="6782a-140">Vous devez arrêter d’utiliser des adaptateurs de contrôle et convertir les adaptateurs existants en CSS et HTML.</span><span class="sxs-lookup"><span data-stu-id="6782a-140">You should stop using Control Adapters and convert any existing adapters to CSS and HTML.</span></span>

<span data-ttu-id="6782a-141">Pour plus d’informations, consultez [requêtes de média](http://www.w3.org/TR/css3-mediaqueries/) et [Comment : ajouter des pages mobiles à votre application ASP.NET Web Forms/MVC](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="6782a-141">For more information, see [Media Queries](http://www.w3.org/TR/css3-mediaqueries/) and [How To: Add Mobile Pages to Your ASP.NET Web Forms / MVC Application](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span></span>

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a><span data-ttu-id="6782a-142">Propriétés de style sur les contrôles</span><span class="sxs-lookup"><span data-stu-id="6782a-142">Style properties on controls</span></span>

<span data-ttu-id="6782a-143">Recommandation : Arrêtez la définition des valeurs de style dans le balisage de contrôle et définissez à la place les valeurs de mise en forme dans les feuilles de style CSS.</span><span class="sxs-lookup"><span data-stu-id="6782a-143">Recommendation: Stop setting style values in the control markup, and instead set formatting values in CSS stylesheets.</span></span>

<span data-ttu-id="6782a-144">Les contrôles serveur Web contiennent des dizaines de propriétés qui peuvent être utilisées pour définir des propriétés de style en ligne.</span><span class="sxs-lookup"><span data-stu-id="6782a-144">Web server controls contain dozens of properties which can be used to set in-line style properties.</span></span> <span data-ttu-id="6782a-145">Par exemple, la propriété ForeColor définit la couleur du texte d’un contrôle.</span><span class="sxs-lookup"><span data-stu-id="6782a-145">For example, the ForeColor property sets the color of the text for a control.</span></span> <span data-ttu-id="6782a-146">Vous pouvez obtenir ce même effet plus efficacement par le biais de feuilles de style CSS.</span><span class="sxs-lookup"><span data-stu-id="6782a-146">You can accomplish this same effect more efficiently through CSS stylesheets.</span></span> <span data-ttu-id="6782a-147">Les feuilles de style vous permettent de centraliser les valeurs de style et d’éviter de définir ces valeurs dans l’ensemble de votre application.</span><span class="sxs-lookup"><span data-stu-id="6782a-147">Stylesheets enable you to centralize style values and avoid setting these values throughout your application.</span></span>

<span data-ttu-id="6782a-148">L’exemple suivant montre une classe CSS qui définit le texte sur rouge.</span><span class="sxs-lookup"><span data-stu-id="6782a-148">The following example shows a CSS class the sets text to red.</span></span>

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

<span data-ttu-id="6782a-149">L’exemple suivant montre comment appliquer de manière dynamique la classe CSS.</span><span class="sxs-lookup"><span data-stu-id="6782a-149">The next example shows how to dynamically apply the CSS class.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a><span data-ttu-id="6782a-150">Rappels de page et de contrôle</span><span class="sxs-lookup"><span data-stu-id="6782a-150">Page and control callbacks</span></span>

<span data-ttu-id="6782a-151">Recommandation : Arrêtez d’utiliser les rappels de page et de contrôle et utilisez à la place l’un des éléments suivants : AJAX, UpdatePanel, les méthodes d’action MVC, l’API Web ou Signalr.</span><span class="sxs-lookup"><span data-stu-id="6782a-151">Recommendation: Stop using page and control callbacks, and instead use any of the following: AJAX, UpdatePanel, MVC action methods, Web API, or SignalR.</span></span>

<span data-ttu-id="6782a-152">Dans les versions antérieures de ASP.NET, les méthodes de rappel de page et de contrôle vous permettaient de mettre à jour une partie de la page Web sans actualiser une page entière.</span><span class="sxs-lookup"><span data-stu-id="6782a-152">In earlier versions of ASP.NET, Page and Control callback methods enabled you to update part of the web page without refreshing an entire page.</span></span> <span data-ttu-id="6782a-153">Vous pouvez désormais effectuer des mises à jour de pages partielles via [Ajax](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), l' [API Web](../../../web-api/index.md) ou [signalr](../../../signalr/index.md).</span><span class="sxs-lookup"><span data-stu-id="6782a-153">You can now accomplish partial-page updates through [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [Web API](../../../web-api/index.md) or [SignalR](../../../signalr/index.md).</span></span> <span data-ttu-id="6782a-154">Vous devez cesser d’utiliser des méthodes de rappel, car elles peuvent provoquer des problèmes avec les URL conviviales et le routage.</span><span class="sxs-lookup"><span data-stu-id="6782a-154">You should stop using callback methods because they can cause issues with friendly URLs and routing.</span></span> <span data-ttu-id="6782a-155">Par défaut, les contrôles n’activent pas les méthodes de rappel, mais si vous avez activé cette fonctionnalité dans un contrôle, vous devez la désactiver.</span><span class="sxs-lookup"><span data-stu-id="6782a-155">By default, controls do not enable callback methods, but if you enabled this feature in a control, you should disable it.</span></span>

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a><span data-ttu-id="6782a-156">Détection des fonctionnalités du navigateur</span><span class="sxs-lookup"><span data-stu-id="6782a-156">Browser capability detection</span></span>

<span data-ttu-id="6782a-157">Recommandation : Arrêtez d’utiliser la détection de fonctionnalité de navigateur statique et utilisez à la place la détection de fonctionnalité dynamique.</span><span class="sxs-lookup"><span data-stu-id="6782a-157">Recommendation: Stop using static browser capability detection, and instead use dynamic feature detection.</span></span>

<span data-ttu-id="6782a-158">Dans les versions antérieures de ASP.NET, les fonctionnalités prises en charge pour chaque navigateur étaient stockées dans un fichier XML.</span><span class="sxs-lookup"><span data-stu-id="6782a-158">In earlier versions of ASP.NET, the supported features for each browser were stored in an XML file.</span></span> <span data-ttu-id="6782a-159">La détection de la prise en charge des fonctionnalités par le biais d’une recherche statique n’est pas la meilleure approche.</span><span class="sxs-lookup"><span data-stu-id="6782a-159">Detecting feature support through a static lookup is not the best approach.</span></span> <span data-ttu-id="6782a-160">À présent, vous pouvez détecter de manière dynamique les fonctionnalités prises en charge par un navigateur à l’aide d’une infrastructure de détection des fonctionnalités, telle que [Modernizr](http://modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="6782a-160">Now, you can dynamically detect a browser's supported features by using a feature detection framework, such as [Modernizr](http://modernizr.com/).</span></span> <span data-ttu-id="6782a-161">La détection de fonctionnalités détermine la prise en charge en tentant d’utiliser une méthode ou une propriété, puis de vérifier si le navigateur a produit le résultat souhaité.</span><span class="sxs-lookup"><span data-stu-id="6782a-161">Feature detection determines support by attempting to use a method or property and then checking to see if the browser produced the desired result.</span></span> <span data-ttu-id="6782a-162">Par défaut, Modernizr est inclus dans les modèles d’application Web.</span><span class="sxs-lookup"><span data-stu-id="6782a-162">By default, Modernizr is included in the Web application templates.</span></span>

<a id="security"></a>

## <a name="security"></a><span data-ttu-id="6782a-163">Sécurité</span><span class="sxs-lookup"><span data-stu-id="6782a-163">Security</span></span>

<a id="validation"></a>

### <a name="request-validation"></a><span data-ttu-id="6782a-164">Validation des demandes</span><span class="sxs-lookup"><span data-stu-id="6782a-164">Request validation</span></span>

<span data-ttu-id="6782a-165">Recommandation : valider les entrées d’utilisateur et coder la sortie des utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="6782a-165">Recommendation: Validate user input, and encode output from users.</span></span>

<span data-ttu-id="6782a-166">La validation de la demande est une fonctionnalité de ASP.NET qui inspecte chaque requête et arrête la requête si une menace perçue est détectée.</span><span class="sxs-lookup"><span data-stu-id="6782a-166">Request validation is a feature of ASP.NET that inspects each request and stops the request if a perceived threat is found.</span></span> <span data-ttu-id="6782a-167">Ne dépendez pas de la validation des demandes pour sécuriser votre application contre les attaques de script entre sites.</span><span class="sxs-lookup"><span data-stu-id="6782a-167">Do not depend on request validation for securing your application against cross-site scripting attacks.</span></span> <span data-ttu-id="6782a-168">Au lieu de cela, validez toutes les entrées des utilisateurs et encodez la sortie.</span><span class="sxs-lookup"><span data-stu-id="6782a-168">Instead, validate all input from users and encode the output.</span></span> <span data-ttu-id="6782a-169">Dans certains cas limités, vous pouvez utiliser des expressions régulières pour valider l’entrée, mais dans des cas plus complexes, vous devez valider les entrées d’utilisateur à l’aide de classes .NET qui déterminent si la valeur correspond aux valeurs autorisées.</span><span class="sxs-lookup"><span data-stu-id="6782a-169">In some limited cases, you can use regular expressions to validate the input, but in more complicated cases you should validate user input by using .NET classes that determine if the value matches allowed values.</span></span>

<span data-ttu-id="6782a-170">L’exemple suivant montre comment utiliser une méthode statique dans la classe URI pour déterminer si l’URI fourni par un utilisateur est valide.</span><span class="sxs-lookup"><span data-stu-id="6782a-170">The following example shows how to use a static method in the Uri class to determine whether the Uri provided by a user is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

<span data-ttu-id="6782a-171">Toutefois, pour vérifier suffisamment l’URI, vous devez également vérifier qu’il spécifie `http` ou `https`.</span><span class="sxs-lookup"><span data-stu-id="6782a-171">However, to sufficiently verify the Uri, you should also check to make sure it specifies `http` or `https`.</span></span> <span data-ttu-id="6782a-172">L’exemple suivant utilise des méthodes d’instance pour vérifier que l’URI est valide.</span><span class="sxs-lookup"><span data-stu-id="6782a-172">The following example uses instance methods to verify that the Uri is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

<span data-ttu-id="6782a-173">Avant de restituer l’entrée utilisateur au format HTML ou d’inclure une entrée d’utilisateur dans une requête SQL, encodez les valeurs pour vous assurer que le code malveillant n’est pas inclus.</span><span class="sxs-lookup"><span data-stu-id="6782a-173">Before rendering user input as HTML or including user input in a SQL query, encode the values to ensure malicious code is not included.</span></span>

<span data-ttu-id="6782a-174">Vous pouvez encoder en HTML la valeur dans le balisage avec la syntaxe &lt;% :%&gt;, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="6782a-174">You can HTML encode the value in markup with the &lt;%: %&gt; syntax, as shown below.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

<span data-ttu-id="6782a-175">Ou, dans syntaxe Razor, vous pouvez encoder en HTML avec @, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="6782a-175">Or, in Razor syntax, you can HTML encode with @, as shown below.</span></span>

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="6782a-176">L’exemple suivant montre comment encoder en HTML une valeur dans du code-behind.</span><span class="sxs-lookup"><span data-stu-id="6782a-176">The next example shows how to HTML encode a value in code-behind.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

<span data-ttu-id="6782a-177">Pour encoder en toute sécurité une valeur pour les commandes SQL, utilisez des paramètres de commande tels que [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx).</span><span class="sxs-lookup"><span data-stu-id="6782a-177">To safely encode a value for SQL commands, use command parameters such as the [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx).</span></span> <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a><span data-ttu-id="6782a-178">Authentification par formulaire et session sans cookies</span><span class="sxs-lookup"><span data-stu-id="6782a-178">Cookieless forms authentication and session</span></span>

<span data-ttu-id="6782a-179">Recommandation : exiger des cookies.</span><span class="sxs-lookup"><span data-stu-id="6782a-179">Recommendation: Require cookies.</span></span>

<span data-ttu-id="6782a-180">La transmission des informations d’authentification dans la chaîne de requête n’est pas sécurisée.</span><span class="sxs-lookup"><span data-stu-id="6782a-180">Passing authentication information in the query string is not secure.</span></span> <span data-ttu-id="6782a-181">Par conséquent, exigez des cookies lorsque votre application comprend une authentification.</span><span class="sxs-lookup"><span data-stu-id="6782a-181">Therefore, require cookies when your application includes authentication.</span></span> <span data-ttu-id="6782a-182">Si votre cookie stocke des informations sensibles, envisagez d’exiger SSL pour le cookie.</span><span class="sxs-lookup"><span data-stu-id="6782a-182">If your cookie stores sensitive information, consider requiring SSL for the cookie.</span></span>

<span data-ttu-id="6782a-183">L’exemple suivant montre comment spécifier dans le fichier Web. config que l’authentification par formulaire requiert un cookie transmis via SSL.</span><span class="sxs-lookup"><span data-stu-id="6782a-183">The following example shows how to specify in the Web.config file that Forms Authentication requires a cookie that is transmitted over SSL.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a><span data-ttu-id="6782a-184">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="6782a-184">EnableViewStateMac</span></span>

<span data-ttu-id="6782a-185">Recommandation : n’affectez jamais la valeur false.</span><span class="sxs-lookup"><span data-stu-id="6782a-185">Recommendation: Never set to false.</span></span>

<span data-ttu-id="6782a-186">Par défaut, EnableViewStateMac a la valeur true.</span><span class="sxs-lookup"><span data-stu-id="6782a-186">By default, EnableViewStateMac is set to true.</span></span> <span data-ttu-id="6782a-187">Même si votre application n’utilise pas l’état d’affichage, n’affectez pas la valeur false à EnableViewStateMac.</span><span class="sxs-lookup"><span data-stu-id="6782a-187">Even if your application is not using view state, do not set EnableViewStateMac to false.</span></span> <span data-ttu-id="6782a-188">Si vous définissez cette valeur sur false, votre application sera vulnérable aux scripts inter-sites.</span><span class="sxs-lookup"><span data-stu-id="6782a-188">Setting this value to false will make your application vulnerable to cross-site scripting.</span></span>

<span data-ttu-id="6782a-189">À compter de ASP.NET 4.5.2, le runtime applique **enableViewStateMac = true**.</span><span class="sxs-lookup"><span data-stu-id="6782a-189">Starting with ASP.NET 4.5.2, the runtime enforces **EnableViewStateMac=true**.</span></span> <span data-ttu-id="6782a-190">Même si vous lui affectez la valeur false, le runtime ignore cette valeur et poursuit avec la valeur définie sur true.</span><span class="sxs-lookup"><span data-stu-id="6782a-190">Even if you set it to false, the runtime ignores this value and proceeds with the value set to true.</span></span> <span data-ttu-id="6782a-191">Pour plus d’informations, consultez [ASP.net 4.5.2 et EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span><span class="sxs-lookup"><span data-stu-id="6782a-191">For more information, see [ASP.NET 4.5.2 and EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span></span>

<span data-ttu-id="6782a-192">L’exemple suivant montre comment définir EnableViewStateMac sur true.</span><span class="sxs-lookup"><span data-stu-id="6782a-192">The following example shows how to set EnableViewStateMac to true.</span></span> <span data-ttu-id="6782a-193">Vous n’avez pas besoin de définir cette valeur sur true, car elle est true par défaut.</span><span class="sxs-lookup"><span data-stu-id="6782a-193">You do not need to actually set this value to true because it is true by default.</span></span> <span data-ttu-id="6782a-194">Toutefois, si vous avez défini la valeur false sur n’importe quelle page de votre application, vous devez immédiatement corriger cette valeur.</span><span class="sxs-lookup"><span data-stu-id="6782a-194">However, if you have set it to false on any page in your application, you must immediately correct this value.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a><span data-ttu-id="6782a-195">Confiance moyenne</span><span class="sxs-lookup"><span data-stu-id="6782a-195">Medium trust</span></span>

<span data-ttu-id="6782a-196">Recommandation : ne dépendez pas de la confiance moyenne (ou de tout autre niveau de confiance) comme limite de sécurité.</span><span class="sxs-lookup"><span data-stu-id="6782a-196">Recommendation: Do not depend on Medium Trust (or any other trust level) as a security boundary.</span></span>

<span data-ttu-id="6782a-197">La confiance partielle ne protège pas correctement votre application et ne doit pas être utilisée.</span><span class="sxs-lookup"><span data-stu-id="6782a-197">Partial trust does not adequately protect your application and should not be used.</span></span> <span data-ttu-id="6782a-198">Au lieu de cela, utilisez une confiance totale et isolez les applications non approuvées dans des pools d’applications distincts.</span><span class="sxs-lookup"><span data-stu-id="6782a-198">Instead, use Full Trust, and isolate untrusted applications in separate application pools.</span></span> <span data-ttu-id="6782a-199">Exécutez également chaque pool d’applications sous une identité unique.</span><span class="sxs-lookup"><span data-stu-id="6782a-199">Also, run each application pool under a unique identity.</span></span> <span data-ttu-id="6782a-200">Pour plus d’informations, consultez [confiance partielle ASP.net ne garantit pas l’isolation des applications](https://support.microsoft.com/kb/2698981).</span><span class="sxs-lookup"><span data-stu-id="6782a-200">For more information, see [ASP.NET Partial Trust does not guarantee application isolation](https://support.microsoft.com/kb/2698981).</span></span>

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a><span data-ttu-id="6782a-201">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="6782a-201">&lt;appSettings&gt;</span></span>

<span data-ttu-id="6782a-202">Recommandation : ne désactivez pas les paramètres de sécurité dans &lt;élément appSettings&gt;.</span><span class="sxs-lookup"><span data-stu-id="6782a-202">Recommendation: Do not disable security settings in &lt;appSettings&gt; element.</span></span>

<span data-ttu-id="6782a-203">L’élément appSettings contient de nombreuses valeurs requises pour les mises à jour de sécurité.</span><span class="sxs-lookup"><span data-stu-id="6782a-203">The appSettings element contains many values which are required for security updates.</span></span> <span data-ttu-id="6782a-204">Vous ne devez pas modifier ou désactiver ces valeurs.</span><span class="sxs-lookup"><span data-stu-id="6782a-204">You should not change or disable these values.</span></span> <span data-ttu-id="6782a-205">Si vous devez désactiver ces valeurs lors du déploiement d’une mise à jour, réactivez-la immédiatement après avoir terminé le déploiement.</span><span class="sxs-lookup"><span data-stu-id="6782a-205">If you must disable these values when deploying an update, immediately re-enable after completing deployment.</span></span>

<span data-ttu-id="6782a-206">Pour plus d’informations, consultez [ASP.net appSettings, élément](https://msdn.microsoft.com/library/hh975440.aspx).</span><span class="sxs-lookup"><span data-stu-id="6782a-206">For details, see [ASP.NET appSettings Element](https://msdn.microsoft.com/library/hh975440.aspx).</span></span>

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a><span data-ttu-id="6782a-207">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="6782a-207">UrlPathEncode</span></span>

<span data-ttu-id="6782a-208">Recommandation : utilisez [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) à la place.</span><span class="sxs-lookup"><span data-stu-id="6782a-208">Recommendation: Use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) instead.</span></span>

<span data-ttu-id="6782a-209">La méthode UrlPathEncode a été ajoutée au .NET Framework pour résoudre un problème de compatibilité de navigateur très spécifique.</span><span class="sxs-lookup"><span data-stu-id="6782a-209">The UrlPathEncode method was added to the .NET Framework to resolve a very specific browser compatibility problem.</span></span> <span data-ttu-id="6782a-210">Il n’encode pas correctement une URL et ne protège pas votre application des scripts inter-sites.</span><span class="sxs-lookup"><span data-stu-id="6782a-210">It does not adequately encode a URL, and does not protect your application from cross-site scripting.</span></span> <span data-ttu-id="6782a-211">Vous ne devez jamais l’utiliser dans votre application.</span><span class="sxs-lookup"><span data-stu-id="6782a-211">You should never use it in your application.</span></span> <span data-ttu-id="6782a-212">Utilisez à la place [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).</span><span class="sxs-lookup"><span data-stu-id="6782a-212">Instead, use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).</span></span>

<span data-ttu-id="6782a-213">L’exemple suivant montre comment passer une URL encodée en tant que paramètre de chaîne de requête pour un contrôle de lien hypertexte.</span><span class="sxs-lookup"><span data-stu-id="6782a-213">The following example shows how to pass an encoded URL as a query string parameter for a hyperlink control.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a><span data-ttu-id="6782a-214">Fiabilité et performances</span><span class="sxs-lookup"><span data-stu-id="6782a-214">Reliability and performance</span></span>

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a><span data-ttu-id="6782a-215">PreSendRequestHeaders et PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="6782a-215">PreSendRequestHeaders and PreSendRequestContent</span></span>

<span data-ttu-id="6782a-216">Recommandation : n’utilisez pas ces événements avec les modules managés.</span><span class="sxs-lookup"><span data-stu-id="6782a-216">Recommendation: Do not use these events with managed modules.</span></span> <span data-ttu-id="6782a-217">Au lieu de cela, écrivez un module IIS natif pour effectuer la tâche requise.</span><span class="sxs-lookup"><span data-stu-id="6782a-217">Instead, write a native IIS module to perform the required task.</span></span> <span data-ttu-id="6782a-218">Consultez [création de modules http en code natif](https://msdn.microsoft.com/library/ms693629.aspx).</span><span class="sxs-lookup"><span data-stu-id="6782a-218">See [Creating Native-Code HTTP Modules](https://msdn.microsoft.com/library/ms693629.aspx).</span></span>

<span data-ttu-id="6782a-219">Vous pouvez utiliser les événements [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) et [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) avec les modules IIS natifs.</span><span class="sxs-lookup"><span data-stu-id="6782a-219">You can use the [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) and [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) events with native IIS modules.</span></span>
> [!WARNING]
> <span data-ttu-id="6782a-220">N’utilisez pas `PreSendRequestHeaders` et `PreSendRequestContent` avec les modules managés qui implémentent `IHttpModule`.</span><span class="sxs-lookup"><span data-stu-id="6782a-220">Do not use `PreSendRequestHeaders` and `PreSendRequestContent` with managed modules that implement `IHttpModule`.</span></span> <span data-ttu-id="6782a-221">La définition de ces propriétés peut provoquer des problèmes avec les demandes asynchrones.</span><span class="sxs-lookup"><span data-stu-id="6782a-221">Setting these properties can cause issues with asynchronous requests.</span></span> <span data-ttu-id="6782a-222">La combinaison du routage demandé par l’application (ARR) et de WebSockets peut entraîner des exceptions de violation d’accès qui peuvent provoquer le blocage de w3wp.</span><span class="sxs-lookup"><span data-stu-id="6782a-222">The combination of Application Requested Routing (ARR) and websockets might lead to access violation exceptions that can cause w3wp to crash.</span></span> <span data-ttu-id="6782a-223">Par exemple, iiscore ! W3_CONTEXT_BASE :: GetIsLastNotification + 68 dans iiscore. dll a provoqué une exception de violation d’accès (0xC0000005).</span><span class="sxs-lookup"><span data-stu-id="6782a-223">For example, iiscore!W3_CONTEXT_BASE::GetIsLastNotification+68 in iiscore.dll has caused an access violation exception (0xC0000005).</span></span>

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a><span data-ttu-id="6782a-224">Événements de page asynchrones avec Web Forms</span><span class="sxs-lookup"><span data-stu-id="6782a-224">Asynchronous page events with web forms</span></span>

<span data-ttu-id="6782a-225">Recommandation : dans Web Forms, évitez d’écrire des méthodes Async void pour les événements de cycle de vie de la page et utilisez à la place [page. RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) pour le code asynchrone.</span><span class="sxs-lookup"><span data-stu-id="6782a-225">Recommendation: In Web Forms, avoid writing async void methods for Page lifecycle events, and instead use [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) for asynchronous code.</span></span>

<span data-ttu-id="6782a-226">Quand vous marquez un événement de page avec **Async** et **void**, vous ne pouvez pas déterminer à quel moment le code asynchrone est terminé.</span><span class="sxs-lookup"><span data-stu-id="6782a-226">When you mark a page event with **async** and **void**, you cannot determine when the asynchronous code has finished.</span></span> <span data-ttu-id="6782a-227">Utilisez plutôt page. RegisterAsyncTask pour exécuter le code asynchrone d’une manière qui vous permet d’effectuer le suivi de son achèvement.</span><span class="sxs-lookup"><span data-stu-id="6782a-227">Instead, use Page.RegisterAsyncTask to run the asynchronous code in a way that enables you to track its completion.</span></span>

<span data-ttu-id="6782a-228">L’exemple suivant illustre un gestionnaire de clic de bouton qui contient du code asynchrone.</span><span class="sxs-lookup"><span data-stu-id="6782a-228">The following example shows a button click handler that contains asynchronous code.</span></span> <span data-ttu-id="6782a-229">Cet exemple comprend la lecture d’une valeur de chaîne de manière asynchrone, qui est fournie uniquement sous la forme d’un exemple simplifié d’une tâche asynchrone, et non comme une pratique recommandée.</span><span class="sxs-lookup"><span data-stu-id="6782a-229">This example includes reading a string value asynchronously, which is provided only as a simplified example of an asynchronous task and not as a recommended practice.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

<span data-ttu-id="6782a-230">Si vous utilisez des tâches asynchrones, définissez le Framework cible du runtime http sur 4,5 (ou version ultérieure) dans le fichier Web. config.</span><span class="sxs-lookup"><span data-stu-id="6782a-230">If you are using asynchronous Tasks, set the Http runtime target framework to 4.5 (or later) in the Web.config file.</span></span> <span data-ttu-id="6782a-231">La définition de la version cible du .NET Framework sur 4,5 active le nouveau contexte de synchronisation qui a été ajouté dans .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="6782a-231">Setting the target framework to 4.5 turns on the new synchronization context that was added in .NET 4.5.</span></span> <span data-ttu-id="6782a-232">Cette valeur est définie par défaut dans les nouveaux projets dans Visual Studio, mais elle n’est pas définie si vous travaillez avec un projet existant.</span><span class="sxs-lookup"><span data-stu-id="6782a-232">This value is set by default in new projects in Visual Studio, but is not be set if you are working with an existing project.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a><span data-ttu-id="6782a-233">Travail d’incendie et d’oubli</span><span class="sxs-lookup"><span data-stu-id="6782a-233">Fire-and-forget work</span></span>

<span data-ttu-id="6782a-234">Recommandation : lors du traitement d’une demande dans ASP.NET, évitez de lancer un travail d’activation et d’oubli (par exemple, appeler la méthode ThreadPool. QueueUserWorkItem ou créer un minuteur qui appelle un délégué à plusieurs reprises).</span><span class="sxs-lookup"><span data-stu-id="6782a-234">Recommendation: When handling a request within ASP.NET, avoid launching fire-and-forget work (such calling the ThreadPool.QueueUserWorkItem method or creating a timer that repeatedly calls a delegate).</span></span>

<span data-ttu-id="6782a-235">Si votre application a un travail Fire-and-oublie qui s’exécute dans ASP.NET, votre application peut être désynchronisée. À tout moment, le domaine d’application peut être détruit, ce qui signifie que votre processus en cours peut ne plus correspondre à l’état actuel de l’application.</span><span class="sxs-lookup"><span data-stu-id="6782a-235">If your application has fire-and-forget work that runs within ASP.NET, your application can get out of sync. At any time, the app domain can be destroyed which means your ongoing process may no longer match the current state of the application.</span></span>

<span data-ttu-id="6782a-236">Vous devez déplacer ce type de travail en dehors de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6782a-236">You should move this type of work outside of ASP.NET.</span></span> <span data-ttu-id="6782a-237">Vous pouvez utiliser des tâches Web, un service Windows ou un rôle de travail dans Azure pour effectuer un travail en cours et exécuter ce code à partir d’un autre processus.</span><span class="sxs-lookup"><span data-stu-id="6782a-237">You can use a Web Jobs, Windows Service or a Worker role in Azure to perform ongoing work, and run that code from another process.</span></span>

<span data-ttu-id="6782a-238">Si vous devez effectuer cette opération dans ASP.NET, vous pouvez ajouter le package NuGet appelé [Webbackgroundr](http://www.nuget.org/packages/webbackgrounder) pour exécuter le code.</span><span class="sxs-lookup"><span data-stu-id="6782a-238">If you must perform this work within ASP.NET, you can add the Nuget package called [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) to run the code.</span></span>

<a id="requestentity"></a>

### <a name="request-entity-body"></a><span data-ttu-id="6782a-239">Corps d’entité de la requête</span><span class="sxs-lookup"><span data-stu-id="6782a-239">Request entity body</span></span>

<span data-ttu-id="6782a-240">Recommandation : Évitez de lire Request. Form ou Request. InputStream avant l’événement Execute du gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="6782a-240">Recommendation: Avoid reading Request.Form or Request.InputStream before the handler's execute event.</span></span>

<span data-ttu-id="6782a-241">Au plus tôt, vous devez lire à partir de Request. Form ou Request. InputStream pendant l’événement d’exécution du gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="6782a-241">The earliest you should read from Request.Form or Request.InputStream is during the handler's execute event.</span></span> <span data-ttu-id="6782a-242">Dans MVC, le contrôleur est le gestionnaire et l’événement d’exécution est lors de l’exécution de la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="6782a-242">In MVC, the Controller is the handler and the execute event is when the action method runs.</span></span> <span data-ttu-id="6782a-243">Dans Web Forms, la page est le gestionnaire et l’événement d’exécution est le déclenchement de l’événement page. init.</span><span class="sxs-lookup"><span data-stu-id="6782a-243">In Web Forms, the Page is the handler and the execute event is when the Page.Init event fires.</span></span> <span data-ttu-id="6782a-244">Si vous lisez le corps d’entité de requête antérieur à l’événement d’exécution, vous interfèrez avec le traitement de la requête.</span><span class="sxs-lookup"><span data-stu-id="6782a-244">If you read the request entity body earlier than the execute event, you interfere with the processing of the request.</span></span>

<span data-ttu-id="6782a-245">Si vous devez lire le corps d’entité de la requête avant l’événement d’exécution, utilisez [Request. GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) ou [Request. GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span><span class="sxs-lookup"><span data-stu-id="6782a-245">If you need to read the request entity body before the execute event, use either [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) or [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span></span> <span data-ttu-id="6782a-246">Lorsque vous utilisez GetBufferlessInputStream, vous recevez le flux brut de la demande et assumez la responsabilité du traitement de l’intégralité de la requête.</span><span class="sxs-lookup"><span data-stu-id="6782a-246">When you use GetBufferlessInputStream, you get the raw stream from the request, and assume responsibility for processing the entire request.</span></span> <span data-ttu-id="6782a-247">Après l’appel de GetBufferlessInputStream, Request. Form et Request. InputStream ne sont pas disponibles parce qu’ils n’ont pas été remplis par ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6782a-247">After calling GetBufferlessInputStream, Request.Form and Request.InputStream are not available because they have not been populated by ASP.NET.</span></span> <span data-ttu-id="6782a-248">Lorsque vous utilisez GetBufferedInputStream, vous recevez une copie du flux de la requête.</span><span class="sxs-lookup"><span data-stu-id="6782a-248">When you use GetBufferedInputStream, you get a copy of the stream from the request.</span></span> <span data-ttu-id="6782a-249">Request. Form et Request. InputStream sont toujours disponibles plus tard dans la requête, car ASP.NET remplit l’autre copie.</span><span class="sxs-lookup"><span data-stu-id="6782a-249">Request.Form and Request.InputStream are still available later in the request because ASP.NET populates the other copy.</span></span>

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a><span data-ttu-id="6782a-250">Response. Redirect et Response. end</span><span class="sxs-lookup"><span data-stu-id="6782a-250">Response.Redirect and Response.End</span></span>

<span data-ttu-id="6782a-251">Recommandation : Tenez compte des différences de gestion des threads après l’appel de [Response. Redirect (String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span><span class="sxs-lookup"><span data-stu-id="6782a-251">Recommendation: Be aware of differences in how thread is handled after calling [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span></span>

<span data-ttu-id="6782a-252">La méthode [Response. Redirect (String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) appelle la méthode Response. end.</span><span class="sxs-lookup"><span data-stu-id="6782a-252">The [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) method calls the Response.End method.</span></span> <span data-ttu-id="6782a-253">Dans un processus synchrone, l’appel de Request. Redirect provoque l’abandon immédiat du thread actuel.</span><span class="sxs-lookup"><span data-stu-id="6782a-253">In a synchronous process, calling Request.Redirect causes the current thread to immediately abort.</span></span> <span data-ttu-id="6782a-254">Toutefois, dans un processus asynchrone, l’appel de Response. Redirect n’abandonne pas le thread actuel, de sorte que l’exécution du code continue pour la demande.</span><span class="sxs-lookup"><span data-stu-id="6782a-254">However, in an asynchronous process, calling Response.Redirect does not abort the current thread, so code execution continues for the request.</span></span> <span data-ttu-id="6782a-255">Dans un processus asynchrone, vous devez retourner la tâche à partir de la méthode pour arrêter l’exécution du code.</span><span class="sxs-lookup"><span data-stu-id="6782a-255">In an asynchronous process, you must return the Task from the method to stop the code execution.</span></span>

<span data-ttu-id="6782a-256">Dans un projet MVC, vous ne devez pas appeler Response. Redirect.</span><span class="sxs-lookup"><span data-stu-id="6782a-256">In an MVC project, you should not call Response.Redirect.</span></span> <span data-ttu-id="6782a-257">Au lieu de cela, retournez un RedirectResult.</span><span class="sxs-lookup"><span data-stu-id="6782a-257">Instead, return a RedirectResult.</span></span>

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a><span data-ttu-id="6782a-258">EnableViewState et ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="6782a-258">EnableViewState and ViewStateMode</span></span>

<span data-ttu-id="6782a-259">Recommandation : utilisez ViewStateMode au lieu de EnableViewState pour fournir un contrôle granulaire sur les contrôles qui utilisent l’état d’affichage.</span><span class="sxs-lookup"><span data-stu-id="6782a-259">Recommendation: Use ViewStateMode, instead of EnableViewState, to provide granular control over which controls use view state.</span></span>

<span data-ttu-id="6782a-260">Lorsque vous affectez à EnableViewState la valeur false dans la directive de page, l’état d’affichage est désactivé pour tous les contrôles de la page et ne peut pas être activé.</span><span class="sxs-lookup"><span data-stu-id="6782a-260">When you set EnableViewState to false in the Page directive, view state is disabled for all controls within the page and cannot be enabled.</span></span> <span data-ttu-id="6782a-261">Si vous souhaitez activer l’état d’affichage pour certains contrôles de votre page, affectez à ViewStateMode la valeur Disabled pour la page.</span><span class="sxs-lookup"><span data-stu-id="6782a-261">If you want to enable view state for only certain controls in your page, set ViewStateMode to Disabled for the Page.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

<span data-ttu-id="6782a-262">Ensuite, affectez à ViewStateMode la valeur activé uniquement sur les contrôles qui ont réellement besoin d’un état d’affichage.</span><span class="sxs-lookup"><span data-stu-id="6782a-262">Then, set ViewStateMode to Enabled on only the controls that actually need view state.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

<span data-ttu-id="6782a-263">En activant l’état d’affichage uniquement pour les contrôles qui en ont besoin, vous pouvez réduire la taille de l’état d’affichage de vos pages Web.</span><span class="sxs-lookup"><span data-stu-id="6782a-263">By enabling view state for only the controls that need it, you can shrink the size of the view state for your web pages.</span></span>

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a><span data-ttu-id="6782a-264">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="6782a-264">SqlMembershipProvider</span></span>

<span data-ttu-id="6782a-265">Recommandation : utilisez Fournisseurs universels.</span><span class="sxs-lookup"><span data-stu-id="6782a-265">Recommendation: Use Universal Providers.</span></span>

<span data-ttu-id="6782a-266">Dans les modèles de projet actuels, SqlMembershipProvider a été remplacé par [fournisseurs universels ASP.net](http://www.nuget.org/packages/Microsoft.AspNet.Providers), qui est disponible sous forme de package NuGet.</span><span class="sxs-lookup"><span data-stu-id="6782a-266">In the current project templates, SqlMembershipProvider has been replaced by [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), which is available as a NuGet package.</span></span> <span data-ttu-id="6782a-267">Si vous utilisez SqlMembershipProvider dans un projet qui a été généré avec une version antérieure des modèles, vous devez basculer vers Fournisseurs universels.</span><span class="sxs-lookup"><span data-stu-id="6782a-267">If you are using SqlMembershipProvider in a project that was built with an earlier version of the templates, you should switch to Universal Providers.</span></span> <span data-ttu-id="6782a-268">L’Fournisseurs universels utiliser toutes les bases de données prises en charge par Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6782a-268">The Universal Providers work with all databases that are supported by Entity Framework.</span></span>

<span data-ttu-id="6782a-269">Pour plus d’informations, consultez [Présentation de fournisseurs universels ASP.net](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span><span class="sxs-lookup"><span data-stu-id="6782a-269">For more information, see [Introducing ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span></span>

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a><span data-ttu-id="6782a-270">Demandes longues (> 110 secondes)</span><span class="sxs-lookup"><span data-stu-id="6782a-270">Long-running requests (>110 seconds)</span></span>

<span data-ttu-id="6782a-271">Recommandation : utilisez [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) ou [signalr](../../../signalr/index.md) pour les clients connectés et utilisez des opérations d’e/s asynchrones.</span><span class="sxs-lookup"><span data-stu-id="6782a-271">Recommendation: Use [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) or [SignalR](../../../signalr/index.md) for connected clients, and use asynchronous I/O operations.</span></span>

<span data-ttu-id="6782a-272">Les requêtes de longue durée peuvent entraîner des résultats imprévisibles et des performances médiocres dans votre application Web.</span><span class="sxs-lookup"><span data-stu-id="6782a-272">Long-running requests can cause unpredictable results and poor performance in your web application.</span></span> <span data-ttu-id="6782a-273">Le paramètre de délai d’expiration par défaut pour une demande est de 110 secondes.</span><span class="sxs-lookup"><span data-stu-id="6782a-273">The default timeout setting for a request is 110 seconds.</span></span> <span data-ttu-id="6782a-274">Si vous utilisez l’état de session avec une demande longue, ASP.NET libère le verrou sur l’objet de session après 110 secondes.</span><span class="sxs-lookup"><span data-stu-id="6782a-274">If you are using session state with a long-running request, ASP.NET will release the lock on the Session object after 110 seconds.</span></span> <span data-ttu-id="6782a-275">Toutefois, votre application peut être au milieu d’une opération sur l’objet de session lorsque le verrou est libéré, et l’opération peut ne pas se terminer correctement.</span><span class="sxs-lookup"><span data-stu-id="6782a-275">However, your application might be in the middle of an operation on the Session object when the lock is released, and the operation might not complete successfully.</span></span> <span data-ttu-id="6782a-276">Si une deuxième demande de l’utilisateur est bloquée pendant que la première demande est en cours d’exécution, la deuxième demande peut accéder à l’objet de session dans un état incohérent.</span><span class="sxs-lookup"><span data-stu-id="6782a-276">If a second request from the user is blocked while the first request is running, the second request might access the Session object in an inconsistent state.</span></span>

<span data-ttu-id="6782a-277">Si votre application comprend des opérations d’e/s bloquantes (ou synchrones), l’application ne répondra pas.</span><span class="sxs-lookup"><span data-stu-id="6782a-277">If your application includes blocking (or synchronous) I/O operations, the application will be unresponsive.</span></span>

<span data-ttu-id="6782a-278">Pour améliorer les performances, utilisez les opérations d’e/s asynchrones dans le .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="6782a-278">To improve performance, use the asynchronous I/O operations in the .NET Framework.</span></span> <span data-ttu-id="6782a-279">En outre, utilisez WebSockets ou Signalr pour connecter les clients au serveur.</span><span class="sxs-lookup"><span data-stu-id="6782a-279">Also, use WebSockets or SignalR for connecting clients to the server.</span></span> <span data-ttu-id="6782a-280">Ces fonctionnalités sont conçues pour gérer efficacement les requêtes à long terme.</span><span class="sxs-lookup"><span data-stu-id="6782a-280">These features are designed to efficiently handle long-running requests.</span></span>
