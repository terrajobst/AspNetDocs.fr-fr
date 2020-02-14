---
title: Utiliser des cookies SameSite dans ASP.NET
author: rick-anderson
description: Découvrez comment utiliser pour SameSite des cookies dans ASP.NET
ms.author: riande
ms.date: 1/22/2019
uid: samesite/system-web-samesite
ms.openlocfilehash: c262e300361f33621e8bd126a34b251c23f56e1a
ms.sourcegitcommit: 6bd0d7581ec36dc32cb85d0d5fc0e51068dd4423
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77234760"
---
# <a name="work-with-samesite-cookies-in-aspnet"></a>Utiliser des cookies SameSite dans ASP.NET

De [Rick Anderson](https://twitter.com/RickAndMSFT)

SameSite est un projet standard de l' [IETF](https://ietf.org/about/) conçu pour offrir une protection contre les attaques de falsification de requête intersites (CSRF). Initialement rédigée dans [2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07), le brouillon standard a été mis à jour dans [2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00). La norme mise à jour n’est pas à compatibilité descendante avec la norme précédente, avec comme suit les différences les plus perceptibles :

* Les cookies sans en-tête SameSite sont traités comme `SameSite=Lax` par défaut.
* `SameSite=None` doit être utilisé pour autoriser l’utilisation de cookies entre sites.
* Les cookies qui déclarent `SameSite=None` doivent également être marqués comme `Secure`.
* La valeur SameSite = None n’est pas autorisée par la [norme 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07) et oblige certaines implémentations à traiter ces cookies comme SameSite = strict. Consultez [prise en charge des navigateurs plus anciens](#sob) dans ce document.

Le paramètre `SameSite=Lax` fonctionne pour la plupart des cookies d’application. Certaines formes d’authentification comme [OpenID Connect](https://openid.net/connect/) (OIDC) et [WS-Federation](https://auth0.com/docs/protocols/ws-fed) default pour la publication de redirections basées sur. Les redirections basées sur la publication déclenchent les protections du navigateur SameSite, donc SameSite est désactivé pour ces composants. La plupart des connexions [OAuth](https://oauth.net/) ne sont pas affectées en raison de différences de flux de demande.

Les applications qui utilisent `iframe` peuvent rencontrer des problèmes avec les cookies `SameSite=Lax` ou `SameSite=Strict`, car les IFRAMES sont traités comme des scénarios intersites.

Chaque composant ASP.NET qui émet des cookies doit décider si SameSite est approprié.

Consultez [problèmes connus liés](#known) aux problèmes d’applications après l’installation des mises à jour 2019 .net SameSite.

## <a name="using-samesite-in-aspnet-472-and-48"></a>Utilisation de SameSite dans ASP.NET 4.7.2 et 4,8

.Net 4.7.2 et 4,8 prennent en charge [2019 Draft Standard](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00) pour SameSite depuis la publication des mises à jour en décembre 2019. Les développeurs sont en mesure de contrôler par programmation la valeur de l’en-tête SameSite à l’aide de la [propriété HttpCookie. SameSite](/dotnet/api/system.web.httpcookie.samesite#System_Web_HttpCookie_SameSite). Si vous affectez à la propriété `SameSite` la valeur `Strict`, `Lax`ou `None`, ces valeurs sont écrites sur le réseau avec le cookie. La valeur `(SameSiteMode)(-1)` indique qu’aucun en-tête SameSite ne doit être inclus sur le réseau avec le cookie. La [propriété HttpCookie. Secure](/dotnet/api/system.web.httpcookie.secure), ou’RequireSSL’dans les fichiers de configuration, peut être utilisée pour marquer le cookie comme `Secure` ou non.

Par défaut, les nouvelles instances de `HttpCookie` sont `SameSite=(SameSiteMode)(-1)` et `Secure=false`. Ces valeurs par défaut peuvent être remplacées dans la section de configuration `system.web/httpCookies`, où la chaîne `"Unspecified"` est une syntaxe conviviale de configuration uniquement pour `(SameSiteMode)(-1)`:

```xml
<configuration>
 <system.web>
  <httpCookies sameSite="[Strict|Lax|None|Unspecified]" requireSSL="[true|false]" />
 <system.web>
<configuration>
```

ASP.Net émet également quatre cookies spécifiques propres à ces fonctionnalités : authentification anonyme, authentification par formulaire, état de session et gestion des rôles. Les instances de ces cookies obtenues dans le runtime peuvent être manipulées à l’aide des propriétés `SameSite` et `Secure` comme n’importe quelle autre instance HttpCookie. Toutefois, en raison de l’émergence du patchwork de la norme SameSite, les options de configuration de ces quatre fonctionnalités sont incohérentes. Les sections de configuration et attributs appropriés, avec les valeurs par défaut, sont indiqués ci-dessous. S’il n’existe aucun `SameSite` ou `Secure` attribut associé pour une fonctionnalité, la fonctionnalité est redéfinie sur les valeurs par défaut configurées dans la section `system.web/httpCookies` présentée ci-dessus.

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

**Remarque**: « non spécifié » n’est disponible que pour `system.web/httpCookies@sameSite` pour le moment. Nous espérons ajouter une syntaxe similaire aux attributs cookieSameSite précédemment affichés dans les futures mises à jour. La définition de `(SameSiteMode)(-1)` dans le code fonctionne toujours sur les instances de ces cookies. *

## <a name="history-and-changes"></a>Historique et modifications

La prise en charge de SameSite a été implémentée pour la première fois dans .NET 4.7.2 à l’aide de la [norme draft 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1).

Le 19 novembre 2019 mises à jour pour Windows a mis à jour .NET 4.7.2 + de la norme 2016 à la norme 2019. Des mises à jour supplémentaires sont à venir pour d’autres versions de Windows. Pour plus d’informations, consultez <xref:samesite/kbs-samesite>.

 Le brouillon 2019 de la spécification SameSite :

* N’est **pas** à compatibilité descendante avec le brouillon 2016. Pour plus d’informations, consultez [prise en charge des navigateurs plus anciens](#sob) dans ce document.
* Spécifie que les cookies sont traités comme `SameSite=Lax` par défaut.
* Spécifie les cookies qui déclarent explicitement `SameSite=None` pour permettre la remise entre sites doivent également être marqués comme `Secure`.
* Est pris en charge par les correctifs émis comme indiqué dans les Articles de la base de connaissances répertoriés ci-dessus.
* Est planifié pour être activé par [chrome](https://chromestatus.com/feature/5088147346030592) par défaut au [2020 février](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html). Les navigateurs ont commencé à passer à cette norme dans 2019.

<a name="known"><a/>

## <a name="known-issues"></a>Problèmes connus

Étant donné que les spécifications de brouillon 2016 et 2019 ne sont pas compatibles, la mise à jour du .NET Framework de novembre 2019 introduit des modifications susceptibles d’être endommagées.

* L’état de session et les cookies d’authentification par formulaire sont désormais écrits sur le réseau en tant que `Lax` au lieu d’être non spécifiés.
  * Tandis que la plupart des applications fonctionnent avec des cookies `SameSite=Lax`, les applications qui sont publiées sur les sites ou les applications qui utilisent des `iframe` peuvent constater que leurs cookies d’état de session ou d’autorisation de formulaires ne sont pas utilisés comme prévu. Pour remédier à cela, modifiez la valeur `cookieSameSite` dans la section de configuration appropriée, comme indiqué précédemment.
* Les HttpCookies qui définissent explicitement `SameSite=None` dans le code ou la configuration ont maintenant cette valeur écrite avec le cookie, alors qu’elle a été précédemment omise. Cela peut entraîner des problèmes avec les navigateurs plus anciens qui prennent uniquement en charge le brouillon 2016 standard.
  * Lorsque vous ciblez des navigateurs prenant en charge le brouillon 2019 avec `SameSite=None` cookies, n’oubliez pas de les marquer également `Secure` ou s’ils ne sont pas reconnus.
  * Pour rétablir le comportement 2016 qui consiste à ne pas écrire `SameSite=None`, utilisez le paramètre d’application `aspnet:SupressSameSiteNone=true`. Notez que cela s’applique à tous les HttpCookies de l’application.

### <a name="azure-app-servicesamesite-cookie-handling"></a>Azure App Service : gestion des cookies SameSite

Pour plus d’informations sur Azure App Service la configuration des comportements SameSite dans les applications .net 4.7.2, consultez [Azure App service, gestion des cookies SameSite et .NET Framework correctif 4.7.2](https://azure.microsoft.com/updates/app-service-samesite-cookie-update/) .

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

* [Modifications de cookie SameSite à venir dans ASP.NET et ASP.NET Core](https://devblogs.microsoft.com/aspnet/upcoming-samesite-cookie-changes-in-asp-net-and-asp-net-core/)
* [Blog du chrome : développeurs : Préparez-vous à la nouvelle SameSite = None ; Sécuriser les paramètres de cookie](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)
* [Explication des cookies SameSite](https://web.dev/samesite-cookies-explained/)
