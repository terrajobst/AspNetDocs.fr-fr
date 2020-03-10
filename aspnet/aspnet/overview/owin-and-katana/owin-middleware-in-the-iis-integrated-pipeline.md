---
uid: aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
title: Intergiciel OWIN dans le pipeline intégré IIS | Microsoft Docs
author: Praburaj
description: Cet article explique comment exécuter des composants de middleware OWIN (OMCs) dans le pipeline intégré IIS et comment définir l’événement de pipeline sur lequel un OMC s’exécute. Tu devrais...
ms.author: riande
ms.date: 11/07/2013
ms.assetid: d031c021-33c2-45a5-bf9f-98f8fa78c2ab
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
msc.type: authoredcontent
ms.openlocfilehash: 7d157fb6bd9e2ae9b55af41ef06c1eb5e6310ce1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78617137"
---
# <a name="owin-middleware-in-the-iis-integrated-pipeline"></a>Intergiciel OWIN dans le pipeline intégré IIS

par [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://twitter.com/RickAndMSFT)

> Cet article explique comment exécuter des composants de middleware OWIN (OMCs) dans le pipeline intégré IIS et comment définir l’événement de pipeline sur lequel un OMC s’exécute. Avant de lire ce didacticiel, vous devez consulter [une vue d’ensemble de](an-overview-of-project-katana.md) la détection de la [classe de démarrage](owin-startup-class-detection.md) Project Katana et OWIN. Ce didacticiel a été rédigé par Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Chris Ross, Praburaj Thiagarajan et Howard Dierking ( [@howard\_Dierking](https://twitter.com/howard_dierking) ).

Bien que les composants de middleware [OWIN](an-overview-of-project-katana.md) (OMCs) soient principalement conçus pour s’exécuter dans un pipeline indépendant du serveur, il est possible d’exécuter également un OMC dans le pipeline intégré IIS (le**mode classique n’est *pas* pris en charge**). Un OMC peut être mis en œuvre dans le pipeline intégré d’IIS en installant le package suivant à partir de la console du gestionnaire de package (PMC) :

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample1.cmd)]

Cela signifie que toutes les infrastructures d’application, même celles qui ne sont pas encore en mesure d’être exécutées en dehors d’IIS et de System. Web, peuvent tirer parti des composants d’intergiciel (middleware) OWIN existants. 

> [!NOTE]
> Tous les packages de `Microsoft.Owin.Security.*` livrés avec le nouveau système d’identité dans Visual Studio 2013 (par exemple : les cookies, le compte Microsoft, Google, Facebook, Twitter, le [jeton du porteur](http://self-issued.info/docs/draft-ietf-oauth-v2-bearer.html), OAuth, le serveur d’autorisation, JWT, Azure Active Directory et Active Directory Federation Services) sont créés en tant que OMCs et peuvent être utilisés dans les scénarios auto-hébergés et hébergés par IIS.

## <a name="how-owin-middleware-executes-in-the-iis-integrated-pipeline"></a>Mode d’exécution du middleware OWIN dans le pipeline intégré IIS

Pour les applications console OWIN, le pipeline d’application créé à l’aide de la [configuration de démarrage](owin-startup-class-detection.md) est défini par l’ordre dans lequel les composants sont ajoutés à l’aide de la méthode `IAppBuilder.Use`. Autrement dit, le pipeline OWIN dans le runtime [Katana](an-overview-of-project-katana.md) traitera OMCs dans l’ordre dans lequel ils ont été inscrits à l’aide de `IAppBuilder.Use`. Dans le pipeline intégré IIS, le pipeline de requête se compose de [modules HttpModule](https://msdn.microsoft.com/library/ms178468(v=vs.85).aspx) souscrits à un ensemble prédéfini d’événements de pipeline tels que [beginRequest](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx), [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), [AuthorizeRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx), etc.

Si nous comparons un OMC à celui d’un [HttpModule](https://msdn.microsoft.com/library/zec9k340(v=vs.85).aspx) dans le monde ASP.net, un OMC doit être inscrit auprès de l’événement de pipeline prédéfini approprié. Par exemple, l' `MyModule` HttpModule sera appelée lorsqu’une requête arrive à l’étape [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) dans le pipeline :

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample2.cs?highlight=10)]

Pour qu’un OMC participe à ce même ordre d’exécution basé sur les événements, le code d’exécution [Katana](an-overview-of-project-katana.md) parcourt la [configuration de démarrage](owin-startup-class-detection.md) et s’abonne à un événement de pipeline intégré à chacun des composants de l’intergiciel (middleware). Par exemple, l’OMC et le code d’inscription suivants vous permettent de voir l’enregistrement des événements par défaut des composants de l’intergiciel (middleware). (Pour obtenir des instructions plus détaillées sur la création d’une classe de démarrage OWIN, consultez détection de la [classe de démarrage OWIN](owin-startup-class-detection.md).)

1. Créez un projet d’application Web vide et nommez-le **owin2**.
2. À partir de la console du gestionnaire de package (PMC), exécutez la commande suivante : 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample3.cmd)]
3. Ajoutez un `OWIN Startup Class` et nommez-le `Startup`. Remplacez le code généré par ce qui suit (les modifications sont mises en surbrillance) :  

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample4.cs?highlight=5-7,15-36)]
4. Appuyez sur F5 pour exécuter l’application.

