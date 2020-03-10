---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: Traçage dans API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: Montre comment activer le traçage dans API Web ASP.NET.
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: a01acb649556d06ab9828ceab0fcbdf363bbc0d1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598552"
---
# <a name="tracing-in-aspnet-web-api-2"></a>Traçage dans API Web ASP.NET 2

par [Mike Wasson](https://github.com/MikeWasson)

> Lorsque vous essayez de déboguer une application Web, il n’y a pas de substitut pour un bon ensemble de journaux de suivi. Ce didacticiel montre comment activer le suivi dans API Web ASP.NET. Vous pouvez utiliser cette fonctionnalité pour suivre ce que fait l’infrastructure de l’API Web avant et après l’appel de votre contrôleur. Vous pouvez également l’utiliser pour effectuer le suivi de votre propre code.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
>
> - [Visual studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) (fonctionne également avec visual studio 2015)
> - API Web 2
> - [Microsoft. AspNet. WebApi. Tracing](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)

## <a name="enable-systemdiagnostics-tracing-in-web-api"></a>Activer le suivi System. Diagnostics dans l’API Web

Tout d’abord, nous allons créer un projet d’application Web ASP.NET. Dans Visual Studio, dans le menu **fichier** , sélectionnez **nouveau** > **projet**. Sous **modèles**, **Web**, sélectionnez **application Web ASP.net**.

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

Choisissez le modèle de projet d’API Web.

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

Dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet**, puis **console de gestion de package**.

Dans la fenêtre console du gestionnaire de package, tapez les commandes suivantes.

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

La première commande installe le dernier package de suivi d’API Web. Il met également à jour les packages d’API Web principaux. La deuxième commande met à jour le package WebApi. WebHost vers la dernière version.

> [!NOTE]
> Si vous souhaitez cibler une version spécifique de l’API Web, utilisez l’indicateur-version lorsque vous installez le package de suivi.

Ouvrez le fichier WebApiConfig.cs dans le dossier de démarrage de l’application\_. Ajoutez le code suivant à la méthode **Register** .

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

Ce code ajoute la classe [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) au pipeline de l’API Web. La classe **SystemDiagnosticsTraceWriter** écrit les traces dans [System. Diagnostics. trace](https://msdn.microsoft.com/library/system.diagnostics.trace).

Pour afficher les traces, exécutez l’application dans le débogueur. Dans un navigateur, accédez à `/api/values`.

![](tracing-in-aspnet-web-api/_static/image5.png)

Les instructions de suivi sont écrites dans la fenêtre sortie de Visual Studio. (Dans le menu **affichage** , sélectionnez **sortie**).

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

Étant donné que **SystemDiagnosticsTraceWriter** écrit des traces dans **System. Diagnostics. trace**, vous pouvez inscrire des écouteurs de suivi supplémentaires. par exemple, pour écrire des traces dans un fichier journal. Pour plus d’informations sur les enregistreurs de trace, consultez la rubrique relative aux [écouteurs de suivi](https://msdn.microsoft.com/library/4y5y10s7.aspx) sur MSDN.

### <a name="configuring-systemdiagnosticstracewriter"></a>Configuration de SystemDiagnosticsTraceWriter

Le code suivant montre comment configurer l’enregistreur de trace.

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

Vous pouvez contrôler deux paramètres :

- IsVerbose : si la valeur est false, chaque trace contient des informations minimales. Si la valeur est true, les traces contiennent davantage d’informations.
- MinimumLevel : définit le niveau de trace minimal. Les niveaux de suivi, dans l’ordre, sont Debug, info, Warn, Error et fatal.

## <a name="adding-traces-to-your-web-api-application"></a>Ajout de traces à votre application API Web

L’ajout d’un enregistreur de trace vous donne un accès immédiat aux traces créées par le pipeline de l’API Web. Vous pouvez également utiliser le générateur de trace pour tracer votre propre code :

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

Pour accéder au writer de trace, appelez **HttpConfiguration. services. GetTraceWriter**. À partir d’un contrôleur, cette méthode est accessible via la propriété **ApiController. Configuration** .

Pour écrire une trace, vous pouvez appeler directement la méthode **ITraceWriter. trace** , mais la classe [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) définit des méthodes d’extension plus conviviales. Par exemple, la méthode **info** présentée ci-dessus crée une trace avec les **informations**de niveau de suivi.

## <a name="web-api-tracing-infrastructure"></a>Infrastructure de suivi des API Web

Cette section décrit comment écrire un enregistreur de trace personnalisé pour l’API Web.

Le package Microsoft. AspNet. WebApi. Tracing repose sur une infrastructure de suivi plus générale dans l’API Web. Au lieu d’utiliser Microsoft. AspNet. WebApi. Tracing, vous pouvez également brancher une autre bibliothèque de suivi/journalisation, telle que [nlog](http://nlog-project.org/) ou [log4net](http://logging.apache.org/log4net/).

Pour collecter des traces, implémentez l’interface **ITraceWriter** . Voici un exemple simple :

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

La méthode **ITraceWriter. trace** crée une trace. L’appelant spécifie une catégorie et un niveau de suivi. La catégorie peut être n’importe quelle chaîne définie par l’utilisateur. Votre implémentation de **trace** doit effectuer les opérations suivantes :

1. Créez un nouveau **TraceRecord**. Initialisez-le avec la requête, la catégorie et le niveau de suivi, comme indiqué. Ces valeurs sont fournies par l’appelant.
2. Appelez le délégué *traceAction* . À l’intérieur de ce délégué, l’appelant est censé remplir le reste du **TraceRecord**.
3. Écrivez **TraceRecord**en utilisant une technique de journalisation de votre choix. L’exemple illustré ici appelle simplement **System. Diagnostics. trace**.

## <a name="setting-the-trace-writer"></a>Définition de l’enregistreur de trace

Pour activer le suivi, vous devez configurer l’API Web pour utiliser votre implémentation de **ITraceWriter** . Pour ce faire, utilisez l’objet **HttpConfiguration** , comme indiqué dans le code suivant :

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

Un seul enregistreur de suivi peut être actif. Par défaut, l’API Web définit un &quot;de non-op&quot; un traceur qui ne fait rien. (Le &quot;non-op&quot; audit existe afin que le code de traçage n’ait pas à vérifier si le writer de trace est **null** avant d’écrire une trace.)

## <a name="how-web-api-tracing-works"></a>Fonctionnement du suivi d’API Web

Le suivi dans l’API Web utilise un modèle de *façade* : lorsque le suivi est activé, l’API Web encapsule diverses parties du pipeline de requête avec des classes qui effectuent des appels de trace.

Par exemple, lors de la sélection d’un contrôleur, le pipeline utilise l’interface **IHttpControllerSelector** . Lorsque le suivi est activé, le pipeline insère une classe qui implémente **IHttpControllerSelector** mais appelle l’implémentation réelle :

![Le suivi de l’API Web utilise le modèle de façade.](tracing-in-aspnet-web-api/_static/image8.png)

Les avantages de cette conception sont les suivants :

- Si vous n’ajoutez pas d’enregistreur de trace, les composants de suivi ne sont pas instanciés et n’ont pas d’impact sur les performances.
- Si vous remplacez des services par défaut tels que **IHttpControllerSelector** par votre propre implémentation personnalisée, le suivi n’est pas affecté, car le suivi est effectué par l’objet de wrapper.

Vous pouvez également remplacer l’ensemble de l’infrastructure de trace de l’API Web par votre propre infrastructure personnalisée, en remplaçant le service **ITraceManager** par défaut :

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

Implémentez **ITraceManager. Initialize** pour initialiser votre système de suivi. N’oubliez pas que cela remplace l' *intégralité* de l’infrastructure de trace, y compris l’ensemble du code de traçage intégré à l’API Web.
