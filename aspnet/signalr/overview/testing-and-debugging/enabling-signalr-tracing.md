---
uid: signalr/overview/testing-and-debugging/enabling-signalr-tracing
title: Activation du suivi Signalr | Microsoft Docs
author: bradygaster
description: Ce document explique comment activer et configurer le suivi pour les serveurs et les clients Signalr. Le suivi vous permet d’afficher des informations de diagnostic sur les événements...
ms.author: bradyg
ms.date: 08/08/2014
ms.assetid: 30060acb-be3e-4347-996f-3870f0c37829
msc.legacyurl: /signalr/overview/testing-and-debugging/enabling-signalr-tracing
msc.type: authoredcontent
ms.openlocfilehash: 34fe2cdb10c4b41a6e8cac7fb1741d53c02dfc80
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578833"
---
# <a name="enabling-signalr-tracing"></a>Activation du traçage SignalR

par [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Ce document explique comment activer et configurer le suivi pour les serveurs et les clients Signalr. Le suivi vous permet d’afficher des informations de diagnostic sur les événements dans votre application Signalr.
>
> Cette rubrique a été écrite à l’origine par Patrick Fletcher.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET Framework 4.5
> - Signalr version 2
>
>
>
> ## <a name="questions-and-comments"></a>Questions et commentaires
>
> N’hésitez pas à nous faire part de vos commentaires sur la façon dont vous aimez ce didacticiel et sur ce que nous pourrions améliorer dans les commentaires en bas de la page. Si vous avez des questions qui ne sont pas directement liées au didacticiel, vous pouvez les poster sur le [forum ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).

Lorsque le suivi est activé, une application Signalr crée des entrées de journal pour les événements. Vous pouvez consigner des événements à la fois à partir du client et du serveur. Le suivi sur le serveur journalise les événements de connexion, de fournisseur ScaleOut et de bus de messages. Le suivi sur le client journalise les événements de connexion. Dans Signalr 2,1 et versions ultérieures, le suivi sur le client enregistre le contenu complet des messages d’appel du Hub.

## <a name="contents"></a>Contenu

- [Activation du suivi sur le serveur](#server)

    - [Journalisation des événements du serveur dans des fichiers texte](#server_text)
    - [Journalisation des événements du serveur dans le journal des événements](#server_eventlog)
- [Activation du suivi dans le client .NET (applications de bureau Windows)](#net_client)

    - [Enregistrement des événements du client du Bureau à partir de la console](#desktop_console)
    - [Enregistrement des événements du client du Bureau à un fichier texte](#desktop_text)
- [Activation du suivi dans Windows Phone 8 clients](#phone)

    - [Journalisation des événements du client Windows Phone à l’interface utilisateur](#phone_ui)
    - [Journalisation des événements du client Windows Phone sur la console de débogage](#phone_debug)
- [Activation du suivi dans le client JavaScript](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a>Activation du suivi sur le serveur

Vous activez le suivi sur le serveur dans le fichier de configuration de l’application (App. config ou Web. config, selon le type de projet). Vous spécifiez les catégories d’événements que vous souhaitez consigner. Dans le fichier de configuration, vous spécifiez également si vous souhaitez enregistrer les événements dans un fichier texte, le journal des événements Windows ou un journal personnalisé à l’aide d’une implémentation de [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).

Les catégories d’événements de serveur incluent les types de messages suivants :

| `Source` | Messages |
| --- | --- |
| SignalR.SqlMessageBus | Installation du fournisseur ScaleOut du bus des messages SQL, opération de base de données, erreur et événements de délai d’attente |
| SignalR.ServiceBusMessageBus | Événements de création et d’abonnement, d’erreur et de messagerie des rubriques du fournisseur ScaleOut de service bus |
| SignalR.RedisMessageBus | Inverser les événements de connexion, de déconnexion et d’erreur du fournisseur ScaleOut |
| SignalR.ScaleoutMessageBus | Événements de messagerie ScaleOut |
| SignalR.Transports.WebSocketTransport | Événements liés à la connexion de transport WebSocket, à la déconnexion, à la messagerie et aux erreurs |
| SignalR.Transports.ServerSentEventsTransport | ServerSentEvents de la connexion de transport, de la déconnexion, de la messagerie et des événements d’erreur |
| SignalR.Transports.ForeverFrameTransport | ForeverFrame de la connexion de transport, de la déconnexion, de la messagerie et des événements d’erreur |
| SignalR.Transports.LongPollingTransport | LongPolling de la connexion de transport, de la déconnexion, de la messagerie et des événements d’erreur |
| SignalR.Transports.TransportHeartBeat | Événements de connexion de transport, de déconnexion et KeepAlive |
| SignalR.ReflectedHubDescriptorProvider | Événements de découverte du Hub |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a>Journalisation des événements du serveur dans des fichiers texte

Le code suivant montre comment activer le suivi pour chaque catégorie d’événement. Cet exemple configure l’application pour enregistrer des événements dans des fichiers texte.

**Code serveur XML pour l’activation du suivi**

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

Dans le code ci-dessus, l’entrée `SignalRSwitch` spécifie le [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) utilisé pour les événements envoyés au journal spécifié. Dans ce cas, il est défini sur `Verbose`, ce qui signifie que tous les messages de débogage et de suivi sont journalisés.

La sortie suivante montre les entrées du fichier `transports.log.txt` pour une application utilisant le fichier de configuration ci-dessus. Il montre une nouvelle connexion, une connexion supprimée et les événements de pulsation de transport.

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a>Journalisation des événements du serveur dans le journal des événements

Pour consigner les événements dans le journal des événements plutôt que dans un fichier texte, modifiez les valeurs des entrées dans le nœud `sharedListeners`. Le code suivant montre comment enregistrer les événements du serveur dans le journal des événements :

**Code du serveur XML pour l’enregistrement des événements dans le journal des événements**

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

Les événements sont consignés dans le journal des applications et sont disponibles via le observateur d’événements, comme indiqué ci-dessous :

![observateur d’événements l’indication des journaux Signalr](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> Lorsque vous utilisez le journal des événements, définissez le **TraceLevel** sur **erreur** pour conserver le nombre de messages gérables.

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a>Activation du suivi dans le client .NET (applications de bureau Windows)

Le client .NET peut consigner des événements dans la console, un fichier texte ou un journal personnalisé à l’aide d’une implémentation de [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).

Pour activer la journalisation dans le client .NET, définissez la propriété `TraceLevel` de la connexion sur une valeur [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) , et la propriété `TraceWriter` sur une instance [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) valide.

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a>Enregistrement des événements du client du Bureau à partir de la console

Le code C# suivant montre comment enregistrer des événements dans le client .net sur la console :

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a>Enregistrement des événements du client du Bureau à un fichier texte

Le code C# suivant montre comment enregistrer des événements dans le client .net dans un fichier texte :

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

La sortie suivante montre les entrées du fichier `ClientLog.txt` pour une application utilisant le fichier de configuration ci-dessus. Il montre le client qui se connecte au serveur et le Hub appelant une méthode cliente appelée `addMessage`:

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a>Activation du suivi dans Windows Phone 8 clients

Les applications signalr pour les applications Windows Phone utilisent le même client .NET que les applications de bureau, mais la [console. out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) et l’écriture dans un fichier avec [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) ne sont pas disponibles. Au lieu de cela, vous devez créer une implémentation personnalisée de [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) pour le traçage.

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a>Journalisation des événements du client Windows Phone à l’interface utilisateur

Le [code base de signalr](https://github.com/SignalR/SignalR/archive/master.zip) comprend un exemple de Windows Phone qui écrit la sortie de trace dans un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) à l’aide d’une implémentation [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) personnalisée appelée `TextBlockWriter`. Cette classe se trouve dans le projet **Samples/Microsoft. Aspnet. signalr. client. wp8. Samples** . Lorsque vous créez une instance de `TextBlockWriter`, transmettez le [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx)actuel et un [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) où il créera un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) à utiliser pour la sortie de trace :

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

La sortie de trace est ensuite écrite dans un nouveau [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) créé dans le [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) que vous avez passé :

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a>Journalisation des événements du client Windows Phone sur la console de débogage

Pour envoyer la sortie vers la console de débogage plutôt que l’interface utilisateur, créez une implémentation de [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) qui écrit dans la fenêtre de débogage et assignez-la à la propriété [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) de votre connexion :

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

Les informations de trace sont ensuite écrites dans la fenêtre de débogage de Visual Studio :

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a>Activation du suivi dans le client JavaScript

Pour activer la journalisation côté client sur une connexion, définissez la propriété `logging` sur l’objet de connexion avant d’appeler la méthode `start` pour établir la connexion.

**Code JavaScript du client pour activer le suivi sur la console du navigateur (avec le proxy généré)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

**Code JavaScript du client pour activer le suivi sur la console du navigateur (sans le proxy généré)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

Lorsque le suivi est activé, le client JavaScript enregistre les événements dans la console du navigateur. Pour accéder à la console du navigateur, consultez [surveillance des transports](../getting-started/introduction-to-signalr.md#MonitoringTransports).

La capture d’écran suivante montre un client JavaScript Signalr pour lequel le suivi est activé. Il montre les événements de connexion et d’appel de concentrateur dans la console du navigateur :

![Événements de suivi signalr dans la console du navigateur](enabling-signalr-tracing/_static/image4.png)