La configuration de démarrage définit un pipeline avec trois composants de l’intergiciel (middleware), les deux premiers éléments qui affichent les informations de diagnostic et le dernier répondant aux événements (et affichent également des informations de diagnostic). La méthode `PrintCurrentIntegratedPipelineStage` affiche l’événement de pipeline intégré sur lequel cet intergiciel (middleware) est appelé et un message. La fenêtre de sortie affiche les éléments suivants :

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample5.cmd)]

Le runtime Katana a mappé chacun des composants de l’intergiciel OWIN à [PreExecuteRequestHandler](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx) par défaut, ce qui correspond à l’événement de pipeline IIS [PreRequestHandlerExecute](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx).

## <a name="stage-markers"></a>Marqueurs intermédiaires

Vous pouvez marquer OMCs pour qu’elle s’exécute à des étapes spécifiques du pipeline à l’aide de la méthode d’extension `IAppBuilder UseStageMarker()`. Pour exécuter un ensemble de composants d’intergiciel (middleware) au cours d’une phase particulière, insérez une marque de phase juste après le dernier composant correspondant au jeu lors de l’inscription. Il existe des règles sur la phase du pipeline sur laquelle vous pouvez exécuter l’intergiciel (middleware) et les composants de commande doivent s’exécuter (les règles sont expliquées plus loin dans ce didacticiel). Ajoutez la méthode `UseStageMarker` au code `Configuration` comme indiqué ci-dessous :

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample6.cs?highlight=13,19)]

L’appel de `app.UseStageMarker(PipelineStage.Authenticate)` configure tous les composants de l’intergiciel (middleware) précédemment inscrits (dans ce cas, nos deux composants de diagnostic) pour qu’ils s’exécutent à l’étape d’authentification du pipeline. Le dernier composant intergiciel (qui affiche les diagnostics et répond aux demandes) s’exécute à l’étape `ResolveCache` (événement [ResolveRequestCache](https://msdn.microsoft.com/library/system.web.httpapplication.resolverequestcache.aspx) ).

Appuyez sur F5 pour exécuter l’application. La fenêtre sortie affiche ce qui suit :

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample7.cmd)]

## <a name="stage-marker-rules"></a>Règles de marqueur d’étape

Les composants de l’intergiciel (middleware) Owin peuvent être configurés pour s’exécuter aux événements de la phase de pipeline OWIN suivants :

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample8.cs)]

1. Par défaut, OMCs s’exécute au dernier événement (`PreHandlerExecute`). C’est la raison pour laquelle notre premier exemple de code affichait « PreExecuteRequestHandler ».
2. Vous pouvez utiliser la méthode a `app.UseStageMarker` pour inscrire un OMC à exécuter précédemment, à n’importe quelle étape du pipeline OWIN répertorié dans l’énumération `PipelineStage`.
3. Le pipeline OWIN et le pipeline IIS sont triés. par conséquent, les appels à `app.UseStageMarker` doivent être dans l’ordre. Vous ne pouvez pas définir le gestionnaire d’événements sur un événement qui précède le dernier événement enregistré auprès de à `app.UseStageMarker`. Par exemple, *après* l’appel de :

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample9.cmd)]

   les appels à `app.UseStageMarker` passant `Authenticate` ou `PostAuthenticate` ne sont pas honorés, et aucune exception n’est levée. OMCs s’exécute à l’étape la plus récente, qui est `PreHandlerExecute`par défaut. Les marqueurs intermédiaires sont utilisés pour les exécuter plus tôt. Si vous spécifiez des marqueurs intermédiaires dans le désordre, nous arrondissons au marqueur précédent. En d’autres termes, l’ajout d’un marqueur d’étape indique « exécuter au plus tard à l’étape X ». L’OMC s’exécute sur le marqueur d’étape le plus précoce ajouté après dans le pipeline OWIN.
4. L’étape la plus précoce des appels à `app.UseStageMarker` gagne. Par exemple, si vous basculez l’ordre des appels de `app.UseStageMarker` à partir de l’exemple précédent :

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample10.cs?highlight=13,19)]

   La fenêtre sortie affiche : 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample11.cmd)]

   Les OMCs s’exécutent tous à l’étape `AuthenticateRequest`, car le dernier OMC inscrit avec l’événement `Authenticate` et l’événement `Authenticate` précède tous les autres événements.
