---
uid: web-api/overview/security/basic-authentication
title: Authentification de base dans API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: Décrit l’utilisation de l’authentification de base dans API Web ASP.NET.
ms.author: riande
ms.date: 10/02/2014
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 1470bd4b5abd5199b9a5105973b053812d643351
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555726"
---
# <a name="basic-authentication-in-aspnet-web-api"></a><span data-ttu-id="063ed-103">Authentification de base dans API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="063ed-103">Basic Authentication in ASP.NET Web API</span></span>

<span data-ttu-id="063ed-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="063ed-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="063ed-105">L’authentification de base est définie dans le [document RFC 2617, authentification http : authentification d’accès de base et Digest](http://www.ietf.org/rfc/rfc2617.txt).</span><span class="sxs-lookup"><span data-stu-id="063ed-105">Basic authentication is defined in [RFC 2617, HTTP Authentication: Basic and Digest Access Authentication](http://www.ietf.org/rfc/rfc2617.txt).</span></span>

<span data-ttu-id="063ed-106">Inconvénients</span><span class="sxs-lookup"><span data-stu-id="063ed-106">Disadvantages</span></span>

- <span data-ttu-id="063ed-107">Les informations d’identification de l’utilisateur sont envoyées dans la demande.</span><span class="sxs-lookup"><span data-stu-id="063ed-107">User credentials are sent in the request.</span></span>
- <span data-ttu-id="063ed-108">Les informations d’identification sont envoyées en texte clair.</span><span class="sxs-lookup"><span data-stu-id="063ed-108">Credentials are sent as plaintext.</span></span>
- <span data-ttu-id="063ed-109">Les informations d’identification sont envoyées avec chaque demande.</span><span class="sxs-lookup"><span data-stu-id="063ed-109">Credentials are sent with every request.</span></span>
- <span data-ttu-id="063ed-110">Aucun moyen de se déconnecter, sauf en mettant fin à la session du navigateur.</span><span class="sxs-lookup"><span data-stu-id="063ed-110">No way to log out, except by ending the browser session.</span></span>
- <span data-ttu-id="063ed-111">Vulnérable à la falsification de requête intersites (CSRF); requiert des mesures anti-CSRF.</span><span class="sxs-lookup"><span data-stu-id="063ed-111">Vulnerable to cross-site request forgery (CSRF); requires anti-CSRF measures.</span></span>

<span data-ttu-id="063ed-112">Avantages</span><span class="sxs-lookup"><span data-stu-id="063ed-112">Advantages</span></span>

- <span data-ttu-id="063ed-113">Internet standard.</span><span class="sxs-lookup"><span data-stu-id="063ed-113">Internet standard.</span></span>
- <span data-ttu-id="063ed-114">Pris en charge par tous les principaux navigateurs.</span><span class="sxs-lookup"><span data-stu-id="063ed-114">Supported by all major browsers.</span></span>
- <span data-ttu-id="063ed-115">Protocole relativement simple.</span><span class="sxs-lookup"><span data-stu-id="063ed-115">Relatively simple protocol.</span></span>

<span data-ttu-id="063ed-116">L’authentification de base fonctionne comme suit :</span><span class="sxs-lookup"><span data-stu-id="063ed-116">Basic authentication works as follows:</span></span>

1. <span data-ttu-id="063ed-117">Si une demande requiert une authentification, le serveur retourne 401 (non autorisé).</span><span class="sxs-lookup"><span data-stu-id="063ed-117">If a request requires authentication, the server returns 401 (Unauthorized).</span></span> <span data-ttu-id="063ed-118">La réponse comprend un en-tête WWW-Authenticate, indiquant que le serveur prend en charge l’authentification de base.</span><span class="sxs-lookup"><span data-stu-id="063ed-118">The response includes a WWW-Authenticate header, indicating the server supports Basic authentication.</span></span>
2. <span data-ttu-id="063ed-119">Le client envoie une autre demande, avec les informations d’identification du client dans l’en-tête Authorization.</span><span class="sxs-lookup"><span data-stu-id="063ed-119">The client sends another request, with the client credentials in the Authorization header.</span></span> <span data-ttu-id="063ed-120">Les informations d’identification sont mises en forme en tant que chaîne « nom : mot de passe », encodée en base64.</span><span class="sxs-lookup"><span data-stu-id="063ed-120">The credentials are formatted as the string "name:password", base64-encoded.</span></span> <span data-ttu-id="063ed-121">Les informations d’identification ne sont pas chiffrées.</span><span class="sxs-lookup"><span data-stu-id="063ed-121">The credentials are not encrypted.</span></span>

<span data-ttu-id="063ed-122">L’authentification de base est effectuée dans le contexte d’un « domaine ».</span><span class="sxs-lookup"><span data-stu-id="063ed-122">Basic authentication is performed within the context of a "realm."</span></span> <span data-ttu-id="063ed-123">Le serveur comprend le nom du domaine dans l’en-tête WWW-Authenticate.</span><span class="sxs-lookup"><span data-stu-id="063ed-123">The server includes the name of the realm in the WWW-Authenticate header.</span></span> <span data-ttu-id="063ed-124">Les informations d’identification de l’utilisateur sont valides dans ce domaine.</span><span class="sxs-lookup"><span data-stu-id="063ed-124">The user's credentials are valid within that realm.</span></span> <span data-ttu-id="063ed-125">La portée exacte d’un domaine est définie par le serveur.</span><span class="sxs-lookup"><span data-stu-id="063ed-125">The exact scope of a realm is defined by the server.</span></span> <span data-ttu-id="063ed-126">Par exemple, vous pouvez définir plusieurs domaines afin de partitionner les ressources.</span><span class="sxs-lookup"><span data-stu-id="063ed-126">For example, you might define several realms in order to partition resources.</span></span>

![](basic-authentication/_static/image1.png)

<span data-ttu-id="063ed-127">Étant donné que les informations d’identification sont envoyées non chiffrées, l’authentification de base est sécurisée uniquement via HTTPs.</span><span class="sxs-lookup"><span data-stu-id="063ed-127">Because the credentials are sent unencrypted, Basic authentication is only secure over HTTPS.</span></span> <span data-ttu-id="063ed-128">Consultez [utilisation de SSL dans l’API Web](working-with-ssl-in-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="063ed-128">See [Working with SSL in Web API](working-with-ssl-in-web-api.md).</span></span>

<span data-ttu-id="063ed-129">L’authentification de base est également vulnérable aux attaques CSRF.</span><span class="sxs-lookup"><span data-stu-id="063ed-129">Basic authentication is also vulnerable to CSRF attacks.</span></span> <span data-ttu-id="063ed-130">Une fois que l’utilisateur a entré les informations d’identification, le navigateur les envoie automatiquement aux demandes suivantes au même domaine, pour la durée de la session.</span><span class="sxs-lookup"><span data-stu-id="063ed-130">After the user enters credentials, the browser automatically sends them on subsequent requests to the same domain, for the duration of the session.</span></span> <span data-ttu-id="063ed-131">Cela comprend les demandes AJAX.</span><span class="sxs-lookup"><span data-stu-id="063ed-131">This includes AJAX requests.</span></span> <span data-ttu-id="063ed-132">Consultez la page [prévention des attaques de falsification de requête intersites (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="063ed-132">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>

## <a name="basic-authentication-with-iis"></a><span data-ttu-id="063ed-133">Authentification de base avec IIS</span><span class="sxs-lookup"><span data-stu-id="063ed-133">Basic Authentication with IIS</span></span>

<span data-ttu-id="063ed-134">IIS prend en charge l’authentification de base, mais il y a un inconvénient : l’utilisateur est authentifié par rapport à ses informations d’identification Windows.</span><span class="sxs-lookup"><span data-stu-id="063ed-134">IIS supports Basic authentication, but there is a caveat: The user is authenticated against their Windows credentials.</span></span> <span data-ttu-id="063ed-135">Cela signifie que l’utilisateur doit disposer d’un compte sur le domaine du serveur.</span><span class="sxs-lookup"><span data-stu-id="063ed-135">That means the user must have an account on the server's domain.</span></span> <span data-ttu-id="063ed-136">Pour un site Web public, vous souhaitez généralement vous authentifier auprès d’un fournisseur d’appartenances ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="063ed-136">For a public-facing web site, you typically want to authenticate against an ASP.NET membership provider.</span></span>

<span data-ttu-id="063ed-137">Pour activer l’authentification de base à l’aide d’IIS, définissez le mode d’authentification sur « Windows » dans le fichier Web. config de votre projet ASP.NET :</span><span class="sxs-lookup"><span data-stu-id="063ed-137">To enable Basic authentication using IIS, set the authentication mode to "Windows" in the Web.config of your ASP.NET project:</span></span>

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

<span data-ttu-id="063ed-138">Dans ce mode, IIS utilise les informations d’identification Windows pour s’authentifier.</span><span class="sxs-lookup"><span data-stu-id="063ed-138">In this mode, IIS uses Windows credentials to authenticate.</span></span> <span data-ttu-id="063ed-139">En outre, vous devez activer l’authentification de base dans IIS.</span><span class="sxs-lookup"><span data-stu-id="063ed-139">In addition, you must enable Basic authentication in IIS.</span></span> <span data-ttu-id="063ed-140">Dans le gestionnaire des services Internet, accédez à l’affichage des fonctionnalités, sélectionnez authentification, puis activez l’authentification de base.</span><span class="sxs-lookup"><span data-stu-id="063ed-140">In IIS Manager, go to Features View, select Authentication, and enable Basic authentication.</span></span>

![](basic-authentication/_static/image2.png)

<span data-ttu-id="063ed-141">Dans votre projet d’API Web, ajoutez l’attribut `[Authorize]` pour toutes les actions de contrôleur qui nécessitent une authentification.</span><span class="sxs-lookup"><span data-stu-id="063ed-141">In your Web API project, add the `[Authorize]` attribute for any controller actions that need authentication.</span></span>

<span data-ttu-id="063ed-142">Un client s’authentifie lui-même en définissant l’en-tête d’autorisation dans la demande.</span><span class="sxs-lookup"><span data-stu-id="063ed-142">A client authenticates itself by setting the Authorization header in the request.</span></span> <span data-ttu-id="063ed-143">Les clients de navigateur effectuent cette étape automatiquement.</span><span class="sxs-lookup"><span data-stu-id="063ed-143">Browser clients perform this step automatically.</span></span> <span data-ttu-id="063ed-144">Les clients qui ne sont pas des navigateurs doivent définir l’en-tête.</span><span class="sxs-lookup"><span data-stu-id="063ed-144">Nonbrowser clients will need to set the header.</span></span>

## <a name="basic-authentication-with-custom-membership"></a><span data-ttu-id="063ed-145">Authentification de base avec appartenance personnalisée</span><span class="sxs-lookup"><span data-stu-id="063ed-145">Basic Authentication with Custom Membership</span></span>

<span data-ttu-id="063ed-146">Comme mentionné précédemment, l’authentification de base intégrée à IIS utilise les informations d’identification Windows.</span><span class="sxs-lookup"><span data-stu-id="063ed-146">As mentioned, the Basic Authentication built into IIS uses Windows credentials.</span></span> <span data-ttu-id="063ed-147">Cela signifie que vous devez créer des comptes pour vos utilisateurs sur le serveur d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="063ed-147">That means you need to create accounts for your users on the hosting server.</span></span> <span data-ttu-id="063ed-148">Toutefois, pour une application Internet, les comptes d’utilisateurs sont généralement stockés dans une base de données externe.</span><span class="sxs-lookup"><span data-stu-id="063ed-148">But for an internet application, user accounts are typically stored in an external database.</span></span>

<span data-ttu-id="063ed-149">Le code suivant montre comment un module HTTP qui effectue l’authentification de base.</span><span class="sxs-lookup"><span data-stu-id="063ed-149">The following code how an HTTP module that performs Basic Authentication.</span></span> <span data-ttu-id="063ed-150">Vous pouvez facilement intégrer un fournisseur d’appartenances ASP.NET en remplaçant la méthode `CheckPassword`, qui est une méthode factice dans cet exemple.</span><span class="sxs-lookup"><span data-stu-id="063ed-150">You can easily plug in an ASP.NET membership provider by replacing the `CheckPassword` method, which is a dummy method in this example.</span></span>

<span data-ttu-id="063ed-151">Dans l’API Web 2, vous devez envisager d’écrire un [filtre d’authentification](authentication-filters.md) ou un [intergiciel (middleware) OWIN](../../../aspnet/overview/owin-and-katana/index.md)à la place d’un module http.</span><span class="sxs-lookup"><span data-stu-id="063ed-151">In Web API 2, you should consider writing an [authentication filter](authentication-filters.md) or [OWIN middleware](../../../aspnet/overview/owin-and-katana/index.md), instead of an HTTP module.</span></span>

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

<span data-ttu-id="063ed-152">Pour activer le module HTTP, ajoutez ce qui suit à votre fichier Web. config dans la section **System. webServer** :</span><span class="sxs-lookup"><span data-stu-id="063ed-152">To enable the HTTP module, add the following to your web.config file in the **system.webServer** section:</span></span>

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

<span data-ttu-id="063ed-153">Remplacez « YourAssemblyName » par le nom de l’assembly (sans l’extension « dll »).</span><span class="sxs-lookup"><span data-stu-id="063ed-153">Replace "YourAssemblyName" with the name of the assembly (not including the "dll" extension).</span></span>

<span data-ttu-id="063ed-154">Vous devez désactiver d’autres schémas d’authentification, tels que les formulaires ou l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="063ed-154">You should disable other authentication schemes, such as Forms or Windows auth.</span></span>
