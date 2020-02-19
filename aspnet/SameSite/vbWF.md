---
title: Exemple de cookie SameSite pour les WebForms ASP.NET 4.7.2 VB
author: blowdart
description: Exemple de cookie SameSite pour les WebForms ASP.NET 4.7.2 VB
ms.author: riande
ms.date: 2/15/2019
uid: samesite/vbWF
ms.openlocfilehash: 8979edecc5acf7dac81b9f53d31af00389f4727c
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77458440"
---
# <a name="samesite-cookie-sample-for-aspnet-472-vb-webforms"></a><span data-ttu-id="18eba-103">Exemple de cookie SameSite pour les WebForms ASP.NET 4.7.2 VB</span><span class="sxs-lookup"><span data-stu-id="18eba-103">SameSite cookie sample for ASP.NET 4.7.2 VB WebForms</span></span>
<span data-ttu-id="18eba-104">.NET Framework 4,7 dispose d’une prise en charge intégrée de l’attribut [SameSite](https://www.owasp.org/index.php/SameSite) , mais il adhère à la norme d’origine.</span><span class="sxs-lookup"><span data-stu-id="18eba-104">.NET Framework 4.7 has built-in support for the [SameSite](https://www.owasp.org/index.php/SameSite) attribute, but it adheres to the original standard.</span></span>
<span data-ttu-id="18eba-105">Le comportement corrigé a modifié la signification de `SameSite.None` pour émettre l’attribut avec une valeur de `None`, au lieu de ne pas émettre la valeur du tout.</span><span class="sxs-lookup"><span data-stu-id="18eba-105">The patched behavior changed the meaning of `SameSite.None` to emit the attribute with a value of `None`, rather than not emit the value at all.</span></span> <span data-ttu-id="18eba-106">Si vous ne souhaitez pas émettre la valeur, vous pouvez définir la propriété `SameSite` sur un cookie sur-1.</span><span class="sxs-lookup"><span data-stu-id="18eba-106">If you want to not emit the value you can set the `SameSite` property on a cookie to -1.</span></span>

## <a name="sampleCode"></a><span data-ttu-id="18eba-107">Écriture de l’attribut SameSite</span><span class="sxs-lookup"><span data-stu-id="18eba-107">Writing the SameSite attribute</span></span>

<span data-ttu-id="18eba-108">Voici un exemple d’écriture d’un attribut SameSite sur un cookie ;</span><span class="sxs-lookup"><span data-stu-id="18eba-108">Following is an example of how to write a SameSite attribute on a cookie;</span></span>

```vb
' Create the cookie
Dim sameSiteCookie As New HttpCookie("sameSiteSample")

' Set a value for the cookie
sameSiteCookie.Value = "sample"

' Set the secure flag, which Chrome's changes will require for SameSite none.
' Note this will also require you to be running on HTTPS
sameSiteCookie.Secure = True

' Set the cookie to HTTP only which is good practice unless you really do need
' to access it client side in scripts.
sameSiteCookie.HttpOnly = True

' Expire the cookie in 1 minute
sameSiteCookie.Expires = Date.Now.AddMinutes(1)

' Add the SameSite attribute, this will emit the attribute with a value of none.
' To Not emit the attribute at all set the SameSite property to -1.
sameSiteCookie.SameSite = SameSiteMode.None

' Add the cookie to the response cookie collection
Response.Cookies.Add(sameSiteCookie)
```

[!INCLUDE[](~/includes/MTcomments.md)]

<span data-ttu-id="18eba-109">L’attribut sameSite par défaut d’un cookie d’authentification par formulaire est défini dans le paramètre `cookieSameSite` des paramètres d’authentification par formulaire dans `web.config`</span><span class="sxs-lookup"><span data-stu-id="18eba-109">The default sameSite attribute for a forms authentication cookie is set in the `cookieSameSite` parameter of the forms authentication settings in `web.config`</span></span> 

```xml
<system.web>
  <authentication mode="Forms">
    <forms name=".ASPXAUTH" loginUrl="~/" cookieSameSite="None" requireSSL="true">
    </forms>
  </authentication>
</system.web>
```

<span data-ttu-id="18eba-110">L’attribut sameSite par défaut pour l’état de session est également défini dans le paramètre « cookieSameSite » des paramètres de session dans `web.config`</span><span class="sxs-lookup"><span data-stu-id="18eba-110">The default sameSite attribute for session state is also set in the 'cookieSameSite' parameter of the session settings in `web.config`</span></span>

```xml
<system.web>
  <sessionState cookieSameSite="None">     
  </sessionState>
</system.web>
```

<span data-ttu-id="18eba-111">La mise à jour de novembre 2019 de .NET a modifié les paramètres par défaut pour l’authentification par formulaire et la session sur `lax` comme le paramètre le plus compatible. Toutefois, si vous incorporez des pages dans des IFRAME, vous devrez peut-être rétablir ce paramètre sur aucun, puis ajouter le code d' [interception](#interception) indiqué ci-dessous pour ajuster le comportement `none` en fonction de la fonctionnalité du navigateur.</span><span class="sxs-lookup"><span data-stu-id="18eba-111">The November 2019 update to .NET changed the default settings for Forms Authentication and Session to `lax` as is the most compatible setting, however if you embed pages into iframes you may need to revert this setting to None, and then add the [interception](#interception) code shown below to adjust the `none` behavior depending on browser capability.</span></span>

### <a name="running-the-sample"></a><span data-ttu-id="18eba-112">Exécution de l’exemple</span><span class="sxs-lookup"><span data-stu-id="18eba-112">Running the sample</span></span>

<span data-ttu-id="18eba-113">Si vous exécutez l’exemple de projet, chargez votre débogueur de navigateur sur la page initiale et utilisez-le pour afficher la collection de cookies pour le site.</span><span class="sxs-lookup"><span data-stu-id="18eba-113">If you run the sample project  load your browser debugger on the initial page and use it to view the cookie collection for the site.</span></span>
<span data-ttu-id="18eba-114">Pour ce faire, dans Edge et chrome, appuyez sur `F12` sélectionnez l’onglet `Application` et cliquez sur l’URL du site sous l’option `Cookies` dans la section `Storage`.</span><span class="sxs-lookup"><span data-stu-id="18eba-114">To do so in Edge and Chrome press `F12` then select the `Application` tab and click the site URL under the `Cookies` option in the `Storage` section.</span></span>

![Liste des cookies du débogueur de navigateur](sample/img/BrowserDebugger.png)

<span data-ttu-id="18eba-116">Vous pouvez voir à partir de l’image ci-dessus que le cookie créé par l’exemple lorsque vous cliquez sur le bouton « créer des cookies » a une valeur d’attribut SameSite de `Lax`, correspondant à la valeur définie dans l' [exemple de code](#sampleCode).</span><span class="sxs-lookup"><span data-stu-id="18eba-116">You can see from the image above that the cookie created by the sample when you click the "Create Cookies" button has a SameSite attribute value of `Lax`, matching the value set in the [sample code](#sampleCode).</span></span>

## <a name="interception"></a><span data-ttu-id="18eba-117">Interception des cookies que vous ne contrôlez pas</span><span class="sxs-lookup"><span data-stu-id="18eba-117">Intercepting cookies you do not control</span></span>

<span data-ttu-id="18eba-118">.NET 4.5.2 a introduit un nouvel événement pour intercepter l’écriture des en-têtes, `Response.AddOnSendingHeaders`.</span><span class="sxs-lookup"><span data-stu-id="18eba-118">.NET 4.5.2 introduced a new event for intercepting the writing of headers, `Response.AddOnSendingHeaders`.</span></span> <span data-ttu-id="18eba-119">Cela peut être utilisé pour intercepter les cookies avant qu’ils ne soient retournés à l’ordinateur client.</span><span class="sxs-lookup"><span data-stu-id="18eba-119">This can be used to intercept cookies before they are returned to the client machine.</span></span> <span data-ttu-id="18eba-120">Dans l’exemple, nous allons associer l’événement à une méthode statique qui vérifie si le navigateur prend en charge les modifications apportées au nouveau sameSite, et dans le cas contraire, modifie les cookies pour ne pas émettre l’attribut si la nouvelle valeur de `None` a été définie.</span><span class="sxs-lookup"><span data-stu-id="18eba-120">In the sample we wire up the event to a static method which checks whether the browser supports the new sameSite changes, and if not, changes the cookies to not emit the attribute if the new `None` value has been set.</span></span>

<span data-ttu-id="18eba-121">Consultez [global. asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicWebForms/Global.asax.vb) pour obtenir un exemple de raccordement de l’événement et de [SameSiteCookieRewriter.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicWebForms/SameSiteCookieRewriter.vb) pour obtenir un exemple de gestion de l’événement et d’ajustement de l’attribut `sameSite` cookie.</span><span class="sxs-lookup"><span data-stu-id="18eba-121">See [global.asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicWebForms/Global.asax.vb) for an example of hooking up the event and [SameSiteCookieRewriter.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicWebForms/SameSiteCookieRewriter.vb) for an example of handling the event and adjusting the cookie `sameSite` attribute.</span></span>


```vb
Sub FilterSameSiteNoneForIncompatibleUserAgents(ByVal sender As Object)
    Dim application As HttpApplication = TryCast(sender, HttpApplication)

    If application IsNot Nothing Then
        Dim userAgent = application.Context.Request.UserAgent

        If SameSite.DisallowsSameSiteNone(userAgent) Then
            application.Response.AddOnSendingHeaders(
                Function(context)
                    Dim cookies = context.Response.Cookies

                    For i = 0 To cookies.Count - 1
                        Dim cookie = cookies(i)

                        If cookie.SameSite = SameSiteMode.None Then
                            cookie.SameSite = CType((-1), SameSiteMode)
                        End If
                    Next
                End Function)
        End If
    End If
End Sub
```

<span data-ttu-id="18eba-122">Vous pouvez modifier le comportement d’un cookie nommé de façon similaire ; l’exemple ci-dessous ajuste le cookie d’authentification par défaut de `Lax` à `None` sur les navigateurs qui prennent en charge la valeur de `None`, ou supprime l’attribut sameSite sur les navigateurs qui ne prennent pas en charge `None`.</span><span class="sxs-lookup"><span data-stu-id="18eba-122">You can change specific named cookie behavior in much the same way; the sample below adjust the default authentication cookie from `Lax` to `None` on browsers which support the `None` value, or removes the sameSite attribute on browsers which do not support `None`.</span></span>

```vb
Public Shared Sub AdjustSpecificCookieSettings()
    HttpContext.Current.Response.AddOnSendingHeaders(Function(context)
            Dim cookies = context.Response.Cookies

            For i = 0 To cookies.Count - 1
            Dim cookie = cookies(i)

            If String.Equals(".ASPXAUTH", cookie.Name, StringComparison.Ordinal) Then

                If SameSite.BrowserDetection.DisallowsSameSiteNone(userAgent) Then
                    cookie.SameSite = -1
                Else
                    cookie.SameSite = SameSiteMode.None
                End If

                cookie.Secure = True
            End If
            Next
        End Function)
End Sub
```

## <a name="more-information"></a><span data-ttu-id="18eba-123">Informations supplémentaires</span><span class="sxs-lookup"><span data-stu-id="18eba-123">More Information</span></span>

[<span data-ttu-id="18eba-124">Mises à jour chrome</span><span class="sxs-lookup"><span data-stu-id="18eba-124">Chrome Updates</span></span>](https://www.chromium.org/updates/same-site)

[<span data-ttu-id="18eba-125">Documentation ASP.NET</span><span class="sxs-lookup"><span data-stu-id="18eba-125">ASP.NET Documentation</span></span>](/aspnet/samesite/system-web-samesite)

[<span data-ttu-id="18eba-126">Correctifs SameSite .NET</span><span class="sxs-lookup"><span data-stu-id="18eba-126">.NET SameSite Patches</span></span>](/aspnet/samesite/kbs-samesite)