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
# <a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a>Ce qu’il ne faut pas faire dans ASP.NET et ce qu’il faut faire à la place

> Cette rubrique décrit plusieurs erreurs courantes que les gens font dans des projets Web ASP.NET. Il fournit des recommandations pour ce que vous devez faire pour éviter ces erreurs courantes. Elle est basée sur une [Présentation](http://vimeo.com/68390507) de **Damian Edwards** à la Conférence des développeurs norvégiens.

## <a name="disclaimer"></a>Exclusion de responsabilité

Cette rubrique n’est pas un guide complet pour garantir la sécurité et l’efficacité de votre application. Vous devez toujours suivre les meilleures pratiques en matière de sécurité et de performances qui ne sont pas décrites dans cette rubrique. Elle suggère uniquement comment éviter les erreurs courantes liées aux processus et aux classes .NET.

## <a name="overview"></a>Présentation

Cette rubrique contient les sections suivantes :

- [Conformité aux normes](#standards)

    - [Adaptateurs de contrôle](#adapters)
    - [Propriétés de style sur les contrôles](#styleprop)
    - [Rappels de page et de contrôle](#callback)
    - [Détection des fonctionnalités du navigateur](#browsercap)
- [Sécurité](#security)

    - [Validation de la demande](#validation)
    - [Authentification par formulaire et session sans cookies](#cookieless)
    - [EnableViewStateMac](#viewstatemac)
    - [Confiance moyenne](#medium)
    - [&lt;appSettings&gt;](#appsettings)
    - [UrlPathEncode](#urlpathencode)
- [Fiabilité et performances](#performance)

    - [PreSendRequestHeaders et PreSendRequestContent](#presend)
    - [Événements de page asynchrones avec Web Forms](#asyncevents)
    - [Travail d’incendie et d’oubli](#fire)
    - [Corps d’entité de la requête](#requestentity)
    - [Response. Redirect et Response. end](#redirect)
    - [EnableViewState et ViewStateMode](#viewstatemode)
    - [SqlMembershipProvider](#sqlprovider)
    - [Longues demandes (> 110 secondes)](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a>Conformité aux normes

<a id="adapters"></a>

### <a name="control-adapters"></a>Adaptateurs de contrôle

Recommandation : Arrêtez d’utiliser des adaptateurs de contrôle pour le rendu adaptatif et utilisez à la place des requêtes de média CSS et du code HTML conforme aux normes.

Les adaptateurs de contrôles ont été introduits dans .NET 2,0 pour afficher le code de présentation qui a été personnalisé pour différents appareils et environnements. À présent, ce rendu adaptatif peut être effectué avec CSS et HTML. Vous devez arrêter d’utiliser des adaptateurs de contrôle et convertir les adaptateurs existants en CSS et HTML.

Pour plus d’informations, consultez [requêtes de média](http://www.w3.org/TR/css3-mediaqueries/) et [Comment : ajouter des pages mobiles à votre application ASP.NET Web Forms/MVC](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a>Propriétés de style sur les contrôles

Recommandation : Arrêtez la définition des valeurs de style dans le balisage de contrôle et définissez à la place les valeurs de mise en forme dans les feuilles de style CSS.

Les contrôles serveur Web contiennent des dizaines de propriétés qui peuvent être utilisées pour définir des propriétés de style en ligne. Par exemple, la propriété ForeColor définit la couleur du texte d’un contrôle. Vous pouvez obtenir ce même effet plus efficacement par le biais de feuilles de style CSS. Les feuilles de style vous permettent de centraliser les valeurs de style et d’éviter de définir ces valeurs dans l’ensemble de votre application.

L’exemple suivant montre une classe CSS qui définit le texte sur rouge.

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

L’exemple suivant montre comment appliquer de manière dynamique la classe CSS.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a>Rappels de page et de contrôle

Recommandation : Arrêtez d’utiliser les rappels de page et de contrôle et utilisez à la place l’un des éléments suivants : AJAX, UpdatePanel, les méthodes d’action MVC, l’API Web ou Signalr.

Dans les versions antérieures de ASP.NET, les méthodes de rappel de page et de contrôle vous permettaient de mettre à jour une partie de la page Web sans actualiser une page entière. Vous pouvez désormais effectuer des mises à jour de pages partielles via [Ajax](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), l' [API Web](../../../web-api/index.md) ou [signalr](../../../signalr/index.md). Vous devez cesser d’utiliser des méthodes de rappel, car elles peuvent provoquer des problèmes avec les URL conviviales et le routage. Par défaut, les contrôles n’activent pas les méthodes de rappel, mais si vous avez activé cette fonctionnalité dans un contrôle, vous devez la désactiver.

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a>Détection des fonctionnalités du navigateur

Recommandation : Arrêtez d’utiliser la détection de fonctionnalité de navigateur statique et utilisez à la place la détection de fonctionnalité dynamique.

Dans les versions antérieures de ASP.NET, les fonctionnalités prises en charge pour chaque navigateur étaient stockées dans un fichier XML. La détection de la prise en charge des fonctionnalités par le biais d’une recherche statique n’est pas la meilleure approche. À présent, vous pouvez détecter de manière dynamique les fonctionnalités prises en charge par un navigateur à l’aide d’une infrastructure de détection des fonctionnalités, telle que [Modernizr](http://modernizr.com/). La détection de fonctionnalités détermine la prise en charge en tentant d’utiliser une méthode ou une propriété, puis de vérifier si le navigateur a produit le résultat souhaité. Par défaut, Modernizr est inclus dans les modèles d’application Web.

<a id="security"></a>

## <a name="security"></a>Sécurité

<a id="validation"></a>

### <a name="request-validation"></a>Validation des demandes

Recommandation : valider les entrées d’utilisateur et coder la sortie des utilisateurs.

La validation de la demande est une fonctionnalité de ASP.NET qui inspecte chaque requête et arrête la requête si une menace perçue est détectée. Ne dépendez pas de la validation des demandes pour sécuriser votre application contre les attaques de script entre sites. Au lieu de cela, validez toutes les entrées des utilisateurs et encodez la sortie. Dans certains cas limités, vous pouvez utiliser des expressions régulières pour valider l’entrée, mais dans des cas plus complexes, vous devez valider les entrées d’utilisateur à l’aide de classes .NET qui déterminent si la valeur correspond aux valeurs autorisées.

L’exemple suivant montre comment utiliser une méthode statique dans la classe URI pour déterminer si l’URI fourni par un utilisateur est valide.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

Toutefois, pour vérifier suffisamment l’URI, vous devez également vérifier qu’il spécifie `http` ou `https`. L’exemple suivant utilise des méthodes d’instance pour vérifier que l’URI est valide.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

Avant de restituer l’entrée utilisateur au format HTML ou d’inclure une entrée d’utilisateur dans une requête SQL, encodez les valeurs pour vous assurer que le code malveillant n’est pas inclus.

Vous pouvez encoder en HTML la valeur dans le balisage avec la syntaxe &lt;% :%&gt;, comme indiqué ci-dessous.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

Ou, dans syntaxe Razor, vous pouvez encoder en HTML avec @, comme indiqué ci-dessous.

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

L’exemple suivant montre comment encoder en HTML une valeur dans du code-behind.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

Pour encoder en toute sécurité une valeur pour les commandes SQL, utilisez des paramètres de commande tels que [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx). <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a>Authentification par formulaire et session sans cookies

Recommandation : exiger des cookies.

La transmission des informations d’authentification dans la chaîne de requête n’est pas sécurisée. Par conséquent, exigez des cookies lorsque votre application comprend une authentification. Si votre cookie stocke des informations sensibles, envisagez d’exiger SSL pour le cookie.

L’exemple suivant montre comment spécifier dans le fichier Web. config que l’authentification par formulaire requiert un cookie transmis via SSL.

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a>EnableViewStateMac

Recommandation : n’affectez jamais la valeur false.

Par défaut, EnableViewStateMac a la valeur true. Même si votre application n’utilise pas l’état d’affichage, n’affectez pas la valeur false à EnableViewStateMac. Si vous définissez cette valeur sur false, votre application sera vulnérable aux scripts inter-sites.

À compter de ASP.NET 4.5.2, le runtime applique **enableViewStateMac = true**. Même si vous lui affectez la valeur false, le runtime ignore cette valeur et poursuit avec la valeur définie sur true. Pour plus d’informations, consultez [ASP.net 4.5.2 et EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).

L’exemple suivant montre comment définir EnableViewStateMac sur true. Vous n’avez pas besoin de définir cette valeur sur true, car elle est true par défaut. Toutefois, si vous avez défini la valeur false sur n’importe quelle page de votre application, vous devez immédiatement corriger cette valeur.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a>Confiance moyenne

Recommandation : ne dépendez pas de la confiance moyenne (ou de tout autre niveau de confiance) comme limite de sécurité.

La confiance partielle ne protège pas correctement votre application et ne doit pas être utilisée. Au lieu de cela, utilisez une confiance totale et isolez les applications non approuvées dans des pools d’applications distincts. Exécutez également chaque pool d’applications sous une identité unique. Pour plus d’informations, consultez [confiance partielle ASP.net ne garantit pas l’isolation des applications](https://support.microsoft.com/kb/2698981).

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a>&lt;appSettings&gt;

Recommandation : ne désactivez pas les paramètres de sécurité dans &lt;élément appSettings&gt;.

L’élément appSettings contient de nombreuses valeurs requises pour les mises à jour de sécurité. Vous ne devez pas modifier ou désactiver ces valeurs. Si vous devez désactiver ces valeurs lors du déploiement d’une mise à jour, réactivez-la immédiatement après avoir terminé le déploiement.

Pour plus d’informations, consultez [ASP.net appSettings, élément](https://msdn.microsoft.com/library/hh975440.aspx).

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a>UrlPathEncode

Recommandation : utilisez [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) à la place.

La méthode UrlPathEncode a été ajoutée au .NET Framework pour résoudre un problème de compatibilité de navigateur très spécifique. Il n’encode pas correctement une URL et ne protège pas votre application des scripts inter-sites. Vous ne devez jamais l’utiliser dans votre application. Utilisez à la place [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).

L’exemple suivant montre comment passer une URL encodée en tant que paramètre de chaîne de requête pour un contrôle de lien hypertexte.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a>Fiabilité et performances

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a>PreSendRequestHeaders et PreSendRequestContent

Recommandation : n’utilisez pas ces événements avec les modules managés. Au lieu de cela, écrivez un module IIS natif pour effectuer la tâche requise. Consultez [création de modules http en code natif](https://msdn.microsoft.com/library/ms693629.aspx).

Vous pouvez utiliser les événements [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) et [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) avec les modules IIS natifs.
> [!WARNING]
> N’utilisez pas `PreSendRequestHeaders` et `PreSendRequestContent` avec les modules managés qui implémentent `IHttpModule`. La définition de ces propriétés peut provoquer des problèmes avec les demandes asynchrones. La combinaison du routage demandé par l’application (ARR) et de WebSockets peut entraîner des exceptions de violation d’accès qui peuvent provoquer le blocage de w3wp. Par exemple, iiscore ! W3_CONTEXT_BASE :: GetIsLastNotification + 68 dans iiscore. dll a provoqué une exception de violation d’accès (0xC0000005).

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a>Événements de page asynchrones avec Web Forms

Recommandation : dans Web Forms, évitez d’écrire des méthodes Async void pour les événements de cycle de vie de la page et utilisez à la place [page. RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) pour le code asynchrone.

Quand vous marquez un événement de page avec **Async** et **void**, vous ne pouvez pas déterminer à quel moment le code asynchrone est terminé. Utilisez plutôt page. RegisterAsyncTask pour exécuter le code asynchrone d’une manière qui vous permet d’effectuer le suivi de son achèvement.

L’exemple suivant illustre un gestionnaire de clic de bouton qui contient du code asynchrone. Cet exemple comprend la lecture d’une valeur de chaîne de manière asynchrone, qui est fournie uniquement sous la forme d’un exemple simplifié d’une tâche asynchrone, et non comme une pratique recommandée.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

Si vous utilisez des tâches asynchrones, définissez le Framework cible du runtime http sur 4,5 (ou version ultérieure) dans le fichier Web. config. La définition de la version cible du .NET Framework sur 4,5 active le nouveau contexte de synchronisation qui a été ajouté dans .NET 4,5. Cette valeur est définie par défaut dans les nouveaux projets dans Visual Studio, mais elle n’est pas définie si vous travaillez avec un projet existant.

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a>Travail d’incendie et d’oubli

Recommandation : lors du traitement d’une demande dans ASP.NET, évitez de lancer un travail d’activation et d’oubli (par exemple, appeler la méthode ThreadPool. QueueUserWorkItem ou créer un minuteur qui appelle un délégué à plusieurs reprises).

Si votre application a un travail Fire-and-oublie qui s’exécute dans ASP.NET, votre application peut être désynchronisée. À tout moment, le domaine d’application peut être détruit, ce qui signifie que votre processus en cours peut ne plus correspondre à l’état actuel de l’application.

Vous devez déplacer ce type de travail en dehors de ASP.NET. Vous pouvez utiliser des tâches Web, un service Windows ou un rôle de travail dans Azure pour effectuer un travail en cours et exécuter ce code à partir d’un autre processus.

Si vous devez effectuer cette opération dans ASP.NET, vous pouvez ajouter le package NuGet appelé [Webbackgroundr](http://www.nuget.org/packages/webbackgrounder) pour exécuter le code.

<a id="requestentity"></a>

### <a name="request-entity-body"></a>Corps d’entité de la requête

Recommandation : Évitez de lire Request. Form ou Request. InputStream avant l’événement Execute du gestionnaire.

Au plus tôt, vous devez lire à partir de Request. Form ou Request. InputStream pendant l’événement d’exécution du gestionnaire. Dans MVC, le contrôleur est le gestionnaire et l’événement d’exécution est lors de l’exécution de la méthode d’action. Dans Web Forms, la page est le gestionnaire et l’événement d’exécution est le déclenchement de l’événement page. init. Si vous lisez le corps d’entité de requête antérieur à l’événement d’exécution, vous interfèrez avec le traitement de la requête.

Si vous devez lire le corps d’entité de la requête avant l’événement d’exécution, utilisez [Request. GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) ou [Request. GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx). Lorsque vous utilisez GetBufferlessInputStream, vous recevez le flux brut de la demande et assumez la responsabilité du traitement de l’intégralité de la requête. Après l’appel de GetBufferlessInputStream, Request. Form et Request. InputStream ne sont pas disponibles parce qu’ils n’ont pas été remplis par ASP.NET. Lorsque vous utilisez GetBufferedInputStream, vous recevez une copie du flux de la requête. Request. Form et Request. InputStream sont toujours disponibles plus tard dans la requête, car ASP.NET remplit l’autre copie.

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a>Response. Redirect et Response. end

Recommandation : Tenez compte des différences de gestion des threads après l’appel de [Response. Redirect (String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).

La méthode [Response. Redirect (String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) appelle la méthode Response. end. Dans un processus synchrone, l’appel de Request. Redirect provoque l’abandon immédiat du thread actuel. Toutefois, dans un processus asynchrone, l’appel de Response. Redirect n’abandonne pas le thread actuel, de sorte que l’exécution du code continue pour la demande. Dans un processus asynchrone, vous devez retourner la tâche à partir de la méthode pour arrêter l’exécution du code.

Dans un projet MVC, vous ne devez pas appeler Response. Redirect. Au lieu de cela, retournez un RedirectResult.

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a>EnableViewState et ViewStateMode

Recommandation : utilisez ViewStateMode au lieu de EnableViewState pour fournir un contrôle granulaire sur les contrôles qui utilisent l’état d’affichage.

Lorsque vous affectez à EnableViewState la valeur false dans la directive de page, l’état d’affichage est désactivé pour tous les contrôles de la page et ne peut pas être activé. Si vous souhaitez activer l’état d’affichage pour certains contrôles de votre page, affectez à ViewStateMode la valeur Disabled pour la page.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

Ensuite, affectez à ViewStateMode la valeur activé uniquement sur les contrôles qui ont réellement besoin d’un état d’affichage.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

En activant l’état d’affichage uniquement pour les contrôles qui en ont besoin, vous pouvez réduire la taille de l’état d’affichage de vos pages Web.

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a>SqlMembershipProvider

Recommandation : utilisez Fournisseurs universels.

Dans les modèles de projet actuels, SqlMembershipProvider a été remplacé par [fournisseurs universels ASP.net](http://www.nuget.org/packages/Microsoft.AspNet.Providers), qui est disponible sous forme de package NuGet. Si vous utilisez SqlMembershipProvider dans un projet qui a été généré avec une version antérieure des modèles, vous devez basculer vers Fournisseurs universels. L’Fournisseurs universels utiliser toutes les bases de données prises en charge par Entity Framework.

Pour plus d’informations, consultez [Présentation de fournisseurs universels ASP.net](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a>Demandes longues (> 110 secondes)

Recommandation : utilisez [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) ou [signalr](../../../signalr/index.md) pour les clients connectés et utilisez des opérations d’e/s asynchrones.

Les requêtes de longue durée peuvent entraîner des résultats imprévisibles et des performances médiocres dans votre application Web. Le paramètre de délai d’expiration par défaut pour une demande est de 110 secondes. Si vous utilisez l’état de session avec une demande longue, ASP.NET libère le verrou sur l’objet de session après 110 secondes. Toutefois, votre application peut être au milieu d’une opération sur l’objet de session lorsque le verrou est libéré, et l’opération peut ne pas se terminer correctement. Si une deuxième demande de l’utilisateur est bloquée pendant que la première demande est en cours d’exécution, la deuxième demande peut accéder à l’objet de session dans un état incohérent.

Si votre application comprend des opérations d’e/s bloquantes (ou synchrones), l’application ne répondra pas.

Pour améliorer les performances, utilisez les opérations d’e/s asynchrones dans le .NET Framework. En outre, utilisez WebSockets ou Signalr pour connecter les clients au serveur. Ces fonctionnalités sont conçues pour gérer efficacement les requêtes à long terme.
