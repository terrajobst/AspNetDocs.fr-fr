---
title: Exemple de cookie SameSite pour ASP.NET 4.7.2 VB MVC
author: blowdart
description: Exemple de cookie SameSite pour ASP.NET 4.7.2 VB MVC
ms.author: riande
ms.date: 2/15/2019
uid: samesite/vbMVC
ms.openlocfilehash: f6effce6075f94fb58ce10ec08bf010fab8b4b56
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77458468"
---
# <a name="samesite-cookie-sample-for-aspnet-472-vb-mvc"></a>Exemple de cookie SameSite pour ASP.NET 4.7.2 VB MVC

.NET Framework 4,7 dispose d’une prise en charge intégrée de l’attribut [SameSite](https://www.owasp.org/index.php/SameSite) , mais il adhère à la norme d’origine.
Le comportement corrigé a modifié la signification de `SameSite.None` pour émettre l’attribut avec une valeur de `None`, au lieu de ne pas émettre la valeur du tout. Si vous ne souhaitez pas émettre la valeur, vous pouvez définir la propriété `SameSite` sur un cookie sur-1.

## <a name="sampleCode"></a>Écriture de l’attribut SameSite

Voici un exemple d’écriture d’un attribut SameSite sur un cookie ;

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

L’attribut sameSite par défaut pour l’état de session est défini dans le paramètre « cookieSameSite » des paramètres de session dans `web.config`

```xml
<system.web>
  <sessionState cookieSameSite="None">     
  </sessionState>
</system.web>
```

## <a name="mvc-authentication"></a>Authentification MVC

L’authentification basée sur les cookies OWIN MVC utilise un gestionnaire de cookies pour permettre la modification des attributs de cookie. [SameSiteCookieManager. vb](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicMVC5/SameSiteCookieManager.vb) est une implémentation d’une telle classe que vous pouvez copier dans vos propres projets. 

Vous devez vous assurer que vos composants Microsoft. Owin sont tous mis à niveau vers la version 4.1.0 ou supérieure. Vérifiez votre fichier `packages.config` pour vous assurer que tous les numéros de version correspondent, par exemple.

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

Les composants d’authentification doivent être configurés pour utiliser CookieManager dans votre classe de démarrage.

```vb
Public Sub Configuration(app As IAppBuilder)
    app.UseCookieAuthentication(New CookieAuthenticationOptions() With {
        .CookieSameSite = SameSiteMode.None,
        .CookieHttpOnly = True,
        .CookieSecure = CookieSecureOption.Always,
        .CookieManager = New SameSiteCookieManager(New SystemWebCookieManager())
    })
End Sub
```

Un gestionnaire de cookies doit être défini sur *chaque* composant qui le prend en charge, y compris CookieAuthentication et OpenIdConnectAuthentication.

Le SystemWebCookieManager est utilisé pour éviter des [problèmes connus](https://github.com/aspnet/AspNetKatana/wiki/System.Web-response-cookie-integration-issues) avec l’intégration de cookie de réponse.

### <a name="running-the-sample"></a>Exécution de l’exemple

Si vous exécutez l’exemple de projet, chargez votre débogueur de navigateur sur la page initiale et utilisez-le pour afficher la collection de cookies pour le site.
Pour ce faire, dans Edge et chrome, appuyez sur `F12` sélectionnez l’onglet `Application` et cliquez sur l’URL du site sous l’option `Cookies` dans la section `Storage`.

![Liste des cookies du débogueur de navigateur](sample/img/BrowserDebugger.png)

Vous pouvez voir à partir de l’image ci-dessus que le cookie créé par l’exemple lorsque vous cliquez sur le bouton « créer un cookie SameSite » a une valeur d’attribut SameSite de `Lax`, ce qui correspond à la valeur définie dans l' [exemple de code](#sampleCode).

## <a name="interception"></a>Interception des cookies que vous ne contrôlez pas

.NET 4.5.2 a introduit un nouvel événement pour intercepter l’écriture des en-têtes, `Response.AddOnSendingHeaders`. Cela peut être utilisé pour intercepter les cookies avant qu’ils ne soient retournés à l’ordinateur client. Dans l’exemple, nous allons associer l’événement à une méthode statique qui vérifie si le navigateur prend en charge les modifications apportées au nouveau sameSite, et dans le cas contraire, modifie les cookies pour ne pas émettre l’attribut si la nouvelle valeur de `None` a été définie.

Consultez [global. asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicMVC5/Global.asax.vb) pour obtenir un exemple de raccordement de l’événement et [SameSiteCookieRewriter. vb](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicMVC5/SameSiteCookieRewriter.vb) pour obtenir un exemple de gestion de l’événement et d’ajustement de l’attribut `sameSite` cookie que vous pouvez copier dans votre propre code.

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

Vous pouvez modifier le comportement d’un cookie nommé de façon similaire ; l’exemple ci-dessous ajuste le cookie d’authentification par défaut de `Lax` à `None` sur les navigateurs qui prennent en charge la valeur de `None`, ou supprime l’attribut sameSite sur les navigateurs qui ne prennent pas en charge `None`.

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

## <a name="more-information"></a>Informations supplémentaires
 
[Mises à jour chrome](https://www.chromium.org/updates/same-site)

[Documentation OWIN SameSite](/aspnet/samesite/owin-samesite)

[Documentation ASP.NET](/aspnet/samesite/system-web-samesite)

[Correctifs SameSite .NET](/aspnet/samesite/kbs-samesite)