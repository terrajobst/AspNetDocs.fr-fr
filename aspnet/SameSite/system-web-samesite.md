---
title: Utiliser des cookies SameSite dans ASP.NET
author: rick-anderson
description: Découvrez comment utiliser pour SameSite des cookies dans ASP.NET
ms.author: riande
ms.date: 2/15/2019
uid: samesite/system-web-samesite
ms.openlocfilehash: 7987a5d6c9b3a82679d42a2d381d471d56f495c2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78546745"
---
# <a name="work-with-samesite-cookies-in-aspnet"></a>Utiliser des cookies SameSite dans ASP.NET

De [Rick Anderson](https://twitter.com/RickAndMSFT)

SameSite est un projet standard de l' [IETF](https://ietf.org/about/) conçu pour offrir une protection contre les attaques de falsification de requête intersites (CSRF). Initialement rédigée dans [2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07), le brouillon standard a été mis à jour dans [2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00). La norme mise à jour n’est pas à compatibilité descendante avec la norme précédente, avec comme suit les différences les plus perceptibles :

* Les cookies sans en-tête SameSite sont traités comme `SameSite=Lax` par défaut.
* `SameSite=None` doit être utilisé pour autoriser l’utilisation de cookies entre sites.
* Les cookies qui déclarent `SameSite=None` doivent également être marqués comme `Secure`.
* Les applications qui utilisent [`<iframe>`](https://developer.mozilla.org/docs/Web/HTML/Element/iframe) peuvent rencontrer des problèmes avec les cookies `sameSite=Lax` ou `sameSite=Strict`, car `<iframe>` est traité comme des scénarios intersites.
* La valeur `SameSite=None` n’est pas autorisée par la [norme 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07) et oblige certaines implémentations à traiter ces cookies comme des `SameSite=Strict`. Consultez [prise en charge des navigateurs plus anciens](#sob) dans ce document.

Le paramètre `SameSite=Lax` fonctionne pour la plupart des cookies d’application. Certaines formes d’authentification comme [OpenID Connect](https://openid.net/connect/) (OIDC) et [WS-Federation](https://auth0.com/docs/protocols/ws-fed) default pour la publication de redirections basées sur. Les redirections basées sur la publication déclenchent les protections du navigateur SameSite, donc SameSite est désactivé pour ces composants. La plupart des connexions [OAuth](https://oauth.net/) ne sont pas affectées en raison de différences de flux de demande.

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

[!INCLUDE[](~/includes/MTcomments.md)]

<a name="retargeting"></a>

### <a name="retarget-net-apps"></a>Recibler des applications .NET

Pour cibler .NET 4.7.2 ou version ultérieure :

* Vérifiez que le *fichier Web. config* contient les éléments suivants :  <!-- review, I removed `debug="true"` -->

  ```xml
  <system.web>
    <compilation targetFramework="4.7.2"/>
    <httpRuntime targetFramework="4.7.2"/>
  </system.web>

* Verify the project file contains the correct [TargetFrameworkVersion](/visualstudio/msbuild/msbuild-target-framework-and-target-platform?view=vs-2019):

  ```xml
  <TargetFrameworkVersion>v4.7.2</TargetFrameworkVersion>
  ```

  Le [Guide de migration de .net](/dotnet/framework/migration-guide/) offre plus de détails.

* Vérifiez que les packages NuGet dans le projet sont ciblés sur la version de Framework appropriée. Vous pouvez vérifier la version du Framework appropriée en examinant le fichier *packages. config* , par exemple :

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <packages>
    <package id="Microsoft.AspNet.Mvc" version="5.2.7" targetFramework="net472" />
    <package id="Microsoft.ApplicationInsights" version="2.4.0" targetFramework="net451" />
  </packages>
  ```

  Dans le fichier *packages. config* précédent, le package `Microsoft.ApplicationInsights` :
    * Est ciblé par rapport à .NET 4.5.1.
    * L’attribut `targetFramework` doit être mis à jour pour `net472` si un package mis à jour ciblant votre cible de Framework existe.

<a name="nope"></a>

### <a name="net-versions-earlier-than-472"></a>Versions de .NET antérieures à 4.7.2

Microsoft ne prend pas en charge les versions .NET inférieures à ce 4.7.2 pour l’écriture de l’attribut de cookie du même site. Nous n’avons pas trouvé de méthode fiable pour :

* Vérifiez que l’attribut est écrit correctement en fonction de la version du navigateur.
* Interceptez et ajustez l’authentification et les cookies de session sur les versions antérieures du Framework.

### <a name="december-patch-behavior-changes"></a>Modifications du comportement du correctif décembre

La modification de comportement spécifique pour .NET Framework est la manière dont la propriété `SameSite` interprète la valeur `None` :

* Avant que le correctif ait une valeur de `None` signifiait :
  * N’émettez pas l’attribut du tout.
* Après le correctif :
  * La valeur `None`signifie « émettre l’attribut avec une valeur de `None`».
  * Une `SameSite` valeur de `(SameSiteMode)(-1)` entraîne la non-émission de l’attribut.

La valeur SameSite par défaut pour les cookies d’authentification par formulaire et d’état de session a été modifiée de `None` à `Lax`.

### <a name="summary-of-change-impact-on-browsers"></a>Résumé de l’impact des modifications sur les navigateurs

Si vous installez le correctif et émettez un cookie avec `SameSite.None`, l’une des deux situations suivantes se produit :
* Chrome V80 traite ce cookie en fonction de la nouvelle implémentation et n’applique pas les mêmes restrictions de site sur le cookie.
* Tout navigateur qui n’a pas été mis à jour pour prendre en charge la nouvelle implémentation suivra l’ancienne implémentation. L’ancienne implémentation indique :
  * Si vous voyez une valeur que vous ne comprenez pas, ignorez-la et basculez vers les mêmes restrictions de site.

Par conséquent, l’application s’arrête en mode chrome, ou vous pouvez interrompre de nombreux autres emplacements.

## <a name="history-and-changes"></a>Historique et modifications

La prise en charge de SameSite a été implémentée pour la première fois dans .NET 4.7.2 à l’aide de la [norme draft 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1).

Le 19 novembre 2019 mises à jour pour Windows a mis à jour .NET 4.7.2 + de la norme 2016 à la norme 2019. Des mises à jour supplémentaires sont à venir pour d’autres versions de Windows. Pour plus d'informations, consultez <xref:samesite/kbs-samesite>.

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

La norme 2016 SameSite stipule que les valeurs inconnues doivent être traitées comme des valeurs `SameSite=Strict`. Les applications accessibles depuis des navigateurs plus anciens qui prennent en charge la norme 2016 SameSite peuvent s’arrêter lorsqu’ils obtiennent une propriété SameSite avec la valeur `None`. Les applications Web doivent implémenter la détection du navigateur si elles envisagent de prendre en charge des navigateurs plus anciens. ASP.NET n’implémente pas la détection du navigateur, car les valeurs des agents utilisateur sont très volatiles et changent fréquemment.

L’approche de Microsoft pour résoudre le problème est de vous aider à implémenter des composants de détection de navigateur pour supprimer l’attribut `sameSite=None` des cookies si un navigateur est connu pour ne pas le prendre en charge. Le Conseil de Google était d’émettre des cookies doubles, l’un avec le nouvel attribut et l’autre sans l’attribut. Toutefois, nous considérons le Conseil de Google limité. Certains navigateurs, en particulier les navigateurs mobiles, ont des limites très réduites sur le nombre de cookies d’un site, ou un nom de domaine peut envoyer. L’envoi de plusieurs cookies, en particulier les cookies volumineux tels que les cookies d’authentification, peut atteindre la limite du navigateur mobile très rapidement, provoquant des échecs d’applications difficiles à diagnostiquer et à résoudre. En outre, il existe un grand écosystème de code et de composants tiers qui peut ne pas être mis à jour pour utiliser une approche de type double cookie.

Le code de détection du navigateur utilisé dans les exemples de projets de [ce référentiel GitHub]() est contenu dans deux fichiers

* [C#SameSiteSupport.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/SameSiteSupport.cs)
* [VB SameSiteSupport. vb](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/SameSiteSupport.vb)

Ces détections sont les agents de navigateur les plus courants qui prennent en charge la norme 2016 et pour lesquels l’attribut doit être complètement supprimé. Il n’est pas conçu comme une implémentation complète :

* Votre application peut voir les navigateurs que nos sites de test n’ont pas.
* Vous devez être prêt à ajouter des détections si nécessaire pour votre environnement.

Le mode de mise en place de la détection dépend de la version de .NET et de l’infrastructure Web que vous utilisez. Le code suivant peut être appelé sur le site d’appel de [HttpCookie](/dotnet/api/system.web.httpcookie) :

[!code-csharp[](sample/SameSiteCheck.cs?name=snippet)]

Consultez les rubriques suivantes sur les cookies ASP.NET 4.7.2 SameSite :

* [C#MVC](xref:samesite/csMVC)
* [C#WebForms](xref:samesite/CSharpWebForms)
* [WebForms VB](xref:samesite/vbWF)
* [MVC VB](xref:samesite/vbMVC)
<!--
* <xref:samesite/csMVC>
* <xref:samesite/CSharpWebForms>
* <xref:samesite/vbWF>
* <xref:samesite/vbMVC>
-->

### <a name="ensuring-your-site-redirects-to-https"></a>Vérification de la redirection de votre site vers HTTPs

Pour ASP.NET 4. x, WebForms et MVC, [la fonctionnalité de réécriture d’URL d’IIS](/iis/extensions/url-rewrite-module/creating-rewrite-rules-for-the-url-rewrite-module) peut être utilisée pour rediriger toutes les requêtes vers HTTPS. Le code XML suivant montre un exemple de règle :

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

Dans les installations locales de la [réécriture d’URL IIS](https://www.iis.net/downloads/microsoft/url-rewrite) , il s’agit d’une fonctionnalité facultative qui peut nécessiter l’installation de.

## <a name="test-apps-for-samesite-problems"></a>Tester des applications pour les problèmes SameSite

Vous devez tester votre application avec les navigateurs que vous prenez en charge et parcourir vos scénarios qui impliquent des cookies. Les scénarios de cookies impliquent généralement

* Formulaires de connexion
* Mécanismes de connexion externes tels que Facebook, Azure AD, OAuth et OIDC
* Pages acceptant les demandes d’autres sites
* Pages de votre application conçues pour être incorporées dans des iframes

Vous devez vérifier que les cookies sont créés, conservés et supprimés correctement dans votre application.

Les applications qui interagissent avec des sites distants, par exemple via une connexion tierce, doivent :

* Testez l’interaction sur plusieurs navigateurs.
* Appliquez la [détection et l’atténuation du navigateur](#sob) abordées dans ce document.

Testez les applications Web à l’aide d’une version du client qui peut s’abonner au nouveau comportement SameSite. Chrome, Firefox et chrome Edge ont tous des indicateurs de fonctionnalités d’abonnement qui peuvent être utilisés à des fins de test. Une fois que votre application applique les correctifs SameSite, testez-la avec des versions client plus anciennes, en particulier Safari. Pour plus d’informations, consultez [prise en charge des navigateurs plus anciens](#sob) dans ce document.

### <a name="test-with-chrome"></a>Tester avec chrome

Chrome 78 + donne des résultats trompeurs, car une atténuation temporaire est en place. Le chrome 78 + atténuation temporaire autorise les cookies datant de moins de deux minutes. Le chrome 76 ou 77 avec les indicateurs de test appropriés activés fournit des résultats plus précis. Pour tester le nouveau comportement SameSite, basculez `chrome://flags/#same-site-by-default-cookies` sur **activé**. Les anciennes versions de chrome (75 et versions antérieures) sont signalées pour échouer avec le nouveau paramètre de `None`. Consultez [prise en charge des navigateurs plus anciens](#sob) dans ce document.

Google ne rend pas les versions de chrome plus anciennes disponibles. Suivez les instructions de [Télécharger chrome](https://www.chromium.org/getting-involved/download-chromium) pour tester les anciennes versions de chrome. Ne téléchargez **pas** chrome à partir des liens fournis en recherchant les anciennes versions de chrome.

* [Chrome 76 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/664998/)
* [Chrome 74 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/638880/)
* Si vous n’utilisez pas une version 64 bits de Windows, vous pouvez utiliser la [visionneuse OmahaProxy](https://omahaproxy.appspot.com/) pour rechercher la branche de chrome qui correspond au chrome 74 (v 74.0.3729.108) à l’aide [des instructions fournies par chrome](https://www.chromium.org/getting-involved/download-chromium).

À partir de la version `80.0.3975.0`de la version de la balise de service, les tests d’atténuation temporaire de type Lax + postal peuvent être désactivés à des fins de test à l’aide du nouvel indicateur `--enable-features=SameSiteDefaultChecksMethodRigorously` pour permettre le test des sites et des services dans l’état final éventuel de la fonctionnalité dans laquelle l’atténuation a été supprimée. Pour plus d’informations, consultez [mises à jour](https://www.chromium.org/updates/same-site) des projets de chrome SameSite

#### <a name="test-with-chrome-80"></a>Test avec chrome 80 +

[Téléchargez](https://www.google.com/chrome/) une version de chrome qui prend en charge son nouvel attribut. Au moment de la rédaction de cet article, la version actuelle est chrome 80. Chrome 80 a besoin que l’indicateur `chrome://flags/#same-site-by-default-cookies` activé pour utiliser le nouveau comportement. Vous devez également activer (`chrome://flags/#cookies-without-same-site-must-be-secure`) pour tester le comportement à venir pour les cookies pour lesquels aucun attribut sameSite n’est activé. Le chrome 80 est sur la cible pour que le commutateur traite les cookies sans l’attribut en tant que `SameSite=Lax`, bien qu’avec une période de grâce minutée pour certaines requêtes. Pour désactiver la période de grâce minutée, chrome 80 peut être lancé avec l’argument de ligne de commande suivant :

`--enable-features=SameSiteDefaultChecksMethodRigorously`

Chrome 80 contient des messages d’avertissement dans la console du navigateur concernant les attributs sameSite manquants. Utilisez la touche F12 pour ouvrir la console du navigateur.

### <a name="test-with-safari"></a>Tester avec Safari

Safari 12 implémentait strictement le brouillon précédent et échoue lorsque la nouvelle valeur de `None` se trouve dans un cookie. `None` est évité par le biais du code de détection du navigateur qui [prend en charge les navigateurs plus anciens](#sob) dans ce document. Testez les connexions de style du système d’exploitation Safari 12, Safari 13 et WebKit à l’aide de MSAL, ADAL ou toute bibliothèque que vous utilisez. Le problème dépend de la version du système d’exploitation sous-jacent. OSX Mojave (10,14) et iOS 12 sont connus pour avoir des problèmes de compatibilité avec le nouveau comportement de SameSite. La mise à niveau du système d’exploitation vers OSX Catalina (10,15) ou iOS 13 résout le problème. Safari ne dispose pas actuellement d’un indicateur d’abonnement pour tester le nouveau comportement des spécifications.

### <a name="test-with-firefox"></a>Test avec Firefox

La prise en charge de Firefox pour la nouvelle norme peut être testée sur la version 68 + en choisissant dans la page `about:config` avec l’indicateur de fonctionnalité `network.cookie.sameSite.laxByDefault`. Il n’y a eu aucun rapport sur les problèmes de compatibilité avec les versions antérieures de Firefox.

### <a name="test-with-edge-legacy-browser"></a>Test avec le navigateur Edge (hérité)

Edge prend en charge l’ancien standard SameSite. Edge version 44 + ne présente aucun problème de compatibilité connu avec la nouvelle norme.

### <a name="test-with-edge-chromium"></a>Test avec Edge (chrome)

Les indicateurs SameSite sont définis sur la page `edge://flags/#same-site-by-default-cookies`. Aucun problème de compatibilité n’a été découvert avec le chrome Edge.

### <a name="test-with-electron"></a>Test avec électron

Les versions d’électrons incluent des versions plus anciennes de chrome. Par exemple, la version de l’électron utilisée par les équipes est le chrome 66, qui présente le comportement plus ancien. Vous devez effectuer vos propres tests de compatibilité avec la version d’électron que votre produit utilise. Consultez [prise en charge des navigateurs plus anciens](#sob).

## <a name="reverting-samesite-patches"></a>Rétablissement des correctifs SameSite

Vous pouvez rétablir le comportement sameSite mis à jour dans .NET Framework applications à son comportement précédent où l’attribut sameSite n’est pas émis pour une valeur de `None`, et restaurer les cookies d’authentification et de session pour ne pas émettre la valeur. Cette solution doit être considérée comme un *correctif extrêmement temporaire*, car les modifications du chrome rompent les demandes ou l’authentification de la publication externe pour les utilisateurs utilisant des navigateurs qui prennent en charge les modifications apportées à la norme.

### <a name="reverting-net-472-behavior"></a>Rétablissement du comportement de .NET 4.7.2

Mettez à jour le *fichier Web. config* pour inclure les paramètres de configuration suivants :

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

## <a name="additional-resources"></a>Ressources supplémentaires

* [Modifications de cookie SameSite à venir dans ASP.NET et ASP.NET Core](https://devblogs.microsoft.com/aspnet/upcoming-samesite-cookie-changes-in-asp-net-and-asp-net-core/)
* [Conseils pour le test et le débogage de SameSite par défaut et «SameSite = None ; Sécuriser les cookies](https://www.chromium.org/updates/same-site/test-debug)
* [Blog du chrome : développeurs : Préparez-vous à la nouvelle SameSite = None ; Sécuriser les paramètres de cookie](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)
* [Explication des cookies SameSite](https://web.dev/samesite-cookies-explained/)
* [Mises à jour chrome](https://www.chromium.org/updates/same-site)
* [Correctifs SameSite .NET](/aspnet/samesite/kbs-samesite)
* [Informations sur les mêmes sites pour les applications Web Azure](https://azure.microsoft.com/updates/app-service-samesite-cookie-update/)
* [Informations sur le même site Azure ActiveDirectory](/azure/active-directory/develop/howto-handle-samesite-cookie-changes-chrome-browser)
