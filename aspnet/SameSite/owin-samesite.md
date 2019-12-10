---
title: Utiliser des cookies SameSite et l’interface Open Web pour .NET (OWIN)
author: rick-anderson
description: Utiliser des cookies SameSite et l’interface Open Web pour .NET (OWIN)
ms.author: riande
ms.date: 12/6/2019
uid: owin-samesite
ms.openlocfilehash: ac5ae24eeb9e8e1cc6296667a4bebef72c3eb62c
ms.sourcegitcommit: 7b1e1784213dd4c301635f9e181764f3e2f94162
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74993076"
---
# <a name="samesite-cookies-and-the-open-web-interface-for-net-owin"></a>SameSite cookies et l’interface Open Web pour .NET (OWIN)

De [Rick Anderson](https://twitter.com/RickAndMSFT)

`SameSite` est un projet [IETF](https://ietf.org/about/) conçu pour offrir une protection contre les attaques de falsification de requête intersites (CSRF). Le [Brouillon SameSite 2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00):

* Traite les cookies comme des `SameSite=Lax` par défaut.
* États les cookies qui déclarent explicitement `SameSite=None` afin d’activer la remise entre sites doivent être marqués comme `Secure`.

`Lax` fonctionne pour la plupart des cookies d’application. Certaines formes d’authentification comme [OpenID Connect](https://openid.net/connect/) (OIDC) et [WS-Federation](https://auth0.com/docs/protocols/ws-fed) default pour la publication de redirections basées sur. Les redirections basées sur la publication déclenchent les protections du navigateur `SameSite`, donc `SameSite` est désactivée pour ces composants. La plupart des connexions [OAuth](https://oauth.net/) ne sont pas affectées en raison de différences de flux de demande. Tous les autres composants ne définissent **pas** `SameSite` par défaut et utilisent le comportement par défaut des clients (ancien ou nouveau).

Le paramètre `None` provoque des problèmes de compatibilité avec les clients qui ont implémenté la [norme préliminaire 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07) précédente (par exemple, IOS 12). Consultez [prise en charge des navigateurs plus anciens](#sob) dans ce document.

Chaque composant OWIN qui émet des cookies doit décider si `SameSite` est approprié.

Pour obtenir la version ASP.NET 4. x de cet article, consultez <xref:samesite/system-web-samesite>.

## <a name="api-usage-with-samesite"></a>Utilisation des API avec SameSite

`Microsoft.Owin` a sa propre implémentation de `SameSite` :

* Qui n’est pas directement dépendante de celui de `System.Web`.
* `SameSite` fonctionne sur toutes les versions ciblables par les packages `Microsoft.Owin`, .NET 4,5 et versions ultérieures.
* Seul le composant [SystemWebCookieManager](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin.Host.SystemWeb/SystemWebCookieManager.cs) interagit directement avec la classe `System.Web` `HttpCookie`.

`SystemWebCookieManager` dépend des API .NET 4.7.2 `System.Web` pour activer la prise en charge `SameSite` et des correctifs pour modifier le comportement.

Les raisons justifiant l’utilisation de `SystemWebCookieManager` sont présentées dans [OWIN et les problèmes d’intégration de cookie de réponse Web](https://github.com/aspnet/AspNetKatana/wiki/System.Web-response-cookie-integration-issues). `SystemWebCookieManager` est recommandé lors de l’exécution sur `System.Web`.

Le code suivant définit `SameSite` sur `Lax`:

```csharp
owinContext.Response.Cookies.Append("My Key", "My Value", new CookieOptions()
{
    SameSite = SameSiteMode.Lax
});
```

Les API suivantes utilisent `SameSite`:

* [Microsoft. Owin. SameSiteMode](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin/SameSiteMode.cs)
* [CookieOptions. SameSite](xref:Microsoft.AspNetCore.Http.CookieOptions.SameSite)
* [CookieAuthenticationOptions, classe](/previous-versions/aspnet/dn385599(v%3Dvs.113)) <!-- CookieAuthenticationOptions.CookieSameSite not published -->
* [CookieAuthenticationOptions. CookieSameSite](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin.Security.Cookies/CookieAuthenticationOptions.cs#L68-#L72)
* [ICookieManager](/previous-versions/aspnet/dn800238(v%3Dvs.113))
* [SystemWebCookieManager](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin.Host.SystemWeb/SystemWebCookieManager.cs)
* [SystemWebChunkingCookieManager](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin.Host.SystemWeb/SystemWebChunkingCookieManager.cs)
* [CookieAuthenticationOptions. CookieManager](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin.Security.Cookies/CookieAuthenticationOptions.cs#L143-#AL148)
* [OpenIdConnectAuthenticationOptions. CookieManager](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin.Security.OpenIdConnect/OpenIdConnectAuthenticationOptions.cs#L315-#L318)

## <a name="history-and-changes"></a>Historique et modifications

[Microsoft. Owin](https://www.nuget.org/packages/Microsoft.Owin/) n’a jamais pris en charge la [norme`SameSite` 2016 Draft](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1).

La prise en charge du [Brouillon SameSite 2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00) est disponible uniquement dans `Microsoft.Owin` 4.1.0 et versions ultérieures. Il n’existe aucun correctif pour les versions antérieures.

Le brouillon 2019 de la spécification `SameSite` :

* N’est **pas** à compatibilité descendante avec le brouillon 2016. Pour plus d’informations, consultez [prise en charge des navigateurs plus anciens](#sob) dans ce document.
* Spécifie que les cookies sont traités comme `SameSite=Lax` par défaut.
* Spécifie les cookies qui déclarent explicitement `SameSite=None` afin d’activer la remise entre sites qui doit être marquée comme `Secure`. `None` est une nouvelle entrée à refuser.
* Est planifié pour être activé par [chrome](https://chromestatus.com/feature/5088147346030592) par défaut au [2020 février](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html). Les navigateurs ont commencé à passer à cette norme dans 2019.
* Est pris en charge par les correctifs émis comme décrit dans les Articles de la base de connaissances. Pour plus d'informations, consultez <xref:samesite/kbs-samesite>.

<a name="sob"></a>

## <a name="supporting-older-browsers"></a>Prise en charge des navigateurs plus anciens

La norme 2016 `SameSite` stipulant que les valeurs inconnues doivent être traitées comme des valeurs `SameSite=Strict`. Les applications accessibles depuis des navigateurs plus anciens qui prennent en charge la norme 2016 `SameSite` peuvent s’arrêter lorsqu’ils obtiennent une propriété `SameSite` avec la valeur `None`. Les applications Web doivent implémenter la détection du navigateur si elles envisagent de prendre en charge des navigateurs plus anciens. ASP.NET n’implémente pas la détection du navigateur, car les valeurs des agents utilisateur sont très volatiles et changent fréquemment. Un point d’extension dans [ICookieManager](/previous-versions/aspnet/dn800238(v%3Dvs.113)) permet de brancher une logique spécifique à l’agent utilisateur.
<!-- https://docs.microsoft.com/en-us/previous-versions/aspnet/dn800238(v%3Dvs.113) -->

Dans `Startup.Configuration`, ajoutez du code similaire à ce qui suit :

[!code-csharp[](sample/Startup1.cs?name=snippet)]

Le code précédent requiert le correctif .NET 4.7.2 ou version `SameSite` ultérieure.

Le code suivant illustre un exemple d’implémentation de `SameSiteCookieManager`:

[!code-csharp[](sample/SameSiteCookieManager.cs?name=snippet)]

Dans l’exemple précédent, `DisallowsSameSiteNone` est appelé dans la méthode `CheckSameSite`. `DisallowsSameSiteNone` est une méthode utilisateur qui détecte si l’agent utilisateur ne prend pas en charge `SameSite` `None`:

[!code-csharp[](sample/SameSiteCookieManager.cs?name=snippet3&highlight=4)]

Le code suivant illustre un exemple de méthode `DisallowsSameSiteNone` :

> [!WARNING]
> Le code suivant est uniquement à des fins de démonstration :
> * Elle ne doit pas être considérée comme terminée.
> * Elle n’est pas gérée ni prise en charge.

[!code-csharp[](sample/SameSiteCookieManager.cs?name=snippet2)]

## <a name="test-apps-for-samesite-problems"></a>Tester des applications pour les problèmes SameSite

Les applications qui interagissent avec des sites distants, par exemple via une connexion tierce, doivent :

* Testez l’interaction sur plusieurs navigateurs.
* Appliquez la [détection et l’atténuation du navigateur](#sob) abordées dans ce document.

Testez les applications Web à l’aide d’une version de client qui peut s’abonner au nouveau comportement de `SameSite`. Chrome, Firefox et chrome Edge ont tous des indicateurs de fonctionnalités d’abonnement qui peuvent être utilisés à des fins de test. Une fois que votre application applique les correctifs `SameSite`, testez-les avec des versions client plus anciennes, en particulier Safari. Pour plus d’informations, consultez [prise en charge des navigateurs plus anciens](#sob) dans ce document.

### <a name="test-with-chrome"></a>Tester avec chrome

Chrome 78 + donne des résultats trompeurs, car une atténuation temporaire est en place. Le chrome 78 + atténuation temporaire autorise les cookies datant de moins de deux minutes. Le chrome 76 ou 77 avec les indicateurs de test appropriés activés fournit des résultats plus précis. Pour tester le nouveau comportement de `SameSite` `chrome://flags/#same-site-by-default-cookies` basculer vers **activé**. Les anciennes versions de chrome (75 et versions antérieures) sont signalées pour échouer avec le nouveau paramètre de `None`. Consultez [prise en charge des navigateurs plus anciens](#sob) dans ce document.

Google ne rend pas les versions de chrome plus anciennes disponibles. Suivez les instructions de [Télécharger chrome](https://www.chromium.org/getting-involved/download-chromium) pour tester les anciennes versions de chrome. Ne téléchargez **pas** chrome à partir des liens fournis en recherchant les anciennes versions de chrome.

* [Chrome 76 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/664998/)
* [Chrome 74 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/638880/)

### <a name="test-with-safari"></a>Tester avec Safari

Safari 12 implémentait strictement le brouillon précédent et échoue lorsque la nouvelle valeur de `None` se trouve dans un cookie. `None` est évité par le biais du code de détection du navigateur qui [prend en charge les navigateurs plus anciens](#sob) dans ce document. Testez les connexions de style du système d’exploitation Safari 12, Safari 13 et WebKit à l’aide de MSAL, ADAL ou toute bibliothèque que vous utilisez. Le problème dépend de la version du système d’exploitation sous-jacent. OSX Mojave (10,14) et iOS 12 sont connus pour présenter des problèmes de compatibilité avec le nouveau comportement de `SameSite`. La mise à niveau du système d’exploitation vers OSX Catalina (10,15) ou iOS 13 résout le problème. Safari ne dispose pas actuellement d’un indicateur d’abonnement pour tester le nouveau comportement des spécifications.

### <a name="test-with-firefox"></a>Test avec Firefox

La prise en charge de Firefox pour la nouvelle norme peut être testée sur la version 68 + en choisissant dans la page `about:config` avec l’indicateur de fonctionnalité `network.cookie.sameSite.laxByDefault`. Il n’y a eu aucun rapport sur les problèmes de compatibilité avec les versions antérieures de Firefox.

### <a name="test-with-edge-browser"></a>Tester avec le navigateur Edge

Edge prend en charge l’ancien `SameSite` standard. Edge version 44 ne présente aucun problème de compatibilité connu avec la nouvelle norme.

### <a name="test-with-edge-chromium"></a>Test avec Edge (chrome)

`SameSite` indicateurs sont définis sur la page `edge://flags/#same-site-by-default-cookies`. Aucun problème de compatibilité n’a été découvert avec le chrome Edge.

### <a name="test-with-electron"></a>Test avec électron

Les versions d’électrons incluent des versions plus anciennes de chrome. Par exemple, la version de l’électron utilisée par les équipes est le chrome 66, qui présente le comportement plus ancien. Vous devez effectuer vos propres tests de compatibilité avec la version d’électron que votre produit utilise. Consultez [prise en charge des navigateurs plus anciens](#sob) dans la section suivante.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Blog du chrome : développeurs : Préparez-vous à la nouvelle SameSite = None ; Sécuriser les paramètres de cookie](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)
* [Explication des cookies SameSite](https://web.dev/samesite-cookies-explained/)
* [Problèmes d’intégration des cookies de réponse OWIN et System. Web](https://github.com/aspnet/AspNetKatana/wiki/System.Web-response-cookie-integration-issues)
