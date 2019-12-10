---
title: Utiliser des cookies SameSite dans ASP.NET
author: rick-anderson
description: Découvrez comment utiliser pour SameSite des cookies dans ASP.NET
ms.author: riande
ms.date: 12/03/2019
uid: samesite/system-web-samesite
ms.openlocfilehash: 47a3d7576edb0e818c39b32fbbcb98475248e18e
ms.sourcegitcommit: 7b1e1784213dd4c301635f9e181764f3e2f94162
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74993065"
---
# <a name="work-with-samesite-cookies-in-aspnet"></a>Utiliser des cookies SameSite dans ASP.NET

De [Rick Anderson](https://twitter.com/RickAndMSFT)

SameSite est un projet [IETF](https://ietf.org/about/) conçu pour offrir une protection contre les attaques de falsification de requête intersites (CSRF). Le [Brouillon SameSite 2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00):

* Traite les cookies comme des `SameSite=Lax` par défaut.
* États les cookies qui déclarent explicitement `SameSite=None` afin d’activer la remise entre sites doivent être marqués comme `Secure`.

`Lax` fonctionne pour la plupart des cookies d’application. Certaines formes d’authentification comme [OpenID Connect](https://openid.net/connect/) (OIDC) et [WS-Federation](https://auth0.com/docs/protocols/ws-fed) default pour la publication de redirections basées sur. Les redirections basées sur la publication déclenchent les protections du navigateur SameSite, donc SameSite est désactivé pour ces composants. La plupart des connexions [OAuth](https://oauth.net/) ne sont pas affectées en raison de différences de flux de demande.

Le paramètre `None` provoque des problèmes de compatibilité avec les clients qui ont implémenté la [norme préliminaire 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07) précédente (par exemple, IOS 12). Consultez [prise en charge des navigateurs plus anciens](#sob) dans ce document.

Chaque composant ASP.NET Core qui émet des cookies doit décider si SameSite est approprié.

## <a name="api-usage-with-samesite"></a>Utilisation des API avec SameSite

Consultez [HttpCookie. SameSite, propriété](/dotnet/api/system.web.httpcookie.samesite#System_Web_HttpCookie_SameSite)

## <a name="history-and-changes"></a>Historique et modifications

La prise en charge de SameSite a été implémentée pour la première fois dans .NET 4.7.2 à l’aide de la [norme draft 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1).

Le 19 novembre 2019 mises à jour pour Windows a mis à jour .NET 4.7.2 + de la norme 2016 à la norme 2019. Des mises à jour supplémentaires sont à venir pour d’autres versions de Windows. Pour plus d'informations, consultez <xref:samesite/kbs-samesite>.

 Le brouillon 2019 de la spécification SameSite :

* N’est **pas** à compatibilité descendante avec le brouillon 2016. Pour plus d’informations, consultez [prise en charge des navigateurs plus anciens](#sob) dans ce document.
* Spécifie que les cookies sont traités comme `SameSite=Lax` par défaut.
* Spécifie les cookies qui déclarent explicitement `SameSite=None` afin d’activer la remise entre sites qui doit être marquée comme `Secure`. `None` est une nouvelle entrée à refuser.
* Est pris en charge par les correctifs émis comme indiqué dans les Articles de la base de connaissances répertoriés ci-dessus.
* Est planifié pour être activé par [chrome](https://chromestatus.com/feature/5088147346030592) par défaut au [2020 février](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html). Les navigateurs ont commencé à passer à cette norme dans 2019.

<a name="sob"></a>

## <a name="supporting-older-browsers"></a>Prise en charge des navigateurs plus anciens

La norme 2016 SameSite stipule que les valeurs inconnues doivent être traitées comme des valeurs `SameSite=Strict`. Les applications accessibles depuis des navigateurs plus anciens qui prennent en charge la norme 2016 SameSite peuvent s’arrêter lorsqu’ils obtiennent une propriété SameSite avec la valeur `None`. Les applications Web doivent implémenter la détection du navigateur si elles envisagent de prendre en charge des navigateurs plus anciens. ASP.NET n’implémente pas la détection du navigateur, car les valeurs des agents utilisateur sont très volatiles et changent fréquemment. Le code suivant peut être appelé au <xref:HTTP.HttpCookie> site d’appel :

[!code-csharp[](sample/SameSiteCheck.cs?name=snippet)]

Dans l’exemple précédent, `MyUserAgentDetectionLib.DisallowsSameSiteNone` est une bibliothèque fournie par l’utilisateur, qui détecte si l’agent utilisateur ne prend pas en charge SameSite `None`. Le code suivant illustre un exemple de méthode `DisallowsSameSiteNone` :

> [!WARNING]
> Le code suivant est uniquement à des fins de démonstration :
> * Elle ne doit pas être considérée comme terminée.
> * Elle n’est pas gérée ni prise en charge.

[!code-csharp[](sample/SameSiteCheck.cs?name=snippet2)]

## <a name="test-apps-for-samesite-problems"></a>Tester des applications pour les problèmes SameSite

Les applications qui interagissent avec des sites distants, par exemple via une connexion tierce, doivent :

* Testez l’interaction sur plusieurs navigateurs.
* Appliquez la [détection et l’atténuation du navigateur](#sob) abordées dans ce document.

Testez les applications Web à l’aide d’une version du client qui peut s’abonner au nouveau comportement SameSite. Chrome, Firefox et chrome Edge ont tous des indicateurs de fonctionnalités d’abonnement qui peuvent être utilisés à des fins de test. Une fois que votre application applique les correctifs SameSite, testez-la avec des versions client plus anciennes, en particulier Safari. Pour plus d’informations, consultez [prise en charge des navigateurs plus anciens](#sob) dans ce document.

### <a name="test-with-chrome"></a>Tester avec chrome

Chrome 78 + donne des résultats trompeurs, car une atténuation temporaire est en place. Le chrome 78 + atténuation temporaire autorise les cookies datant de moins de deux minutes. Le chrome 76 ou 77 avec les indicateurs de test appropriés activés fournit des résultats plus précis. Pour tester le nouveau comportement SameSite, basculez `chrome://flags/#same-site-by-default-cookies` sur **activé**. Les anciennes versions de chrome (75 et versions antérieures) sont signalées pour échouer avec le nouveau paramètre de `None`. Consultez [prise en charge des navigateurs plus anciens](#sob) dans ce document.

Google ne rend pas les versions de chrome plus anciennes disponibles. Suivez les instructions de [Télécharger chrome](https://www.chromium.org/getting-involved/download-chromium) pour tester les anciennes versions de chrome. Ne téléchargez **pas** chrome à partir des liens fournis en recherchant les anciennes versions de chrome.

* [Chrome 76 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/664998/)
* [Chrome 74 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/638880/)

### <a name="test-with-safari"></a>Tester avec Safari

Safari 12 implémentait strictement le brouillon précédent et échoue lorsque la nouvelle valeur de `None` se trouve dans un cookie. `None` est évité par le biais du code de détection du navigateur qui [prend en charge les navigateurs plus anciens](#sob) dans ce document. Testez les connexions de style du système d’exploitation Safari 12, Safari 13 et WebKit à l’aide de MSAL, ADAL ou toute bibliothèque que vous utilisez. Le problème dépend de la version du système d’exploitation sous-jacent. OSX Mojave (10,14) et iOS 12 sont connus pour avoir des problèmes de compatibilité avec le nouveau comportement de SameSite. La mise à niveau du système d’exploitation vers OSX Catalina (10,15) ou iOS 13 résout le problème. Safari ne dispose pas actuellement d’un indicateur d’abonnement pour tester le nouveau comportement des spécifications.

### <a name="test-with-firefox"></a>Test avec Firefox

La prise en charge de Firefox pour la nouvelle norme peut être testée sur la version 68 + en choisissant dans la page `about:config` avec l’indicateur de fonctionnalité `network.cookie.sameSite.laxByDefault`. Il n’y a eu aucun rapport sur les problèmes de compatibilité avec les versions antérieures de Firefox.

### <a name="test-with-edge-browser"></a>Tester avec le navigateur Edge

Edge prend en charge l’ancien standard SameSite. Edge version 44 ne présente aucun problème de compatibilité connu avec la nouvelle norme.

### <a name="test-with-edge-chromium"></a>Test avec Edge (chrome)

Les indicateurs SameSite sont définis sur la page `edge://flags/#same-site-by-default-cookies`. Aucun problème de compatibilité n’a été découvert avec le chrome Edge.

### <a name="test-with-electron"></a>Test avec électron

Les versions d’électrons incluent des versions plus anciennes de chrome. Par exemple, la version de l’électron utilisée par les équipes est le chrome 66, qui présente le comportement plus ancien. Vous devez effectuer vos propres tests de compatibilité avec la version d’électron que votre produit utilise. Consultez [prise en charge des navigateurs plus anciens](#sob) dans la section suivante.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Blog du chrome : développeurs : Préparez-vous à la nouvelle SameSite = None ; Sécuriser les paramètres de cookie](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)
* [Explication des cookies SameSite](https://web.dev/samesite-cookies-explained/)
