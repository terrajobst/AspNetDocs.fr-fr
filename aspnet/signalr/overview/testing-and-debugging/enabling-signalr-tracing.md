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
# <a name="enabling-signalr-tracing"></a><span data-ttu-id="af575-104">Activation du traçage SignalR</span><span class="sxs-lookup"><span data-stu-id="af575-104">Enabling SignalR Tracing</span></span>

<span data-ttu-id="af575-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="af575-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="af575-106">Ce document explique comment activer et configurer le suivi pour les serveurs et les clients Signalr.</span><span class="sxs-lookup"><span data-stu-id="af575-106">This document describes how to enable and configure tracing for SignalR servers and clients.</span></span> <span data-ttu-id="af575-107">Le suivi vous permet d’afficher des informations de diagnostic sur les événements dans votre application Signalr.</span><span class="sxs-lookup"><span data-stu-id="af575-107">Tracing enables you to view diagnostic information about events in your SignalR application.</span></span>
>
> <span data-ttu-id="af575-108">Cette rubrique a été écrite à l’origine par Patrick Fletcher.</span><span class="sxs-lookup"><span data-stu-id="af575-108">This topic was originally written by Patrick Fletcher.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="af575-109">Versions logicielles utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="af575-109">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="af575-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="af575-110">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="af575-111">.NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="af575-111">.NET Framework 4.5</span></span>
> - <span data-ttu-id="af575-112">Signalr version 2</span><span class="sxs-lookup"><span data-stu-id="af575-112">SignalR version 2</span></span>
>
>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="af575-113">Questions et commentaires</span><span class="sxs-lookup"><span data-stu-id="af575-113">Questions and comments</span></span>
>
> <span data-ttu-id="af575-114">N’hésitez pas à nous faire part de vos commentaires sur la façon dont vous aimez ce didacticiel et sur ce que nous pourrions améliorer dans les commentaires en bas de la page.</span><span class="sxs-lookup"><span data-stu-id="af575-114">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="af575-115">Si vous avez des questions qui ne sont pas directement liées au didacticiel, vous pouvez les poster sur le [forum ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="af575-115">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<span data-ttu-id="af575-116">Lorsque le suivi est activé, une application Signalr crée des entrées de journal pour les événements.</span><span class="sxs-lookup"><span data-stu-id="af575-116">When tracing is enabled, a SignalR application creates log entries for events.</span></span> <span data-ttu-id="af575-117">Vous pouvez consigner des événements à la fois à partir du client et du serveur.</span><span class="sxs-lookup"><span data-stu-id="af575-117">You can log events from both the client and the server.</span></span> <span data-ttu-id="af575-118">Le suivi sur le serveur journalise les événements de connexion, de fournisseur ScaleOut et de bus de messages.</span><span class="sxs-lookup"><span data-stu-id="af575-118">Tracing on the server logs connection, scaleout provider, and message bus events.</span></span> <span data-ttu-id="af575-119">Le suivi sur le client journalise les événements de connexion.</span><span class="sxs-lookup"><span data-stu-id="af575-119">Tracing on the client logs connection events.</span></span> <span data-ttu-id="af575-120">Dans Signalr 2,1 et versions ultérieures, le suivi sur le client enregistre le contenu complet des messages d’appel du Hub.</span><span class="sxs-lookup"><span data-stu-id="af575-120">In SignalR 2.1 and later, tracing on the client logs the full content of hub invocation messages.</span></span>

## <a name="contents"></a><span data-ttu-id="af575-121">Contenu</span><span class="sxs-lookup"><span data-stu-id="af575-121">Contents</span></span>

- [<span data-ttu-id="af575-122">Activation du suivi sur le serveur</span><span class="sxs-lookup"><span data-stu-id="af575-122">Enabling tracing on the server</span></span>](#server)

    - [<span data-ttu-id="af575-123">Journalisation des événements du serveur dans des fichiers texte</span><span class="sxs-lookup"><span data-stu-id="af575-123">Logging server events to text files</span></span>](#server_text)
    - [<span data-ttu-id="af575-124">Journalisation des événements du serveur dans le journal des événements</span><span class="sxs-lookup"><span data-stu-id="af575-124">Logging server events to the event log</span></span>](#server_eventlog)
- [<span data-ttu-id="af575-125">Activation du suivi dans le client .NET (applications de bureau Windows)</span><span class="sxs-lookup"><span data-stu-id="af575-125">Enabling tracing in the .NET client (Windows Desktop apps)</span></span>](#net_client)

    - [<span data-ttu-id="af575-126">Enregistrement des événements du client du Bureau à partir de la console</span><span class="sxs-lookup"><span data-stu-id="af575-126">Logging Desktop client events to the console</span></span>](#desktop_console)
    - [<span data-ttu-id="af575-127">Enregistrement des événements du client du Bureau à un fichier texte</span><span class="sxs-lookup"><span data-stu-id="af575-127">Logging Desktop client events to a text file</span></span>](#desktop_text)
- [<span data-ttu-id="af575-128">Activation du suivi dans Windows Phone 8 clients</span><span class="sxs-lookup"><span data-stu-id="af575-128">Enabling tracing in Windows Phone 8 clients</span></span>](#phone)

    - [<span data-ttu-id="af575-129">Journalisation des événements du client Windows Phone à l’interface utilisateur</span><span class="sxs-lookup"><span data-stu-id="af575-129">Logging Windows Phone client events to the UI</span></span>](#phone_ui)
    - [<span data-ttu-id="af575-130">Journalisation des événements du client Windows Phone sur la console de débogage</span><span class="sxs-lookup"><span data-stu-id="af575-130">Logging Windows Phone client events to the debug console</span></span>](#phone_debug)
- [<span data-ttu-id="af575-131">Activation du suivi dans le client JavaScript</span><span class="sxs-lookup"><span data-stu-id="af575-131">Enabling tracing in the JavaScript client</span></span>](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a><span data-ttu-id="af575-132">Activation du suivi sur le serveur</span><span class="sxs-lookup"><span data-stu-id="af575-132">Enabling tracing on the server</span></span>

<span data-ttu-id="af575-133">Vous activez le suivi sur le serveur dans le fichier de configuration de l’application (App. config ou Web. config, selon le type de projet). Vous spécifiez les catégories d’événements que vous souhaitez consigner.</span><span class="sxs-lookup"><span data-stu-id="af575-133">You enable tracing on the server within the application's configuration file (either App.config or Web.config depending on the type of project.) You specify which categories of events you want to log.</span></span> <span data-ttu-id="af575-134">Dans le fichier de configuration, vous spécifiez également si vous souhaitez enregistrer les événements dans un fichier texte, le journal des événements Windows ou un journal personnalisé à l’aide d’une implémentation de [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="af575-134">In the configuration file, you also specify whether to log the events to a text file, the Windows event log, or a custom log using an implementation of [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).</span></span>

<span data-ttu-id="af575-135">Les catégories d’événements de serveur incluent les types de messages suivants :</span><span class="sxs-lookup"><span data-stu-id="af575-135">The server event categories include the following sorts of messages:</span></span>

| <span data-ttu-id="af575-136">`Source`</span><span class="sxs-lookup"><span data-stu-id="af575-136">Source</span></span> | <span data-ttu-id="af575-137">Messages</span><span class="sxs-lookup"><span data-stu-id="af575-137">Messages</span></span> |
| --- | --- |
| <span data-ttu-id="af575-138">SignalR.SqlMessageBus</span><span class="sxs-lookup"><span data-stu-id="af575-138">SignalR.SqlMessageBus</span></span> | <span data-ttu-id="af575-139">Installation du fournisseur ScaleOut du bus des messages SQL, opération de base de données, erreur et événements de délai d’attente</span><span class="sxs-lookup"><span data-stu-id="af575-139">SQL Message Bus scaleout provider setup, database operation, error, and timeout events</span></span> |
| <span data-ttu-id="af575-140">SignalR.ServiceBusMessageBus</span><span class="sxs-lookup"><span data-stu-id="af575-140">SignalR.ServiceBusMessageBus</span></span> | <span data-ttu-id="af575-141">Événements de création et d’abonnement, d’erreur et de messagerie des rubriques du fournisseur ScaleOut de service bus</span><span class="sxs-lookup"><span data-stu-id="af575-141">Service bus scaleout provider topic creation and subscription, error, and messaging events</span></span> |
| <span data-ttu-id="af575-142">SignalR.RedisMessageBus</span><span class="sxs-lookup"><span data-stu-id="af575-142">SignalR.RedisMessageBus</span></span> | <span data-ttu-id="af575-143">Inverser les événements de connexion, de déconnexion et d’erreur du fournisseur ScaleOut</span><span class="sxs-lookup"><span data-stu-id="af575-143">Redis scaleout provider connection, disconnection, and error events</span></span> |
| <span data-ttu-id="af575-144">SignalR.ScaleoutMessageBus</span><span class="sxs-lookup"><span data-stu-id="af575-144">SignalR.ScaleoutMessageBus</span></span> | <span data-ttu-id="af575-145">Événements de messagerie ScaleOut</span><span class="sxs-lookup"><span data-stu-id="af575-145">Scaleout messaging events</span></span> |
| <span data-ttu-id="af575-146">SignalR.Transports.WebSocketTransport</span><span class="sxs-lookup"><span data-stu-id="af575-146">SignalR.Transports.WebSocketTransport</span></span> | <span data-ttu-id="af575-147">Événements liés à la connexion de transport WebSocket, à la déconnexion, à la messagerie et aux erreurs</span><span class="sxs-lookup"><span data-stu-id="af575-147">WebSocket transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="af575-148">SignalR.Transports.ServerSentEventsTransport</span><span class="sxs-lookup"><span data-stu-id="af575-148">SignalR.Transports.ServerSentEventsTransport</span></span> | <span data-ttu-id="af575-149">ServerSentEvents de la connexion de transport, de la déconnexion, de la messagerie et des événements d’erreur</span><span class="sxs-lookup"><span data-stu-id="af575-149">ServerSentEvents transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="af575-150">SignalR.Transports.ForeverFrameTransport</span><span class="sxs-lookup"><span data-stu-id="af575-150">SignalR.Transports.ForeverFrameTransport</span></span> | <span data-ttu-id="af575-151">ForeverFrame de la connexion de transport, de la déconnexion, de la messagerie et des événements d’erreur</span><span class="sxs-lookup"><span data-stu-id="af575-151">ForeverFrame transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="af575-152">SignalR.Transports.LongPollingTransport</span><span class="sxs-lookup"><span data-stu-id="af575-152">SignalR.Transports.LongPollingTransport</span></span> | <span data-ttu-id="af575-153">LongPolling de la connexion de transport, de la déconnexion, de la messagerie et des événements d’erreur</span><span class="sxs-lookup"><span data-stu-id="af575-153">LongPolling transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="af575-154">SignalR.Transports.TransportHeartBeat</span><span class="sxs-lookup"><span data-stu-id="af575-154">SignalR.Transports.TransportHeartBeat</span></span> | <span data-ttu-id="af575-155">Événements de connexion de transport, de déconnexion et KeepAlive</span><span class="sxs-lookup"><span data-stu-id="af575-155">Transport connection, disconnection, and keepalive events</span></span> |
| <span data-ttu-id="af575-156">SignalR.ReflectedHubDescriptorProvider</span><span class="sxs-lookup"><span data-stu-id="af575-156">SignalR.ReflectedHubDescriptorProvider</span></span> | <span data-ttu-id="af575-157">Événements de découverte du Hub</span><span class="sxs-lookup"><span data-stu-id="af575-157">Hub discovery events</span></span> |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a><span data-ttu-id="af575-158">Journalisation des événements du serveur dans des fichiers texte</span><span class="sxs-lookup"><span data-stu-id="af575-158">Logging server events to text files</span></span>

<span data-ttu-id="af575-159">Le code suivant montre comment activer le suivi pour chaque catégorie d’événement.</span><span class="sxs-lookup"><span data-stu-id="af575-159">The following code shows how to enable tracing for each category of event.</span></span> <span data-ttu-id="af575-160">Cet exemple configure l’application pour enregistrer des événements dans des fichiers texte.</span><span class="sxs-lookup"><span data-stu-id="af575-160">This sample configures the application to log events to text files.</span></span>

<span data-ttu-id="af575-161">**Code serveur XML pour l’activation du suivi**</span><span class="sxs-lookup"><span data-stu-id="af575-161">**XML server code for enabling tracing**</span></span>

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

<span data-ttu-id="af575-162">Dans le code ci-dessus, l’entrée `SignalRSwitch` spécifie le [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) utilisé pour les événements envoyés au journal spécifié.</span><span class="sxs-lookup"><span data-stu-id="af575-162">In the code above, the `SignalRSwitch` entry specifies the [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) used for events sent to the specified log.</span></span> <span data-ttu-id="af575-163">Dans ce cas, il est défini sur `Verbose`, ce qui signifie que tous les messages de débogage et de suivi sont journalisés.</span><span class="sxs-lookup"><span data-stu-id="af575-163">In this case, it is set to `Verbose` which means all debugging and tracing messages are logged.</span></span>

<span data-ttu-id="af575-164">La sortie suivante montre les entrées du fichier `transports.log.txt` pour une application utilisant le fichier de configuration ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="af575-164">The following output shows entries from the `transports.log.txt` file for an application using the above configuration file.</span></span> <span data-ttu-id="af575-165">Il montre une nouvelle connexion, une connexion supprimée et les événements de pulsation de transport.</span><span class="sxs-lookup"><span data-stu-id="af575-165">It shows a new connection, a removed connection, and transport heartbeat events.</span></span>

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a><span data-ttu-id="af575-166">Journalisation des événements du serveur dans le journal des événements</span><span class="sxs-lookup"><span data-stu-id="af575-166">Logging server events to the event log</span></span>

<span data-ttu-id="af575-167">Pour consigner les événements dans le journal des événements plutôt que dans un fichier texte, modifiez les valeurs des entrées dans le nœud `sharedListeners`.</span><span class="sxs-lookup"><span data-stu-id="af575-167">To log events to the event log rather than a text file, change the values for the entries in the `sharedListeners` node.</span></span> <span data-ttu-id="af575-168">Le code suivant montre comment enregistrer les événements du serveur dans le journal des événements :</span><span class="sxs-lookup"><span data-stu-id="af575-168">The following code shows how to log server events to the event log:</span></span>

<span data-ttu-id="af575-169">**Code du serveur XML pour l’enregistrement des événements dans le journal des événements**</span><span class="sxs-lookup"><span data-stu-id="af575-169">**XML server code for logging events to the event log**</span></span>

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

<span data-ttu-id="af575-170">Les événements sont consignés dans le journal des applications et sont disponibles via le observateur d’événements, comme indiqué ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="af575-170">The events are logged in the Application log, and are available through the Event Viewer, as shown below:</span></span>

![observateur d’événements l’indication des journaux Signalr](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="af575-172">Lorsque vous utilisez le journal des événements, définissez le **TraceLevel** sur **erreur** pour conserver le nombre de messages gérables.</span><span class="sxs-lookup"><span data-stu-id="af575-172">When using the event log, set the **TraceLevel** to **Error** to keep the number of messages manageable.</span></span>

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a><span data-ttu-id="af575-173">Activation du suivi dans le client .NET (applications de bureau Windows)</span><span class="sxs-lookup"><span data-stu-id="af575-173">Enabling tracing in the .NET client (Windows Desktop apps)</span></span>

<span data-ttu-id="af575-174">Le client .NET peut consigner des événements dans la console, un fichier texte ou un journal personnalisé à l’aide d’une implémentation de [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).</span><span class="sxs-lookup"><span data-stu-id="af575-174">The .NET client can log events to the console, a text file, or to a custom log using an implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).</span></span>

<span data-ttu-id="af575-175">Pour activer la journalisation dans le client .NET, définissez la propriété `TraceLevel` de la connexion sur une valeur [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) , et la propriété `TraceWriter` sur une instance [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) valide.</span><span class="sxs-lookup"><span data-stu-id="af575-175">To enable logging in the .NET client, set the connection's `TraceLevel` property to a [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) value, and the `TraceWriter` property to a valid [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) instance.</span></span>

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a><span data-ttu-id="af575-176">Enregistrement des événements du client du Bureau à partir de la console</span><span class="sxs-lookup"><span data-stu-id="af575-176">Logging Desktop client events to the console</span></span>

<span data-ttu-id="af575-177">Le code C# suivant montre comment enregistrer des événements dans le client .net sur la console :</span><span class="sxs-lookup"><span data-stu-id="af575-177">The following C# code shows how to log events in the .NET client to the console:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a><span data-ttu-id="af575-178">Enregistrement des événements du client du Bureau à un fichier texte</span><span class="sxs-lookup"><span data-stu-id="af575-178">Logging Desktop client events to a text file</span></span>

<span data-ttu-id="af575-179">Le code C# suivant montre comment enregistrer des événements dans le client .net dans un fichier texte :</span><span class="sxs-lookup"><span data-stu-id="af575-179">The following C# code shows how to log events in the .NET client to a text file:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

<span data-ttu-id="af575-180">La sortie suivante montre les entrées du fichier `ClientLog.txt` pour une application utilisant le fichier de configuration ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="af575-180">The following output shows entries from the `ClientLog.txt` file for an application using the above configuration file.</span></span> <span data-ttu-id="af575-181">Il montre le client qui se connecte au serveur et le Hub appelant une méthode cliente appelée `addMessage`:</span><span class="sxs-lookup"><span data-stu-id="af575-181">It shows the client connecting to the server, and the hub invoking a client method called `addMessage`:</span></span>

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a><span data-ttu-id="af575-182">Activation du suivi dans Windows Phone 8 clients</span><span class="sxs-lookup"><span data-stu-id="af575-182">Enabling tracing in Windows Phone 8 clients</span></span>

<span data-ttu-id="af575-183">Les applications signalr pour les applications Windows Phone utilisent le même client .NET que les applications de bureau, mais la [console. out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) et l’écriture dans un fichier avec [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) ne sont pas disponibles.</span><span class="sxs-lookup"><span data-stu-id="af575-183">SignalR applications for Windows Phone apps use the same .NET client as desktop apps, but [Console.Out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) and writing to a file with [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) are not available.</span></span> <span data-ttu-id="af575-184">Au lieu de cela, vous devez créer une implémentation personnalisée de [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) pour le traçage.</span><span class="sxs-lookup"><span data-stu-id="af575-184">Instead, you need to create a custom implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) for tracing.</span></span>

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a><span data-ttu-id="af575-185">Journalisation des événements du client Windows Phone à l’interface utilisateur</span><span class="sxs-lookup"><span data-stu-id="af575-185">Logging Windows Phone client events to the UI</span></span>

<span data-ttu-id="af575-186">Le [code base de signalr](https://github.com/SignalR/SignalR/archive/master.zip) comprend un exemple de Windows Phone qui écrit la sortie de trace dans un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) à l’aide d’une implémentation [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) personnalisée appelée `TextBlockWriter`.</span><span class="sxs-lookup"><span data-stu-id="af575-186">The [SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) includes a Windows Phone sample that writes trace output to a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) using a custom [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) implementation called `TextBlockWriter`.</span></span> <span data-ttu-id="af575-187">Cette classe se trouve dans le projet **Samples/Microsoft. Aspnet. signalr. client. wp8. Samples** .</span><span class="sxs-lookup"><span data-stu-id="af575-187">This class can be found in the **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** project.</span></span> <span data-ttu-id="af575-188">Lorsque vous créez une instance de `TextBlockWriter`, transmettez le [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx)actuel et un [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) où il créera un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) à utiliser pour la sortie de trace :</span><span class="sxs-lookup"><span data-stu-id="af575-188">When creating an instance of `TextBlockWriter`, pass in the current [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx), and a [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) where it will create a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) to use for trace output:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

<span data-ttu-id="af575-189">La sortie de trace est ensuite écrite dans un nouveau [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) créé dans le [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) que vous avez passé :</span><span class="sxs-lookup"><span data-stu-id="af575-189">The trace output will then be written to a new [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) created in the [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) you passed in:</span></span>

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a><span data-ttu-id="af575-190">Journalisation des événements du client Windows Phone sur la console de débogage</span><span class="sxs-lookup"><span data-stu-id="af575-190">Logging Windows Phone client events to the debug console</span></span>

<span data-ttu-id="af575-191">Pour envoyer la sortie vers la console de débogage plutôt que l’interface utilisateur, créez une implémentation de [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) qui écrit dans la fenêtre de débogage et assignez-la à la propriété [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) de votre connexion :</span><span class="sxs-lookup"><span data-stu-id="af575-191">To send output to the debug console rather than the UI, create an implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) that writes to the debug window, and assign it to your connection's [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) property:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

<span data-ttu-id="af575-192">Les informations de trace sont ensuite écrites dans la fenêtre de débogage de Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="af575-192">Trace information will then be written to the debug window in Visual Studio:</span></span>

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a><span data-ttu-id="af575-193">Activation du suivi dans le client JavaScript</span><span class="sxs-lookup"><span data-stu-id="af575-193">Enabling tracing in the JavaScript client</span></span>

<span data-ttu-id="af575-194">Pour activer la journalisation côté client sur une connexion, définissez la propriété `logging` sur l’objet de connexion avant d’appeler la méthode `start` pour établir la connexion.</span><span class="sxs-lookup"><span data-stu-id="af575-194">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="af575-195">**Code JavaScript du client pour activer le suivi sur la console du navigateur (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="af575-195">**Client JavaScript code for enabling tracing to the browser console (with the generated proxy)**</span></span>

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

<span data-ttu-id="af575-196">**Code JavaScript du client pour activer le suivi sur la console du navigateur (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="af575-196">**Client JavaScript code for enabling tracing to the browser console (without the generated proxy)**</span></span>

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

<span data-ttu-id="af575-197">Lorsque le suivi est activé, le client JavaScript enregistre les événements dans la console du navigateur.</span><span class="sxs-lookup"><span data-stu-id="af575-197">When tracing is enabled, the JavaScript client logs events to the browser console.</span></span> <span data-ttu-id="af575-198">Pour accéder à la console du navigateur, consultez [surveillance des transports](../getting-started/introduction-to-signalr.md#MonitoringTransports).</span><span class="sxs-lookup"><span data-stu-id="af575-198">To access the browser console, see [Monitoring Transports](../getting-started/introduction-to-signalr.md#MonitoringTransports).</span></span>

<span data-ttu-id="af575-199">La capture d’écran suivante montre un client JavaScript Signalr pour lequel le suivi est activé.</span><span class="sxs-lookup"><span data-stu-id="af575-199">The following screenshot shows a SignalR JavaScript client with tracing enabled.</span></span> <span data-ttu-id="af575-200">Il montre les événements de connexion et d’appel de concentrateur dans la console du navigateur :</span><span class="sxs-lookup"><span data-stu-id="af575-200">It shows connection and hub invocation events in the browser console:</span></span>

![Événements de suivi signalr dans la console du navigateur](enabling-signalr-tracing/_static/image4.png)
