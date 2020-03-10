---
title: Exemple de cookie SameSite pour ASP.NET C# 4.7.2 MVC
author: blowdart
description: Exemple de cookie SameSite pour ASP.NET C# 4.7.2 MVC
ms.author: riande
ms.date: 2/15/2019
uid: samesite/csMVC
ms.openlocfilehash: dcbd0bee009669fb747d74e6ccef07fbae70a236
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544722"
---
# <a name="samesite-cookie-sample-for-aspnet-472-c-mvc"></a><span data-ttu-id="5be41-103">Exemple de cookie SameSite pour ASP.NET C# 4.7.2 MVC</span><span class="sxs-lookup"><span data-stu-id="5be41-103">SameSite cookie sample for ASP.NET 4.7.2 C# MVC</span></span>

<span data-ttu-id="5be41-104">.NET Framework 4,7 dispose d’une prise en charge intégrée de l’attribut [SameSite](https://www.owasp.org/index.php/SameSite) , mais il adhère à la norme d’origine.</span><span class="sxs-lookup"><span data-stu-id="5be41-104">.NET Framework 4.7 has built-in support for the [SameSite](https://www.owasp.org/index.php/SameSite) attribute, but it adheres to the original standard.</span></span>
<span data-ttu-id="5be41-105">Le comportement corrigé a modifié la signification de `SameSite.None` pour émettre l’attribut avec une valeur de `None`, au lieu de ne pas émettre la valeur du tout.</span><span class="sxs-lookup"><span data-stu-id="5be41-105">The patched behavior changed the meaning of `SameSite.None` to emit the attribute with a value of `None`, rather than not emit the value at all.</span></span> <span data-ttu-id="5be41-106">Si vous ne souhaitez pas émettre la valeur, vous pouvez définir la propriété `SameSite` sur un cookie sur-1.</span><span class="sxs-lookup"><span data-stu-id="5be41-106">If you want to not emit the value you can set the `SameSite` property on a cookie to -1.</span></span>

## <a name="sampleCode"></a><span data-ttu-id="5be41-107">Écriture de l’attribut SameSite</span><span class="sxs-lookup"><span data-stu-id="5be41-107">Writing the SameSite attribute</span></span>

<span data-ttu-id="5be41-108">Voici un exemple d’écriture d’un attribut SameSite sur un cookie ;</span><span class="sxs-lookup"><span data-stu-id="5be41-108">Following is an example of how to write a SameSite attribute on a cookie;</span></span>

```c#
// Create the cookie
HttpCookie sameSiteCookie = new HttpCookie("SameSiteSample");

// Set a value for the cookieSite none.
// Note this will also require you to be running on HTTPS
sameSiteCookie.Value = "sample";

// Set the secure flag, which Chrome's changes will require for Same
sameSiteCookie.Secure = true;

// Set the cookie to HTTP only which is good practice unless you really do need
// to access it client side in scripts.
sameSiteCookie.HttpOnly = true;

// Add the SameSite attribute, this will emit the attribute with a value of none.
// To not emit the attribute at all set the SameSite property to -1.
sameSiteCookie.SameSite = SameSiteMode.None;

// Add the cookie to the response cookie collection
Response.Cookies.Add(sameSiteCookie);
```

[!INCLUDE[](~/includes/MTcomments.md)]

<span data-ttu-id="5be41-109">L’attribut sameSite par défaut pour l’état de session est défini dans le paramètre « cookieSameSite » des paramètres de session dans `web.config`</span><span class="sxs-lookup"><span data-stu-id="5be41-109">The default sameSite attribute for session state is set in the 'cookieSameSite' parameter of the session settings in `web.config`</span></span>

```xml
<system.web>
  <sessionState cookieSameSite="None">     
  </sessionState>
</system.web>
```

## <a name="mvc-authentication"></a><span data-ttu-id="5be41-110">Authentification MVC</span><span class="sxs-lookup"><span data-stu-id="5be41-110">MVC Authentication</span></span>

<span data-ttu-id="5be41-111">L’authentification basée sur les cookies OWIN MVC utilise un gestionnaire de cookies pour permettre la modification des attributs de cookie.</span><span class="sxs-lookup"><span data-stu-id="5be41-111">OWIN MVC cookie based authentication uses a cookie manager to enable the changing of cookie attributes.</span></span> <span data-ttu-id="5be41-112">Le [SameSiteCookieManager.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpMVC5/SameSiteCookieManager.cs) est une implémentation d’une telle classe que vous pouvez copier dans vos propres projets.</span><span class="sxs-lookup"><span data-stu-id="5be41-112">The [SameSiteCookieManager.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpMVC5/SameSiteCookieManager.cs) is an implementation of such a class which you can copy into your own projects.</span></span> 

<span data-ttu-id="5be41-113">Vous devez vous assurer que vos composants Microsoft. Owin sont tous mis à niveau vers la version 4.1.0 ou supérieure.</span><span class="sxs-lookup"><span data-stu-id="5be41-113">You must ensure your Microsoft.Owin components are all upgraded to version 4.1.0 or greater.</span></span> <span data-ttu-id="5be41-114">Vérifiez votre fichier `packages.config` pour vous assurer que tous les numéros de version correspondent, par exemple.</span><span class="sxs-lookup"><span data-stu-id="5be41-114">Check your `packages.config` file to ensure all the version numbers match, for example.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
  <!-- other packages -->
  <package id="Microsoft.Owin.Host.SystemWeb" version="4.1.0" targetFramework="net472" />
  <package id="Microsoft.Owin.Security" version="4.1.0" targetFramework="net472" />
  <package id="Microsoft.Owin.Security.Cookies" version="4.1.0" targetFramework="net472" />
  <package id="Microsoft.Web.Infrastructure" version="1.0.0.0" targetFramework="net472" />
  <package id="Owin" version="1.0" targetFramework="net472" />
</packages>
```

<span data-ttu-id="5be41-115">Les composants d’authentification doivent ensuite être configurés pour utiliser CookieManager dans votre classe de démarrage.</span><span class="sxs-lookup"><span data-stu-id="5be41-115">The authentication components must then be configured to use the CookieManager in your startup class;</span></span>

```c#
public void Configuration(IAppBuilder app)
{
    app.UseCookieAuthentication(new CookieAuthenticationOptions
    {
        CookieSameSite = SameSiteMode.None,
        CookieHttpOnly = true,
        CookieSecure = CookieSecureOption.Always,
        CookieManager = new SameSiteCookieManager(new SystemWebCookieManager())
    });
}
```

<span data-ttu-id="5be41-116">Un gestionnaire de cookies doit être défini sur *chaque* composant qui le prend en charge, y compris CookieAuthentication et OpenIdConnectAuthentication.</span><span class="sxs-lookup"><span data-stu-id="5be41-116">A cookie manager must be set on *each* component that supports it, this includes CookieAuthentication and OpenIdConnectAuthentication.</span></span>

<span data-ttu-id="5be41-117">Le SystemWebCookieManager est utilisé pour éviter des [problèmes connus](https://github.com/aspnet/AspNetKatana/wiki/System.Web-response-cookie-integration-issues) avec l’intégration de cookie de réponse.</span><span class="sxs-lookup"><span data-stu-id="5be41-117">The SystemWebCookieManager is used to avoid [known issues](https://github.com/aspnet/AspNetKatana/wiki/System.Web-response-cookie-integration-issues) with response cookie integration.</span></span>

### <a name="running-the-sample"></a><span data-ttu-id="5be41-118">Exécution de l'exemple</span><span class="sxs-lookup"><span data-stu-id="5be41-118">Running the sample</span></span>

<span data-ttu-id="5be41-119">Si vous exécutez l’exemple de projet, chargez votre débogueur de navigateur sur la page initiale et utilisez-le pour afficher la collection de cookies pour le site.</span><span class="sxs-lookup"><span data-stu-id="5be41-119">If you run the sample project  load your browser debugger on the initial page and use it to view the cookie collection for the site.</span></span>
<span data-ttu-id="5be41-120">Pour ce faire, dans Edge et chrome, appuyez sur `F12` sélectionnez l’onglet `Application` et cliquez sur l’URL du site sous l’option `Cookies` dans la section `Storage`.</span><span class="sxs-lookup"><span data-stu-id="5be41-120">To do so in Edge and Chrome press `F12` then select the `Application` tab and click the site URL under the `Cookies` option in the `Storage` section.</span></span>

![Liste des cookies du débogueur de navigateur](sample/img/BrowserDebugger.png)

<span data-ttu-id="5be41-122">Vous pouvez voir à partir de l’image ci-dessus que le cookie créé par l’exemple lorsque vous cliquez sur le bouton « créer des cookies » a une valeur d’attribut SameSite de `Lax`, correspondant à la valeur définie dans l' [exemple de code](#sampleCode).</span><span class="sxs-lookup"><span data-stu-id="5be41-122">You can see from the image above that the cookie created by the sample when you click the "Create Cookies" button has a SameSite attribute value of `Lax`, matching the value set in the [sample code](#sampleCode).</span></span>

## <a name="interception"></a><span data-ttu-id="5be41-123">Interception des cookies que vous ne contrôlez pas</span><span class="sxs-lookup"><span data-stu-id="5be41-123">Intercepting cookies you do not control</span></span>

<span data-ttu-id="5be41-124">.NET 4.5.2 a introduit un nouvel événement pour intercepter l’écriture des en-têtes, `Response.AddOnSendingHeaders`.</span><span class="sxs-lookup"><span data-stu-id="5be41-124">.NET 4.5.2 introduced a new event for intercepting the writing of headers, `Response.AddOnSendingHeaders`.</span></span> <span data-ttu-id="5be41-125">Cela peut être utilisé pour intercepter les cookies avant qu’ils ne soient retournés à l’ordinateur client.</span><span class="sxs-lookup"><span data-stu-id="5be41-125">This can be used to intercept cookies before they are returned to the client machine.</span></span> <span data-ttu-id="5be41-126">Dans l’exemple, nous allons associer l’événement à une méthode statique qui vérifie si le navigateur prend en charge les modifications apportées au nouveau sameSite, et dans le cas contraire, modifie les cookies pour ne pas émettre l’attribut si la nouvelle valeur de `None` a été définie.</span><span class="sxs-lookup"><span data-stu-id="5be41-126">In the sample we wire up the event to a static method which checks whether the browser supports the new sameSite changes, and if not, changes the cookies to not emit the attribute if the new `None` value has been set.</span></span>

<span data-ttu-id="5be41-127">Consultez [global. asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpMVC5/Global.asax.cs) pour obtenir un exemple de raccordement de l’événement et de [SameSiteCookieRewriter.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpMVC5/SameSiteCookieRewriter.cs) pour obtenir un exemple de gestion de l’événement et d’ajustement de l’attribut de `sameSite` cookie que vous pouvez copier dans votre propre code.</span><span class="sxs-lookup"><span data-stu-id="5be41-127">See [global.asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpMVC5/Global.asax.cs) for an example of hooking up the event and [SameSiteCookieRewriter.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpMVC5/SameSiteCookieRewriter.cs) for an example of handling the event and adjusting the cookie `sameSite` attribute which you can copy into your own code.</span></span>

```c#
public static void FilterSameSiteNoneForIncompatibleUserAgents(object sender)
{
    HttpApplication application = sender as HttpApplication;
    if (application != null)
    {
        var userAgent = application.Context.Request.UserAgent;
        if (SameSite.BrowserDetection.DisallowsSameSiteNone(userAgent))
        {
            HttpContext.Current.Response.AddOnSendingHeaders(context =>
            {
                var cookies = context.Response.Cookies;
                for (var i = 0; i < cookies.Count; i++)
                {
                    var cookie = cookies[i];
                    if (cookie.SameSite == SameSiteMode.None)
                    {
                        cookie.SameSite = (SameSiteMode)(-1); // Unspecified
                    }
                }
            });
        }
    }
}
```

<span data-ttu-id="5be41-128">Vous pouvez modifier le comportement d’un cookie nommé de façon similaire ; l’exemple ci-dessous ajuste le cookie d’authentification par défaut de `Lax` à `None` sur les navigateurs qui prennent en charge la valeur de `None`, ou supprime l’attribut sameSite sur les navigateurs qui ne prennent pas en charge `None`.</span><span class="sxs-lookup"><span data-stu-id="5be41-128">You can change specific named cookie behavior in much the same way; the sample below adjust the default authentication cookie from `Lax` to `None` on browsers which support the `None` value, or removes the sameSite attribute on browsers which do not support `None`.</span></span>

```c#
public static void AdjustSpecificCookieSettings()
{
    HttpContext.Current.Response.AddOnSendingHeaders(context =>
    {
        var cookies = context.Response.Cookies;
        for (var i = 0; i < cookies.Count; i++)
        {
            var cookie = cookies[i]; 
            // Forms auth: ".ASPXAUTH"
            // Session: "ASP.NET_SessionId"
            if (string.Equals(".ASPXAUTH", cookie.Name, StringComparison.Ordinal))
            { 
                if (SameSite.BrowserDetection.DisallowsSameSiteNone(userAgent))
                {
                    cookie.SameSite = -1;
                }
                else
                {
                    cookie.SameSite = SameSiteMode.None;
                }
                cookie.Secure = true;
            }
        }
    });
}
```

### <a name="more-information"></a><span data-ttu-id="5be41-129">Plus d'informations</span><span class="sxs-lookup"><span data-stu-id="5be41-129">More Information</span></span>
 
[<span data-ttu-id="5be41-130">Mises à jour chrome</span><span class="sxs-lookup"><span data-stu-id="5be41-130">Chrome Updates</span></span>](https://www.chromium.org/updates/same-site)

[<span data-ttu-id="5be41-131">Documentation OWIN SameSite</span><span class="sxs-lookup"><span data-stu-id="5be41-131">OWIN SameSite Documentation</span></span>](/aspnet/samesite/owin-samesite)

[<span data-ttu-id="5be41-132">Documentation ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5be41-132">ASP.NET Documentation</span></span>](/aspnet/samesite/system-web-samesite)

[<span data-ttu-id="5be41-133">Correctifs SameSite .NET</span><span class="sxs-lookup"><span data-stu-id="5be41-133">.NET SameSite Patches</span></span>](/aspnet/samesite/kbs-samesite)