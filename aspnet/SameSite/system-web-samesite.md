---
title: Utiliser des cookies SameSite dans ASP.NET
author: rick-anderson
description: Découvrez comment utiliser pour SameSite des cookies dans ASP.NET
ms.author: riande
ms.date: 2/15/2019
uid: samesite/system-web-samesite
ms.openlocfilehash: edb368910b24be2d042afe3c19ffa1fb23245443
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77455701"
---
# <a name="work-with-samesite-cookies-in-aspnet"></a><span data-ttu-id="14ed4-103">Utiliser des cookies SameSite dans ASP.NET</span><span class="sxs-lookup"><span data-stu-id="14ed4-103">Work with SameSite cookies in ASP.NET</span></span>

<span data-ttu-id="14ed4-104">De [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="14ed4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="14ed4-105">SameSite est un projet standard de l' [IETF](https://ietf.org/about/) conçu pour offrir une protection contre les attaques de falsification de requête intersites (CSRF).</span><span class="sxs-lookup"><span data-stu-id="14ed4-105">SameSite is an [IETF](https://ietf.org/about/) draft standard designed to provide some protection against cross-site request forgery (CSRF) attacks.</span></span> <span data-ttu-id="14ed4-106">Initialement rédigée dans [2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07), le brouillon standard a été mis à jour dans [2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00).</span><span class="sxs-lookup"><span data-stu-id="14ed4-106">Originally drafted in [2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07), the draft standard was updated in [2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00).</span></span> <span data-ttu-id="14ed4-107">La norme mise à jour n’est pas à compatibilité descendante avec la norme précédente, avec comme suit les différences les plus perceptibles :</span><span class="sxs-lookup"><span data-stu-id="14ed4-107">The updated standard is not backward compatible with the previous standard, with the following being the most noticeable differences:</span></span>

* <span data-ttu-id="14ed4-108">Les cookies sans en-tête SameSite sont traités comme `SameSite=Lax` par défaut.</span><span class="sxs-lookup"><span data-stu-id="14ed4-108">Cookies without SameSite header are treated as `SameSite=Lax` by default.</span></span>
* <span data-ttu-id="14ed4-109">`SameSite=None` doit être utilisé pour autoriser l’utilisation de cookies entre sites.</span><span class="sxs-lookup"><span data-stu-id="14ed4-109">`SameSite=None` must be used to allow cross-site cookie use.</span></span>
* <span data-ttu-id="14ed4-110">Les cookies qui déclarent `SameSite=None` doivent également être marqués comme `Secure`.</span><span class="sxs-lookup"><span data-stu-id="14ed4-110">Cookies that assert `SameSite=None` must also be marked as `Secure`.</span></span>
* <span data-ttu-id="14ed4-111">Les applications qui utilisent [`<iframe>`](https://developer.mozilla.org/docs/Web/HTML/Element/iframe) peuvent rencontrer des problèmes avec les cookies `sameSite=Lax` ou `sameSite=Strict`, car `<iframe>` est traité comme des scénarios intersites.</span><span class="sxs-lookup"><span data-stu-id="14ed4-111">Applications that use [`<iframe>`](https://developer.mozilla.org/docs/Web/HTML/Element/iframe) may experience issues with `sameSite=Lax` or `sameSite=Strict` cookies because `<iframe>` is treated as cross-site scenarios.</span></span>
* <span data-ttu-id="14ed4-112">La valeur `SameSite=None` n’est pas autorisée par la [norme 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07) et oblige certaines implémentations à traiter ces cookies comme des `SameSite=Strict`.</span><span class="sxs-lookup"><span data-stu-id="14ed4-112">The value `SameSite=None` is not allowed by the [2016 standard](https://tools.ietf.org/html/draft-west-first-party-cookies-07) and causes some implementations to treat such cookies as `SameSite=Strict`.</span></span> <span data-ttu-id="14ed4-113">Consultez [prise en charge des navigateurs plus anciens](#sob) dans ce document.</span><span class="sxs-lookup"><span data-stu-id="14ed4-113">See [Supporting older browsers](#sob) in this document.</span></span>

<span data-ttu-id="14ed4-114">Le paramètre `SameSite=Lax` fonctionne pour la plupart des cookies d’application.</span><span class="sxs-lookup"><span data-stu-id="14ed4-114">The `SameSite=Lax` setting works for most application cookies.</span></span> <span data-ttu-id="14ed4-115">Certaines formes d’authentification comme [OpenID Connect](https://openid.net/connect/) (OIDC) et [WS-Federation](https://auth0.com/docs/protocols/ws-fed) default pour la publication de redirections basées sur.</span><span class="sxs-lookup"><span data-stu-id="14ed4-115">Some forms of authentication like [OpenID Connect](https://openid.net/connect/) (OIDC) and [WS-Federation](https://auth0.com/docs/protocols/ws-fed) default to POST based redirects.</span></span> <span data-ttu-id="14ed4-116">Les redirections basées sur la publication déclenchent les protections du navigateur SameSite, donc SameSite est désactivé pour ces composants.</span><span class="sxs-lookup"><span data-stu-id="14ed4-116">The POST based redirects trigger the SameSite browser protections, so SameSite is disabled for these components.</span></span> <span data-ttu-id="14ed4-117">La plupart des connexions [OAuth](https://oauth.net/) ne sont pas affectées en raison de différences de flux de demande.</span><span class="sxs-lookup"><span data-stu-id="14ed4-117">Most [OAuth](https://oauth.net/) logins are not affected due to differences in how the request flows.</span></span>

<span data-ttu-id="14ed4-118">Chaque composant ASP.NET qui émet des cookies doit décider si SameSite est approprié.</span><span class="sxs-lookup"><span data-stu-id="14ed4-118">Each ASP.NET component that emits cookies needs to decide if SameSite is appropriate.</span></span>

<span data-ttu-id="14ed4-119">Consultez [problèmes connus liés](#known) aux problèmes d’applications après l’installation des mises à jour 2019 .net SameSite.</span><span class="sxs-lookup"><span data-stu-id="14ed4-119">See [Known Issues](#known) for problems with applications after installing the 2019 .Net SameSite updates.</span></span>

## <a name="using-samesite-in-aspnet-472-and-48"></a><span data-ttu-id="14ed4-120">Utilisation de SameSite dans ASP.NET 4.7.2 et 4,8</span><span class="sxs-lookup"><span data-stu-id="14ed4-120">Using SameSite in ASP.NET 4.7.2 and 4.8</span></span>

<span data-ttu-id="14ed4-121">.Net 4.7.2 et 4,8 prennent en charge [2019 Draft Standard](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00) pour SameSite depuis la publication des mises à jour en décembre 2019.</span><span class="sxs-lookup"><span data-stu-id="14ed4-121">.Net 4.7.2 and 4.8 supports the [2019 draft standard](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00) for SameSite since the release of updates in December 2019.</span></span> <span data-ttu-id="14ed4-122">Les développeurs sont en mesure de contrôler par programmation la valeur de l’en-tête SameSite à l’aide de la [propriété HttpCookie. SameSite](/dotnet/api/system.web.httpcookie.samesite#System_Web_HttpCookie_SameSite).</span><span class="sxs-lookup"><span data-stu-id="14ed4-122">Developers are able to programmatically control the value of the SameSite header using the [HttpCookie.SameSite property](/dotnet/api/system.web.httpcookie.samesite#System_Web_HttpCookie_SameSite).</span></span> <span data-ttu-id="14ed4-123">Si vous affectez à la propriété `SameSite` la valeur `Strict`, `Lax`ou `None`, ces valeurs sont écrites sur le réseau avec le cookie.</span><span class="sxs-lookup"><span data-stu-id="14ed4-123">Setting the `SameSite` property to `Strict`, `Lax`, or `None` results in those values being written on the network with the cookie.</span></span> <span data-ttu-id="14ed4-124">La valeur `(SameSiteMode)(-1)` indique qu’aucun en-tête SameSite ne doit être inclus sur le réseau avec le cookie.</span><span class="sxs-lookup"><span data-stu-id="14ed4-124">Setting it equal to `(SameSiteMode)(-1)` indicates that no SameSite header should be included on the network with the cookie.</span></span> <span data-ttu-id="14ed4-125">La [propriété HttpCookie. Secure](/dotnet/api/system.web.httpcookie.secure), ou’RequireSSL’dans les fichiers de configuration, peut être utilisée pour marquer le cookie comme `Secure` ou non.</span><span class="sxs-lookup"><span data-stu-id="14ed4-125">The [HttpCookie.Secure Property](/dotnet/api/system.web.httpcookie.secure), or 'requireSSL' in config files, can be used to mark the cookie as `Secure` or not.</span></span>

<span data-ttu-id="14ed4-126">Par défaut, les nouvelles instances de `HttpCookie` sont `SameSite=(SameSiteMode)(-1)` et `Secure=false`.</span><span class="sxs-lookup"><span data-stu-id="14ed4-126">New `HttpCookie` instances will default to `SameSite=(SameSiteMode)(-1)` and `Secure=false`.</span></span> <span data-ttu-id="14ed4-127">Ces valeurs par défaut peuvent être remplacées dans la section de configuration `system.web/httpCookies`, où la chaîne `"Unspecified"` est une syntaxe conviviale de configuration uniquement pour `(SameSiteMode)(-1)`:</span><span class="sxs-lookup"><span data-stu-id="14ed4-127">These defaults can be overridden in the `system.web/httpCookies` configuration section, where the string `"Unspecified"` is a friendly configuration-only syntax for `(SameSiteMode)(-1)`:</span></span>

```xml
<configuration>
 <system.web>
  <httpCookies sameSite="[Strict|Lax|None|Unspecified]" requireSSL="[true|false]" />
 <system.web>
<configuration>
```

<span data-ttu-id="14ed4-128">ASP.Net émet également quatre cookies spécifiques propres à ces fonctionnalités : authentification anonyme, authentification par formulaire, état de session et gestion des rôles.</span><span class="sxs-lookup"><span data-stu-id="14ed4-128">ASP.Net also issues four specific cookies of its own for these features: Anonymous Authentication, Forms Authentication, Session State, and Role Management.</span></span> <span data-ttu-id="14ed4-129">Les instances de ces cookies obtenues dans le runtime peuvent être manipulées à l’aide des propriétés `SameSite` et `Secure` comme n’importe quelle autre instance HttpCookie.</span><span class="sxs-lookup"><span data-stu-id="14ed4-129">Instances of these cookies obtained in runtime can be manipulated using the `SameSite` and `Secure` properties just like any other HttpCookie instance.</span></span> <span data-ttu-id="14ed4-130">Toutefois, en raison de l’émergence du patchwork de la norme SameSite, les options de configuration de ces quatre fonctionnalités sont incohérentes.</span><span class="sxs-lookup"><span data-stu-id="14ed4-130">However, due to the patchwork emergence of the SameSite standard, configuration options for these four features cookies is inconsistent.</span></span> <span data-ttu-id="14ed4-131">Les sections de configuration et attributs appropriés, avec les valeurs par défaut, sont indiqués ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="14ed4-131">The relevant configuration sections and attributes, with defaults, are shown below.</span></span> <span data-ttu-id="14ed4-132">S’il n’existe aucun `SameSite` ou `Secure` attribut associé pour une fonctionnalité, la fonctionnalité est redéfinie sur les valeurs par défaut configurées dans la section `system.web/httpCookies` présentée ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="14ed4-132">If there is no `SameSite` or `Secure` related attribute for a feature, then the feature will fall back on the defaults configured in the `system.web/httpCookies` section discussed above.</span></span>

```xml
<configuration>
 <system.web>
  <anonymousIdentification cookieRequireSSL="false" /> <!-- No config attribute for SameSite -->
  <authentication>
   <forms cookieSameSite="Lax" requireSSL="false" />
  </authentication>
  <sessionState cookieSameSite="Lax" /> <!-- No config attribute for Secure -->
  <roleManager cookieRequireSSL="false" /> <!-- No config attribute for SameSite -->
 <system.web>
<configuration>
```  

<span data-ttu-id="14ed4-133">**Remarque**: « non spécifié » n’est disponible que pour `system.web/httpCookies@sameSite` pour le moment.</span><span class="sxs-lookup"><span data-stu-id="14ed4-133">**Note**: 'Unspecified' is only available to `system.web/httpCookies@sameSite` at the moment.</span></span> <span data-ttu-id="14ed4-134">Nous espérons ajouter une syntaxe similaire aux attributs cookieSameSite précédemment affichés dans les futures mises à jour.</span><span class="sxs-lookup"><span data-stu-id="14ed4-134">We hope to add similar syntax to the previously shown cookieSameSite attributes in future updates.</span></span> <span data-ttu-id="14ed4-135">La définition de `(SameSiteMode)(-1)` dans le code fonctionne toujours sur les instances de ces cookies. \*</span><span class="sxs-lookup"><span data-stu-id="14ed4-135">Setting `(SameSiteMode)(-1)` in code still works on instances of these cookies.\*</span></span>

[!INCLUDE[](~/includes/MTcomments.md)]

<a name="retargeting"></a>

### <a name="retarget-net-apps"></a><span data-ttu-id="14ed4-136">Recibler des applications .NET</span><span class="sxs-lookup"><span data-stu-id="14ed4-136">Retarget .NET apps</span></span>

<span data-ttu-id="14ed4-137">Pour cibler .NET 4.7.2 ou version ultérieure :</span><span class="sxs-lookup"><span data-stu-id="14ed4-137">To target .NET 4.7.2 or later:</span></span>

* <span data-ttu-id="14ed4-138">Vérifiez que le *fichier Web. config* contient les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="14ed4-138">Ensure *web.config* contains the following:</span></span>  <!-- review, I removed `debug="true"` -->

  ```xml
  <system.web>
    <compilation targetFramework="4.7.2"/>
    <httpRuntime targetFramework="4.7.2"/>
  </system.web>

* Verify the project file contains the correct [TargetFrameworkVersion](/visualstudio/msbuild/msbuild-target-framework-and-target-platform?view=vs-2019):

  ```xml
  <TargetFrameworkVersion>v4.7.2</TargetFrameworkVersion>
  ```

  <span data-ttu-id="14ed4-139">Le [Guide de migration de .net](/dotnet/framework/migration-guide/) offre plus de détails.</span><span class="sxs-lookup"><span data-stu-id="14ed4-139">The [.NET Migration Guide](/dotnet/framework/migration-guide/) has more details.</span></span>

* <span data-ttu-id="14ed4-140">Vérifiez que les packages NuGet dans le projet sont ciblés sur la version de Framework appropriée.</span><span class="sxs-lookup"><span data-stu-id="14ed4-140">Verify NuGet packages in the project are targeted at the correct framework version.</span></span> <span data-ttu-id="14ed4-141">Vous pouvez vérifier la version du Framework appropriée en examinant le fichier *packages. config* , par exemple :</span><span class="sxs-lookup"><span data-stu-id="14ed4-141">You can verify the correct framework version by examining the *packages.config* file, for example:</span></span>

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <packages>
    <package id="Microsoft.AspNet.Mvc" version="5.2.7" targetFramework="net472" />
    <package id="Microsoft.ApplicationInsights" version="2.4.0" targetFramework="net451" />
  </packages>
  ```

  <span data-ttu-id="14ed4-142">Dans le fichier *packages. config* précédent, le package `Microsoft.ApplicationInsights` :</span><span class="sxs-lookup"><span data-stu-id="14ed4-142">In the preceding *packages.config* file, the `Microsoft.ApplicationInsights` package:</span></span>
    * <span data-ttu-id="14ed4-143">Est ciblé par rapport à .NET 4.5.1.</span><span class="sxs-lookup"><span data-stu-id="14ed4-143">Is  targeted against .NET 4.5.1.</span></span>
    * <span data-ttu-id="14ed4-144">L’attribut `targetFramework` doit être mis à jour pour `net472` si un package mis à jour ciblant votre cible de Framework existe.</span><span class="sxs-lookup"><span data-stu-id="14ed4-144">Should have its `targetFramework` attribute updated to `net472` if an updated package targeting your framework target exists.</span></span>

<a name="nope"></a>

### <a name="net-versions-earlier-than-472"></a><span data-ttu-id="14ed4-145">Versions de .NET antérieures à 4.7.2</span><span class="sxs-lookup"><span data-stu-id="14ed4-145">.NET versions earlier than 4.7.2</span></span>

<span data-ttu-id="14ed4-146">Microsoft ne prend pas en charge les versions .NET inférieures à ce 4.7.2 pour l’écriture de l’attribut de cookie du même site.</span><span class="sxs-lookup"><span data-stu-id="14ed4-146">Microsoft does not support .NET versions lower that 4.7.2 for writing the same-site cookie attribute.</span></span> <span data-ttu-id="14ed4-147">Nous n’avons pas trouvé de méthode fiable pour :</span><span class="sxs-lookup"><span data-stu-id="14ed4-147">We have not found a reliable way to:</span></span>

* <span data-ttu-id="14ed4-148">Vérifiez que l’attribut est écrit correctement en fonction de la version du navigateur.</span><span class="sxs-lookup"><span data-stu-id="14ed4-148">Ensure the attribute is written correctly based on browser version.</span></span>
* <span data-ttu-id="14ed4-149">Interceptez et ajustez l’authentification et les cookies de session sur les versions antérieures du Framework.</span><span class="sxs-lookup"><span data-stu-id="14ed4-149">Intercept and adjust authentication and session cookies on older framework versions.</span></span>

### <a name="december-patch-behavior-changes"></a><span data-ttu-id="14ed4-150">Modifications du comportement du correctif décembre</span><span class="sxs-lookup"><span data-stu-id="14ed4-150">December patch behavior changes</span></span>

<span data-ttu-id="14ed4-151">La modification de comportement spécifique pour .NET Framework est la manière dont la propriété `SameSite` interprète la valeur `None` :</span><span class="sxs-lookup"><span data-stu-id="14ed4-151">The specific behavior change for .NET Framework is how the `SameSite` property interprets the `None` value:</span></span>

* <span data-ttu-id="14ed4-152">Avant que le correctif ait une valeur de `None` signifiait :</span><span class="sxs-lookup"><span data-stu-id="14ed4-152">Before the patch a value of `None` meant:</span></span>
  * <span data-ttu-id="14ed4-153">N’émettez pas l’attribut du tout.</span><span class="sxs-lookup"><span data-stu-id="14ed4-153">Do not emit the attribute at all.</span></span>
* <span data-ttu-id="14ed4-154">Après le correctif :</span><span class="sxs-lookup"><span data-stu-id="14ed4-154">After the patch:</span></span>
  * <span data-ttu-id="14ed4-155">La valeur `None`signifie « émettre l’attribut avec une valeur de `None`».</span><span class="sxs-lookup"><span data-stu-id="14ed4-155">A value of `None`it means "Emit the attribute with a value of `None`".</span></span>
  * <span data-ttu-id="14ed4-156">Une `SameSite` valeur de `(SameSiteMode)(-1)` entraîne la non-émission de l’attribut.</span><span class="sxs-lookup"><span data-stu-id="14ed4-156">A `SameSite` value of `(SameSiteMode)(-1)` causes the attribute not to be emitted.</span></span>

<span data-ttu-id="14ed4-157">La valeur SameSite par défaut pour les cookies d’authentification par formulaire et d’état de session a été modifiée de `None` à `Lax`.</span><span class="sxs-lookup"><span data-stu-id="14ed4-157">The default SameSite value for forms authentication and session state cookies was changed from `None` to `Lax`.</span></span>

### <a name="summary-of-change-impact-on-browsers"></a><span data-ttu-id="14ed4-158">Résumé de l’impact des modifications sur les navigateurs</span><span class="sxs-lookup"><span data-stu-id="14ed4-158">Summary of change impact on browsers</span></span>

<span data-ttu-id="14ed4-159">Si vous installez le correctif et émettez un cookie avec `SameSite.None`, l’une des deux situations suivantes se produit :</span><span class="sxs-lookup"><span data-stu-id="14ed4-159">If you install the patch and issue a cookie with `SameSite.None`, one of two things will happen:</span></span>
* <span data-ttu-id="14ed4-160">Chrome V80 traite ce cookie en fonction de la nouvelle implémentation et n’applique pas les mêmes restrictions de site sur le cookie.</span><span class="sxs-lookup"><span data-stu-id="14ed4-160">Chrome v80 will treat this cookie according to the new implementation, and not enforce same site restrictions on the cookie.</span></span>
* <span data-ttu-id="14ed4-161">Tout navigateur qui n’a pas été mis à jour pour prendre en charge la nouvelle implémentation suivra l’ancienne implémentation.</span><span class="sxs-lookup"><span data-stu-id="14ed4-161">Any browser that has not been updated to support the new implementation will follow the old implementation.</span></span> <span data-ttu-id="14ed4-162">L’ancienne implémentation indique :</span><span class="sxs-lookup"><span data-stu-id="14ed4-162">The old implementation says:</span></span>
  * <span data-ttu-id="14ed4-163">Si vous voyez une valeur que vous ne comprenez pas, ignorez-la et basculez vers les mêmes restrictions de site.</span><span class="sxs-lookup"><span data-stu-id="14ed4-163">If you see a value you don't understand, ignore it and switch to strict same site restrictions.</span></span>

<span data-ttu-id="14ed4-164">Par conséquent, l’application s’arrête en mode chrome, ou vous pouvez interrompre de nombreux autres emplacements.</span><span class="sxs-lookup"><span data-stu-id="14ed4-164">So either the app breaks in Chrome, or you break in numerous other places.</span></span>

## <a name="history-and-changes"></a><span data-ttu-id="14ed4-165">Historique et modifications</span><span class="sxs-lookup"><span data-stu-id="14ed4-165">History and changes</span></span>

<span data-ttu-id="14ed4-166">La prise en charge de SameSite a été implémentée pour la première fois dans .NET 4.7.2 à l’aide de la [norme draft 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1).</span><span class="sxs-lookup"><span data-stu-id="14ed4-166">SameSite support was first implemented in .NET 4.7.2 using the [2016 draft standard](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1).</span></span>

<span data-ttu-id="14ed4-167">Le 19 novembre 2019 mises à jour pour Windows a mis à jour .NET 4.7.2 + de la norme 2016 à la norme 2019.</span><span class="sxs-lookup"><span data-stu-id="14ed4-167">The November 19, 2019 updates for Windows updated .NET 4.7.2+ from the 2016 standard to the 2019 standard.</span></span> <span data-ttu-id="14ed4-168">Des mises à jour supplémentaires sont à venir pour d’autres versions de Windows.</span><span class="sxs-lookup"><span data-stu-id="14ed4-168">Additional updates are forthcoming for other versions of Windows.</span></span> <span data-ttu-id="14ed4-169">Pour plus d’informations, consultez <xref:samesite/kbs-samesite>.</span><span class="sxs-lookup"><span data-stu-id="14ed4-169">For more information, see <xref:samesite/kbs-samesite>.</span></span>

 <span data-ttu-id="14ed4-170">Le brouillon 2019 de la spécification SameSite :</span><span class="sxs-lookup"><span data-stu-id="14ed4-170">The 2019 draft of the SameSite specification:</span></span>

* <span data-ttu-id="14ed4-171">N’est **pas** à compatibilité descendante avec le brouillon 2016.</span><span class="sxs-lookup"><span data-stu-id="14ed4-171">Is **not** backwards compatible with the 2016 draft.</span></span> <span data-ttu-id="14ed4-172">Pour plus d’informations, consultez [prise en charge des navigateurs plus anciens](#sob) dans ce document.</span><span class="sxs-lookup"><span data-stu-id="14ed4-172">For more information, see [Supporting older browsers](#sob) in this document.</span></span>
* <span data-ttu-id="14ed4-173">Spécifie que les cookies sont traités comme `SameSite=Lax` par défaut.</span><span class="sxs-lookup"><span data-stu-id="14ed4-173">Specifies cookies are treated as `SameSite=Lax` by default.</span></span>
* <span data-ttu-id="14ed4-174">Spécifie les cookies qui déclarent explicitement `SameSite=None` pour permettre la remise entre sites doivent également être marqués comme `Secure`.</span><span class="sxs-lookup"><span data-stu-id="14ed4-174">Specifies cookies that explicitly assert `SameSite=None` in order to enable cross-site delivery should also be marked as `Secure`.</span></span>
* <span data-ttu-id="14ed4-175">Est pris en charge par les correctifs émis comme indiqué dans les Articles de la base de connaissances répertoriés ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="14ed4-175">Is supported by patches issued as described in the KB's listed above.</span></span>
* <span data-ttu-id="14ed4-176">Est planifié pour être activé par [chrome](https://chromestatus.com/feature/5088147346030592) par défaut au [2020 février](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html).</span><span class="sxs-lookup"><span data-stu-id="14ed4-176">Is scheduled to be enabled by [Chrome](https://chromestatus.com/feature/5088147346030592) by default in [Feb 2020](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html).</span></span> <span data-ttu-id="14ed4-177">Les navigateurs ont commencé à passer à cette norme dans 2019.</span><span class="sxs-lookup"><span data-stu-id="14ed4-177">Browsers started moving to this standard in 2019.</span></span>

<a name="known"><a/>

## <a name="known-issues"></a><span data-ttu-id="14ed4-178">Problèmes connus</span><span class="sxs-lookup"><span data-stu-id="14ed4-178">Known Issues</span></span>

<span data-ttu-id="14ed4-179">Étant donné que les spécifications de brouillon 2016 et 2019 ne sont pas compatibles, la mise à jour du .NET Framework de novembre 2019 introduit des modifications susceptibles d’être endommagées.</span><span class="sxs-lookup"><span data-stu-id="14ed4-179">Because the 2016 and 2019 draft specifications are not compatible, the November 2019 .Net Framework update introduces some changes that may be breaking.</span></span>

* <span data-ttu-id="14ed4-180">L’état de session et les cookies d’authentification par formulaire sont désormais écrits sur le réseau en tant que `Lax` au lieu d’être non spécifiés.</span><span class="sxs-lookup"><span data-stu-id="14ed4-180">Session State and Forms Authentication cookies are now written to the network as `Lax` instead of unspecified.</span></span>
  * <span data-ttu-id="14ed4-181">Tandis que la plupart des applications fonctionnent avec des cookies `SameSite=Lax`, les applications qui sont publiées sur les sites ou les applications qui utilisent des `iframe` peuvent constater que leurs cookies d’état de session ou d’autorisation de formulaires ne sont pas utilisés comme prévu.</span><span class="sxs-lookup"><span data-stu-id="14ed4-181">While most apps work with `SameSite=Lax` cookies, apps that POST across sites or applications that make use of `iframe` may find that their session state or forms authorization cookies aren't being used as expected.</span></span> <span data-ttu-id="14ed4-182">Pour remédier à cela, modifiez la valeur `cookieSameSite` dans la section de configuration appropriée, comme indiqué précédemment.</span><span class="sxs-lookup"><span data-stu-id="14ed4-182">To remedy this, change the `cookieSameSite` value in the appropriate configuration section as discussed previously.</span></span>
* <span data-ttu-id="14ed4-183">Les HttpCookies qui définissent explicitement `SameSite=None` dans le code ou la configuration ont maintenant cette valeur écrite avec le cookie, alors qu’elle a été précédemment omise.</span><span class="sxs-lookup"><span data-stu-id="14ed4-183">HttpCookies that explicitly set `SameSite=None` in code or configuration now have that value written with the cookie, whereas it was previously omitted.</span></span> <span data-ttu-id="14ed4-184">Cela peut entraîner des problèmes avec les navigateurs plus anciens qui prennent uniquement en charge le brouillon 2016 standard.</span><span class="sxs-lookup"><span data-stu-id="14ed4-184">This may cause issues with older browsers that only support the 2016 draft standard.</span></span>
  * <span data-ttu-id="14ed4-185">Lorsque vous ciblez des navigateurs prenant en charge le brouillon 2019 avec `SameSite=None` cookies, n’oubliez pas de les marquer également `Secure` ou s’ils ne sont pas reconnus.</span><span class="sxs-lookup"><span data-stu-id="14ed4-185">When targeting browsers supporting the 2019 draft standard with `SameSite=None` cookies, remember to also mark them `Secure` or they may not be recognized.</span></span>
  * <span data-ttu-id="14ed4-186">Pour rétablir le comportement 2016 qui consiste à ne pas écrire `SameSite=None`, utilisez le paramètre d’application `aspnet:SupressSameSiteNone=true`.</span><span class="sxs-lookup"><span data-stu-id="14ed4-186">To revert to the 2016 behavior of not writing `SameSite=None`, use the app setting `aspnet:SupressSameSiteNone=true`.</span></span> <span data-ttu-id="14ed4-187">Notez que cela s’applique à tous les HttpCookies de l’application.</span><span class="sxs-lookup"><span data-stu-id="14ed4-187">Note that this will apply to all HttpCookies in the app.</span></span>

### <a name="azure-app-servicesamesite-cookie-handling"></a><span data-ttu-id="14ed4-188">Azure App Service : gestion des cookies SameSite</span><span class="sxs-lookup"><span data-stu-id="14ed4-188">Azure App Service—SameSite cookie handling</span></span>

<span data-ttu-id="14ed4-189">Pour plus d’informations sur Azure App Service la configuration des comportements SameSite dans les applications .net 4.7.2, consultez [Azure App service, gestion des cookies SameSite et .NET Framework correctif 4.7.2](https://azure.microsoft.com/updates/app-service-samesite-cookie-update/) .</span><span class="sxs-lookup"><span data-stu-id="14ed4-189">See [Azure App Service—SameSite cookie handling and .NET Framework 4.7.2 patch](https://azure.microsoft.com/updates/app-service-samesite-cookie-update/) for information about how Azure App Service is configuring SameSite behaviors in .Net 4.7.2 apps.</span></span>

<a name="sob"></a>

## <a name="supporting-older-browsers"></a><span data-ttu-id="14ed4-190">Prise en charge des navigateurs plus anciens</span><span class="sxs-lookup"><span data-stu-id="14ed4-190">Supporting older browsers</span></span>

<span data-ttu-id="14ed4-191">La norme 2016 SameSite stipule que les valeurs inconnues doivent être traitées comme des valeurs `SameSite=Strict`.</span><span class="sxs-lookup"><span data-stu-id="14ed4-191">The 2016 SameSite standard mandated that unknown values must be treated as `SameSite=Strict` values.</span></span> <span data-ttu-id="14ed4-192">Les applications accessibles depuis des navigateurs plus anciens qui prennent en charge la norme 2016 SameSite peuvent s’arrêter lorsqu’ils obtiennent une propriété SameSite avec la valeur `None`.</span><span class="sxs-lookup"><span data-stu-id="14ed4-192">Apps accessed from older browsers which support the 2016 SameSite standard may break when they get a SameSite property with a value of `None`.</span></span> <span data-ttu-id="14ed4-193">Les applications Web doivent implémenter la détection du navigateur si elles envisagent de prendre en charge des navigateurs plus anciens.</span><span class="sxs-lookup"><span data-stu-id="14ed4-193">Web apps must implement browser detection if they intend to support older browsers.</span></span> <span data-ttu-id="14ed4-194">ASP.NET n’implémente pas la détection du navigateur, car les valeurs des agents utilisateur sont très volatiles et changent fréquemment.</span><span class="sxs-lookup"><span data-stu-id="14ed4-194">ASP.NET doesn't implement browser detection because User-Agents values are highly volatile and change frequently.</span></span>

<span data-ttu-id="14ed4-195">L’approche de Microsoft pour résoudre le problème est de vous aider à implémenter des composants de détection de navigateur pour supprimer l’attribut `sameSite=None` des cookies si un navigateur est connu pour ne pas le prendre en charge.</span><span class="sxs-lookup"><span data-stu-id="14ed4-195">Microsoft's approach to fixing the problem is to help you implement browser detection components to strip the `sameSite=None` attribute from cookies if a browser is known to not support it.</span></span> <span data-ttu-id="14ed4-196">Le Conseil de Google était d’émettre des cookies doubles, l’un avec le nouvel attribut et l’autre sans l’attribut.</span><span class="sxs-lookup"><span data-stu-id="14ed4-196">Google's advice was to issue double cookies, one with the new attribute, and one without the attribute at all.</span></span> <span data-ttu-id="14ed4-197">Toutefois, nous considérons le Conseil de Google limité.</span><span class="sxs-lookup"><span data-stu-id="14ed4-197">However we consider Google's advice limited.</span></span> <span data-ttu-id="14ed4-198">Certains navigateurs, en particulier les navigateurs mobiles, ont des limites très réduites sur le nombre de cookies d’un site, ou un nom de domaine peut envoyer.</span><span class="sxs-lookup"><span data-stu-id="14ed4-198">Some browsers, especially mobile browsers have very small limits on the number of cookies a site, or a domain name can send.</span></span> <span data-ttu-id="14ed4-199">L’envoi de plusieurs cookies, en particulier les cookies volumineux tels que les cookies d’authentification, peut atteindre la limite du navigateur mobile très rapidement, provoquant des échecs d’applications difficiles à diagnostiquer et à résoudre.</span><span class="sxs-lookup"><span data-stu-id="14ed4-199">Sending multiple cookies, especially large cookies like authentication cookies can reach the mobile browser limit very quickly, causing app failures that are hard to diagnose and fix.</span></span> <span data-ttu-id="14ed4-200">En outre, il existe un grand écosystème de code et de composants tiers qui peut ne pas être mis à jour pour utiliser une approche de type double cookie.</span><span class="sxs-lookup"><span data-stu-id="14ed4-200">Furthermore as a framework there is a large ecosystem of third party code and components that may not be updated to use a double cookie approach.</span></span>

<span data-ttu-id="14ed4-201">Le code de détection du navigateur utilisé dans les exemples de projets de [ce référentiel GitHub]() est contenu dans deux fichiers</span><span class="sxs-lookup"><span data-stu-id="14ed4-201">The browser detection code used in the sample projects in [this GitHub repository]() is contained in two files</span></span>

* [<span data-ttu-id="14ed4-202">C#SameSiteSupport.cs</span><span class="sxs-lookup"><span data-stu-id="14ed4-202">C# SameSiteSupport.cs</span></span>](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/SameSiteSupport.cs)
* [<span data-ttu-id="14ed4-203">VB SameSiteSupport. vb</span><span class="sxs-lookup"><span data-stu-id="14ed4-203">VB SameSiteSupport.vb</span></span>](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/SameSiteSupport.vb)

<span data-ttu-id="14ed4-204">Ces détections sont les agents de navigateur les plus courants qui prennent en charge la norme 2016 et pour lesquels l’attribut doit être complètement supprimé.</span><span class="sxs-lookup"><span data-stu-id="14ed4-204">These detections are the most common browser agents we have seen that support the 2016 standard and for which the attribute needs to be completely removed.</span></span> <span data-ttu-id="14ed4-205">Il n’est pas conçu comme une implémentation complète :</span><span class="sxs-lookup"><span data-stu-id="14ed4-205">It isn't meant as a complete implementation:</span></span>

* <span data-ttu-id="14ed4-206">Votre application peut voir les navigateurs que nos sites de test n’ont pas.</span><span class="sxs-lookup"><span data-stu-id="14ed4-206">Your app may see browsers that our test sites do not.</span></span>
* <span data-ttu-id="14ed4-207">Vous devez être prêt à ajouter des détections si nécessaire pour votre environnement.</span><span class="sxs-lookup"><span data-stu-id="14ed4-207">You should be prepared to add detections as necessary for your environment.</span></span>

<span data-ttu-id="14ed4-208">Le mode de mise en place de la détection dépend de la version de .NET et de l’infrastructure Web que vous utilisez.</span><span class="sxs-lookup"><span data-stu-id="14ed4-208">How you wire up the detection varies according the version of .NET and the web framework that you are using.</span></span> <span data-ttu-id="14ed4-209">Le code suivant peut être appelé au <xref:HTTP.HttpCookie> site d’appel :</span><span class="sxs-lookup"><span data-stu-id="14ed4-209">The following code can be called at the <xref:HTTP.HttpCookie> call site:</span></span>

[!code-csharp[](sample/SameSiteCheck.cs?name=snippet)]

<span data-ttu-id="14ed4-210">Consultez les rubriques suivantes sur les cookies ASP.NET 4.7.2 SameSite :</span><span class="sxs-lookup"><span data-stu-id="14ed4-210">See the following ASP.NET 4.7.2 SameSite cookie topics:</span></span>

* [<span data-ttu-id="14ed4-211">C#MVC</span><span class="sxs-lookup"><span data-stu-id="14ed4-211">C# MVC</span></span>](xref:samesite/csMVC)
* [<span data-ttu-id="14ed4-212">C#WebForms</span><span class="sxs-lookup"><span data-stu-id="14ed4-212">C# WebForms</span></span>](xref:samesite/CSharpWebForms)
* [<span data-ttu-id="14ed4-213">WebForms VB</span><span class="sxs-lookup"><span data-stu-id="14ed4-213">VB WebForms</span></span>](xref:samesite/vbWF)
* [<span data-ttu-id="14ed4-214">MVC VB</span><span class="sxs-lookup"><span data-stu-id="14ed4-214">VB MVC</span></span>](xref:samesite/vbMVC)
<!--
* <xref:samesite/csMVC>
* <xref:samesite/CSharpWebForms>
* <xref:samesite/vbWF>
* <xref:samesite/vbMVC>
-->

### <a name="ensuring-your-site-redirects-to-https"></a><span data-ttu-id="14ed4-215">Vérification de la redirection de votre site vers HTTPs</span><span class="sxs-lookup"><span data-stu-id="14ed4-215">Ensuring your site redirects to HTTPS</span></span>

<span data-ttu-id="14ed4-216">Pour ASP.NET 4. x, WebForms et MVC, [la fonctionnalité de réécriture d’URL d’IIS](/iis/extensions/url-rewrite-module/creating-rewrite-rules-for-the-url-rewrite-module) peut être utilisée pour rediriger toutes les requêtes vers HTTPS.</span><span class="sxs-lookup"><span data-stu-id="14ed4-216">For ASP.NET 4.x, WebForms and MVC, [IIS's URL Rewrite](/iis/extensions/url-rewrite-module/creating-rewrite-rules-for-the-url-rewrite-module) feature can be used to redirect all requests to HTTPS.</span></span> <span data-ttu-id="14ed4-217">Le code XML suivant montre un exemple de règle :</span><span class="sxs-lookup"><span data-stu-id="14ed4-217">The following XML shows a sample rule:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <system.webServer>
    <rewrite>
      <rules>
        <rule name="Redirect to https" stopProcessing="true">
          <match url="(.*)"/>
          <conditions>
            <add input="{HTTPS}" pattern="Off"/>
            <add input="{REQUEST_METHOD}" pattern="^get$|^head$" />
          </conditions>
          <action type="Redirect" url="https://{HTTP_HOST}/{R:1}" redirectType="Permanent"/>
        </rule>
      </rules>
    </rewrite>
  </system.webServer>
</configuration>
```

<span data-ttu-id="14ed4-218">Dans les installations locales de la [réécriture d’URL IIS](https://www.iis.net/downloads/microsoft/url-rewrite) , il s’agit d’une fonctionnalité facultative qui peut nécessiter l’installation de.</span><span class="sxs-lookup"><span data-stu-id="14ed4-218">In on-premises installations of [IIS URL Rewrite](https://www.iis.net/downloads/microsoft/url-rewrite) is an optional feature that may need installing.</span></span>

## <a name="test-apps-for-samesite-problems"></a><span data-ttu-id="14ed4-219">Tester des applications pour les problèmes SameSite</span><span class="sxs-lookup"><span data-stu-id="14ed4-219">Test apps for SameSite problems</span></span>

<span data-ttu-id="14ed4-220">Vous devez tester votre application avec les navigateurs que vous prenez en charge et parcourir vos scénarios qui impliquent des cookies.</span><span class="sxs-lookup"><span data-stu-id="14ed4-220">You must test your app with the browsers you support and go through your scenarios that involve cookies.</span></span> <span data-ttu-id="14ed4-221">Les scénarios de cookies impliquent généralement</span><span class="sxs-lookup"><span data-stu-id="14ed4-221">Cookie scenarios typically involve</span></span>

* <span data-ttu-id="14ed4-222">Formulaires de connexion</span><span class="sxs-lookup"><span data-stu-id="14ed4-222">Login forms</span></span>
* <span data-ttu-id="14ed4-223">Mécanismes de connexion externes tels que Facebook, Azure AD, OAuth et OIDC</span><span class="sxs-lookup"><span data-stu-id="14ed4-223">External login mechanisms such as Facebook, Azure AD, OAuth and OIDC</span></span>
* <span data-ttu-id="14ed4-224">Pages acceptant les demandes d’autres sites</span><span class="sxs-lookup"><span data-stu-id="14ed4-224">Pages that accept requests from other sites</span></span>
* <span data-ttu-id="14ed4-225">Pages de votre application conçues pour être incorporées dans des iframes</span><span class="sxs-lookup"><span data-stu-id="14ed4-225">Pages in your app designed to be embedded in iframes</span></span>

<span data-ttu-id="14ed4-226">Vous devez vérifier que les cookies sont créés, conservés et supprimés correctement dans votre application.</span><span class="sxs-lookup"><span data-stu-id="14ed4-226">You should check that cookies are created, persisted and deleted correctly in your app.</span></span>

<span data-ttu-id="14ed4-227">Les applications qui interagissent avec des sites distants, par exemple via une connexion tierce, doivent :</span><span class="sxs-lookup"><span data-stu-id="14ed4-227">Apps that interact with remote sites such as through third-party login need to:</span></span>

* <span data-ttu-id="14ed4-228">Testez l’interaction sur plusieurs navigateurs.</span><span class="sxs-lookup"><span data-stu-id="14ed4-228">Test the interaction on multiple browsers.</span></span>
* <span data-ttu-id="14ed4-229">Appliquez la [détection et l’atténuation du navigateur](#sob) abordées dans ce document.</span><span class="sxs-lookup"><span data-stu-id="14ed4-229">Apply the [browser detection and mitigation](#sob) discussed in this document.</span></span>

<span data-ttu-id="14ed4-230">Testez les applications Web à l’aide d’une version du client qui peut s’abonner au nouveau comportement SameSite.</span><span class="sxs-lookup"><span data-stu-id="14ed4-230">Test web apps using a client version that can opt-in to the new SameSite behavior.</span></span> <span data-ttu-id="14ed4-231">Chrome, Firefox et chrome Edge ont tous des indicateurs de fonctionnalités d’abonnement qui peuvent être utilisés à des fins de test.</span><span class="sxs-lookup"><span data-stu-id="14ed4-231">Chrome, Firefox, and Chromium Edge all have new opt-in feature flags that can be used for testing.</span></span> <span data-ttu-id="14ed4-232">Une fois que votre application applique les correctifs SameSite, testez-la avec des versions client plus anciennes, en particulier Safari.</span><span class="sxs-lookup"><span data-stu-id="14ed4-232">After your app applies the SameSite patches, test it with older client versions, especially Safari.</span></span> <span data-ttu-id="14ed4-233">Pour plus d’informations, consultez [prise en charge des navigateurs plus anciens](#sob) dans ce document.</span><span class="sxs-lookup"><span data-stu-id="14ed4-233">For more information, see [Supporting older browsers](#sob) in this document.</span></span>

### <a name="test-with-chrome"></a><span data-ttu-id="14ed4-234">Tester avec chrome</span><span class="sxs-lookup"><span data-stu-id="14ed4-234">Test with Chrome</span></span>

<span data-ttu-id="14ed4-235">Chrome 78 + donne des résultats trompeurs, car une atténuation temporaire est en place.</span><span class="sxs-lookup"><span data-stu-id="14ed4-235">Chrome 78+ gives misleading results because it has a temporary mitigation in place.</span></span> <span data-ttu-id="14ed4-236">Le chrome 78 + atténuation temporaire autorise les cookies datant de moins de deux minutes.</span><span class="sxs-lookup"><span data-stu-id="14ed4-236">The Chrome 78+ temporary mitigation allows cookies less than two minutes old.</span></span> <span data-ttu-id="14ed4-237">Le chrome 76 ou 77 avec les indicateurs de test appropriés activés fournit des résultats plus précis.</span><span class="sxs-lookup"><span data-stu-id="14ed4-237">Chrome 76 or 77 with the appropriate test flags enabled provides more accurate results.</span></span> <span data-ttu-id="14ed4-238">Pour tester le nouveau comportement SameSite, basculez `chrome://flags/#same-site-by-default-cookies` sur **activé**.</span><span class="sxs-lookup"><span data-stu-id="14ed4-238">To test the new SameSite behavior toggle `chrome://flags/#same-site-by-default-cookies` to **Enabled**.</span></span> <span data-ttu-id="14ed4-239">Les anciennes versions de chrome (75 et versions antérieures) sont signalées pour échouer avec le nouveau paramètre de `None`.</span><span class="sxs-lookup"><span data-stu-id="14ed4-239">Older versions of Chrome (75 and below) are reported to fail with the new `None` setting.</span></span> <span data-ttu-id="14ed4-240">Consultez [prise en charge des navigateurs plus anciens](#sob) dans ce document.</span><span class="sxs-lookup"><span data-stu-id="14ed4-240">See [Supporting older browsers](#sob) in this document.</span></span>

<span data-ttu-id="14ed4-241">Google ne rend pas les versions de chrome plus anciennes disponibles.</span><span class="sxs-lookup"><span data-stu-id="14ed4-241">Google does not make older chrome versions available.</span></span> <span data-ttu-id="14ed4-242">Suivez les instructions de [Télécharger chrome](https://www.chromium.org/getting-involved/download-chromium) pour tester les anciennes versions de chrome.</span><span class="sxs-lookup"><span data-stu-id="14ed4-242">Follow the instructions at [Download Chromium](https://www.chromium.org/getting-involved/download-chromium) to test older versions of Chrome.</span></span> <span data-ttu-id="14ed4-243">Ne téléchargez **pas** chrome à partir des liens fournis en recherchant les anciennes versions de chrome.</span><span class="sxs-lookup"><span data-stu-id="14ed4-243">Do **not** download Chrome from links provided by searching for older versions of chrome.</span></span>

* [<span data-ttu-id="14ed4-244">Chrome 76 Win64</span><span class="sxs-lookup"><span data-stu-id="14ed4-244">Chromium 76 Win64</span></span>](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/664998/)
* [<span data-ttu-id="14ed4-245">Chrome 74 Win64</span><span class="sxs-lookup"><span data-stu-id="14ed4-245">Chromium 74 Win64</span></span>](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/638880/)
* <span data-ttu-id="14ed4-246">Si vous n’utilisez pas une version 64 bits de Windows, vous pouvez utiliser la [visionneuse OmahaProxy](https://omahaproxy.appspot.com/) pour rechercher la branche de chrome qui correspond au chrome 74 (v 74.0.3729.108) à l’aide [des instructions fournies par chrome](https://www.chromium.org/getting-involved/download-chromium).</span><span class="sxs-lookup"><span data-stu-id="14ed4-246">If you're not using a 64bit version of Windows you can use the [OmahaProxy viewer](https://omahaproxy.appspot.com/) to look up which Chromium branch corresponds to Chrome 74 (v74.0.3729.108) using the [instructions provided by Chromium](https://www.chromium.org/getting-involved/download-chromium).</span></span>

#### <a name="test-with-chrome-80"></a><span data-ttu-id="14ed4-247">Test avec chrome 80 +</span><span class="sxs-lookup"><span data-stu-id="14ed4-247">Test with Chrome 80+</span></span>

<span data-ttu-id="14ed4-248">[Téléchargez](https://www.google.com/chrome/) une version de chrome qui prend en charge son nouvel attribut.</span><span class="sxs-lookup"><span data-stu-id="14ed4-248">[Download](https://www.google.com/chrome/) a version of Chrome that supports their new attribute.</span></span> <span data-ttu-id="14ed4-249">Au moment de la rédaction de cet article, la version actuelle est chrome 80.</span><span class="sxs-lookup"><span data-stu-id="14ed4-249">At the time of writing, the current version is Chrome 80.</span></span> <span data-ttu-id="14ed4-250">Chrome 80 a besoin que l’indicateur `chrome://flags/#same-site-by-default-cookies` activé pour utiliser le nouveau comportement.</span><span class="sxs-lookup"><span data-stu-id="14ed4-250">Chrome 80 needs the flag `chrome://flags/#same-site-by-default-cookies` enabled to use the new behavior.</span></span> <span data-ttu-id="14ed4-251">Vous devez également activer (`chrome://flags/#cookies-without-same-site-must-be-secure`) pour tester le comportement à venir pour les cookies pour lesquels aucun attribut sameSite n’est activé.</span><span class="sxs-lookup"><span data-stu-id="14ed4-251">You should also enable (`chrome://flags/#cookies-without-same-site-must-be-secure`) to test the upcoming behavior for cookies which have no sameSite attribute enabled.</span></span> <span data-ttu-id="14ed4-252">Le chrome 80 est sur la cible pour que le commutateur traite les cookies sans l’attribut en tant que `SameSite=Lax`, bien qu’avec une période de grâce minutée pour certaines requêtes.</span><span class="sxs-lookup"><span data-stu-id="14ed4-252">Chrome 80 is on target to make the switch to treat cookies without the attribute as `SameSite=Lax`, albeit with a timed grace period for certain requests.</span></span> <span data-ttu-id="14ed4-253">Pour désactiver la période de grâce minutée, chrome 80 peut être lancé avec l’argument de ligne de commande suivant :</span><span class="sxs-lookup"><span data-stu-id="14ed4-253">To disable the timed grace period Chrome 80 can be launched with the following command line argument:</span></span>

`--enable-features=SameSiteDefaultChecksMethodRigorously`

<span data-ttu-id="14ed4-254">Chrome 80 contient des messages d’avertissement dans la console du navigateur concernant les attributs sameSite manquants.</span><span class="sxs-lookup"><span data-stu-id="14ed4-254">Chrome 80 has warning messages in the browser console about missing sameSite attributes.</span></span> <span data-ttu-id="14ed4-255">Utilisez la touche F12 pour ouvrir la console du navigateur.</span><span class="sxs-lookup"><span data-stu-id="14ed4-255">Use F12 to open the browser console.</span></span>

### <a name="test-with-safari"></a><span data-ttu-id="14ed4-256">Tester avec Safari</span><span class="sxs-lookup"><span data-stu-id="14ed4-256">Test with Safari</span></span>

<span data-ttu-id="14ed4-257">Safari 12 implémentait strictement le brouillon précédent et échoue lorsque la nouvelle valeur de `None` se trouve dans un cookie.</span><span class="sxs-lookup"><span data-stu-id="14ed4-257">Safari 12 strictly implemented the prior draft and fails when the new `None` value is in a cookie.</span></span> <span data-ttu-id="14ed4-258">`None` est évité par le biais du code de détection du navigateur qui [prend en charge les navigateurs plus anciens](#sob) dans ce document.</span><span class="sxs-lookup"><span data-stu-id="14ed4-258">`None` is avoided via the browser detection code [Supporting older browsers](#sob) in this document.</span></span> <span data-ttu-id="14ed4-259">Testez les connexions de style du système d’exploitation Safari 12, Safari 13 et WebKit à l’aide de MSAL, ADAL ou toute bibliothèque que vous utilisez.</span><span class="sxs-lookup"><span data-stu-id="14ed4-259">Test Safari 12, Safari 13, and WebKit based OS style logins using MSAL, ADAL or whatever library you are using.</span></span> <span data-ttu-id="14ed4-260">Le problème dépend de la version du système d’exploitation sous-jacent.</span><span class="sxs-lookup"><span data-stu-id="14ed4-260">The problem is dependent on the underlying OS version.</span></span> <span data-ttu-id="14ed4-261">OSX Mojave (10,14) et iOS 12 sont connus pour avoir des problèmes de compatibilité avec le nouveau comportement de SameSite.</span><span class="sxs-lookup"><span data-stu-id="14ed4-261">OSX Mojave (10.14) and iOS 12 are known to have compatibility problems with the new SameSite behavior.</span></span> <span data-ttu-id="14ed4-262">La mise à niveau du système d’exploitation vers OSX Catalina (10,15) ou iOS 13 résout le problème.</span><span class="sxs-lookup"><span data-stu-id="14ed4-262">Upgrading the OS to OSX Catalina (10.15) or iOS 13 fixes the problem.</span></span> <span data-ttu-id="14ed4-263">Safari ne dispose pas actuellement d’un indicateur d’abonnement pour tester le nouveau comportement des spécifications.</span><span class="sxs-lookup"><span data-stu-id="14ed4-263">Safari does not currently have an opt-in flag for testing the new spec behavior.</span></span>

### <a name="test-with-firefox"></a><span data-ttu-id="14ed4-264">Test avec Firefox</span><span class="sxs-lookup"><span data-stu-id="14ed4-264">Test with Firefox</span></span>

<span data-ttu-id="14ed4-265">La prise en charge de Firefox pour la nouvelle norme peut être testée sur la version 68 + en choisissant dans la page `about:config` avec l’indicateur de fonctionnalité `network.cookie.sameSite.laxByDefault`.</span><span class="sxs-lookup"><span data-stu-id="14ed4-265">Firefox support for the new standard can be tested on version 68+ by opting in on the `about:config` page with the feature flag `network.cookie.sameSite.laxByDefault`.</span></span> <span data-ttu-id="14ed4-266">Il n’y a eu aucun rapport sur les problèmes de compatibilité avec les versions antérieures de Firefox.</span><span class="sxs-lookup"><span data-stu-id="14ed4-266">There haven't been reports of compatibility issues with older versions of Firefox.</span></span>

### <a name="test-with-edge-legacy-browser"></a><span data-ttu-id="14ed4-267">Test avec le navigateur Edge (hérité)</span><span class="sxs-lookup"><span data-stu-id="14ed4-267">Test with Edge (Legacy) browser</span></span>

<span data-ttu-id="14ed4-268">Edge prend en charge l’ancien standard SameSite.</span><span class="sxs-lookup"><span data-stu-id="14ed4-268">Edge supports the old SameSite standard.</span></span> <span data-ttu-id="14ed4-269">Edge version 44 + ne présente aucun problème de compatibilité connu avec la nouvelle norme.</span><span class="sxs-lookup"><span data-stu-id="14ed4-269">Edge version 44+ doesn't have any known compatibility problems with the new standard.</span></span>

### <a name="test-with-edge-chromium"></a><span data-ttu-id="14ed4-270">Test avec Edge (chrome)</span><span class="sxs-lookup"><span data-stu-id="14ed4-270">Test with Edge (Chromium)</span></span>

<span data-ttu-id="14ed4-271">Les indicateurs SameSite sont définis sur la page `edge://flags/#same-site-by-default-cookies`.</span><span class="sxs-lookup"><span data-stu-id="14ed4-271">SameSite flags are set on the `edge://flags/#same-site-by-default-cookies` page.</span></span> <span data-ttu-id="14ed4-272">Aucun problème de compatibilité n’a été découvert avec le chrome Edge.</span><span class="sxs-lookup"><span data-stu-id="14ed4-272">No compatibility issues were discovered with Edge Chromium.</span></span>

### <a name="test-with-electron"></a><span data-ttu-id="14ed4-273">Test avec électron</span><span class="sxs-lookup"><span data-stu-id="14ed4-273">Test with Electron</span></span>

<span data-ttu-id="14ed4-274">Les versions d’électrons incluent des versions plus anciennes de chrome.</span><span class="sxs-lookup"><span data-stu-id="14ed4-274">Versions of Electron include older versions of Chromium.</span></span> <span data-ttu-id="14ed4-275">Par exemple, la version de l’électron utilisée par les équipes est le chrome 66, qui présente le comportement plus ancien.</span><span class="sxs-lookup"><span data-stu-id="14ed4-275">For example, the version of Electron used by Teams is Chromium 66, which exhibits the older behavior.</span></span> <span data-ttu-id="14ed4-276">Vous devez effectuer vos propres tests de compatibilité avec la version d’électron que votre produit utilise.</span><span class="sxs-lookup"><span data-stu-id="14ed4-276">You must perform your own compatibility testing with the version of Electron your product uses.</span></span> <span data-ttu-id="14ed4-277">Consultez [prise en charge des navigateurs plus anciens](#sob).</span><span class="sxs-lookup"><span data-stu-id="14ed4-277">See [Supporting older browsers](#sob).</span></span>

## <a name="reverting-samesite-patches"></a><span data-ttu-id="14ed4-278">Rétablissement des correctifs SameSite</span><span class="sxs-lookup"><span data-stu-id="14ed4-278">Reverting SameSite patches</span></span>

<span data-ttu-id="14ed4-279">Vous pouvez rétablir le comportement sameSite mis à jour dans .NET Framework applications à son comportement précédent où l’attribut sameSite n’est pas émis pour une valeur de `None`, et restaurer les cookies d’authentification et de session pour ne pas émettre la valeur.</span><span class="sxs-lookup"><span data-stu-id="14ed4-279">You can revert the updated sameSite behavior in .NET Framework apps to its previous behavior where the sameSite attribute is not emitted for a value of `None`, and revert the authentication and session cookies to not emit the value.</span></span> <span data-ttu-id="14ed4-280">Cette solution doit être considérée comme un *correctif extrêmement temporaire*, car les modifications du chrome rompent les demandes ou l’authentification de la publication externe pour les utilisateurs utilisant des navigateurs qui prennent en charge les modifications apportées à la norme.</span><span class="sxs-lookup"><span data-stu-id="14ed4-280">This should be viewed as an *extremely temporary fix*, as the Chrome changes will break any external POST requests or authentication for users using browsers which support the changes to the standard.</span></span>

### <a name="reverting-net-472-behavior"></a><span data-ttu-id="14ed4-281">Rétablissement du comportement de .NET 4.7.2</span><span class="sxs-lookup"><span data-stu-id="14ed4-281">Reverting .NET 4.7.2 behavior</span></span>

<span data-ttu-id="14ed4-282">Mettez à jour le *fichier Web. config* pour inclure les paramètres de configuration suivants :</span><span class="sxs-lookup"><span data-stu-id="14ed4-282">Update *web.config* to include the following configuration settings:</span></span>

```xml
<configuration> 
  <appSettings>
    <add key="aspnet:SuppressSameSiteNone" value="true" />
  </appSettings>
 
  <system.web> 
    <authentication> 
      <forms cookieSameSite="None" /> 
    </authentication> 
    <sessionState cookieSameSite="None" /> 
  </system.web> 
</configuration>
```

## <a name="additional-resources"></a><span data-ttu-id="14ed4-283">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="14ed4-283">Additional resources</span></span>

* [<span data-ttu-id="14ed4-284">Modifications de cookie SameSite à venir dans ASP.NET et ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="14ed4-284">Upcoming SameSite Cookie Changes in ASP.NET and ASP.NET Core</span></span>](https://devblogs.microsoft.com/aspnet/upcoming-samesite-cookie-changes-in-asp-net-and-asp-net-core/)
* [<span data-ttu-id="14ed4-285">Blog du chrome : développeurs : Préparez-vous à la nouvelle SameSite = None ; Sécuriser les paramètres de cookie</span><span class="sxs-lookup"><span data-stu-id="14ed4-285">Chromium Blog:Developers: Get Ready for New SameSite=None; Secure Cookie Settings</span></span>](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)
* [<span data-ttu-id="14ed4-286">Explication des cookies SameSite</span><span class="sxs-lookup"><span data-stu-id="14ed4-286">SameSite cookies explained</span></span>](https://web.dev/samesite-cookies-explained/)
* [<span data-ttu-id="14ed4-287">Mises à jour chrome</span><span class="sxs-lookup"><span data-stu-id="14ed4-287">Chrome Updates</span></span>](https://www.chromium.org/updates/same-site)
* [<span data-ttu-id="14ed4-288">Correctifs SameSite .NET</span><span class="sxs-lookup"><span data-stu-id="14ed4-288">.NET SameSite Patches</span></span>](/aspnet/samesite/kbs-samesite)
* [<span data-ttu-id="14ed4-289">Informations sur les mêmes sites pour les applications Web Azure</span><span class="sxs-lookup"><span data-stu-id="14ed4-289">Azure Web Applications Same Site Information</span></span>](https://azure.microsoft.com/updates/app-service-samesite-cookie-update/)
* [<span data-ttu-id="14ed4-290">Informations sur le même site Azure ActiveDirectory</span><span class="sxs-lookup"><span data-stu-id="14ed4-290">Azure ActiveDirectory Same Site Information</span></span>](/azure/active-directory/develop/howto-handle-samesite-cookie-changes-chrome-browser)
