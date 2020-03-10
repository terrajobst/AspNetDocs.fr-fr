---
uid: signalr/overview/performance/signalr-performance
title: Performances de signalr | Microsoft Docs
author: bradygaster
description: Performances de SignalR
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: b8a44f4c924c94cdfff1ce7630539b45fe269bbf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558323"
---
# <a name="signalr-performance"></a><span data-ttu-id="b2595-103">Performances de SignalR</span><span class="sxs-lookup"><span data-stu-id="b2595-103">SignalR Performance</span></span>

<span data-ttu-id="b2595-104">de [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="b2595-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="b2595-105">Cette rubrique explique comment concevoir, mesurer et améliorer les performances dans une application Signalr.</span><span class="sxs-lookup"><span data-stu-id="b2595-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="b2595-106">Versions logicielles utilisées dans cette rubrique</span><span class="sxs-lookup"><span data-stu-id="b2595-106">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="b2595-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="b2595-107">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="b2595-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="b2595-108">.NET 4.5</span></span>
> - <span data-ttu-id="b2595-109">Signalr version 2</span><span class="sxs-lookup"><span data-stu-id="b2595-109">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="b2595-110">Versions précédentes de cette rubrique</span><span class="sxs-lookup"><span data-stu-id="b2595-110">Previous versions of this topic</span></span>
>
> <span data-ttu-id="b2595-111">Pour plus d’informations sur les versions antérieures de Signalr, consultez [versions antérieures de signalr](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="b2595-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="b2595-112">Questions et commentaires</span><span class="sxs-lookup"><span data-stu-id="b2595-112">Questions and comments</span></span>
>
> <span data-ttu-id="b2595-113">N’hésitez pas à nous faire part de vos commentaires sur la façon dont vous aimez ce didacticiel et sur ce que nous pourrions améliorer dans les commentaires en bas de la page.</span><span class="sxs-lookup"><span data-stu-id="b2595-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="b2595-114">Si vous avez des questions qui ne sont pas directement liées au didacticiel, vous pouvez les poster sur le [forum ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="b2595-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<span data-ttu-id="b2595-115">Pour obtenir une présentation récente des performances et de la mise à l’échelle de Signalr, consultez [mise à l’échelle du Web en temps réel avec ASP.net signalr](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="b2595-115">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="b2595-116">Cette rubrique contient les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="b2595-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="b2595-117">Remarques relatives à la conception</span><span class="sxs-lookup"><span data-stu-id="b2595-117">Design considerations</span></span>](#design)
- [<span data-ttu-id="b2595-118">Réglage des performances de votre serveur Signalr</span><span class="sxs-lookup"><span data-stu-id="b2595-118">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="b2595-119">Résolution des problèmes de performances</span><span class="sxs-lookup"><span data-stu-id="b2595-119">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="b2595-120">Utilisation des compteurs de performance Signalr</span><span class="sxs-lookup"><span data-stu-id="b2595-120">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="b2595-121">Utilisation d’autres compteurs de performances</span><span class="sxs-lookup"><span data-stu-id="b2595-121">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="b2595-122">Autres ressources</span><span class="sxs-lookup"><span data-stu-id="b2595-122">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="b2595-123">Remarques relatives à la conception</span><span class="sxs-lookup"><span data-stu-id="b2595-123">Design considerations</span></span>

<span data-ttu-id="b2595-124">Cette section décrit les modèles qui peuvent être implémentés lors de la conception d’une application Signalr, afin de garantir que les performances ne sont pas gênées par la génération d’un trafic réseau inutile.</span><span class="sxs-lookup"><span data-stu-id="b2595-124">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="b2595-125">Limitation de la fréquence des messages</span><span class="sxs-lookup"><span data-stu-id="b2595-125">Throttling message frequency</span></span>

<span data-ttu-id="b2595-126">Même dans une application qui envoie des messages à une fréquence élevée (par exemple, une application de jeu en temps réel), la plupart des applications n’ont pas besoin d’envoyer plus de quelques messages par seconde.</span><span class="sxs-lookup"><span data-stu-id="b2595-126">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="b2595-127">Pour réduire le volume de trafic généré par chaque client, il est possible d’implémenter une boucle de messages qui met en file d’attente et envoie des messages plus fréquemment qu’un taux fixe (autrement dit, jusqu’à un certain nombre de messages sont envoyés chaque seconde, s’il y a des messages dans ce délai en onstruction à envoyer).</span><span class="sxs-lookup"><span data-stu-id="b2595-127">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="b2595-128">Pour obtenir un exemple d’application qui limite les messages à un certain taux (à partir du client et du serveur), consultez [temps réel haute fréquence avec signalr](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="b2595-128">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="b2595-129">Réduction de la taille des messages</span><span class="sxs-lookup"><span data-stu-id="b2595-129">Reducing message size</span></span>

<span data-ttu-id="b2595-130">Vous pouvez réduire la taille d’un message Signalr en réduisant la taille de vos objets sérialisés.</span><span class="sxs-lookup"><span data-stu-id="b2595-130">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="b2595-131">Dans le code serveur, si vous envoyez un objet qui contient des propriétés qui n’ont pas besoin d’être transmises, empêchez la sérialisation de ces propriétés à l’aide de l’attribut `JsonIgnore`.</span><span class="sxs-lookup"><span data-stu-id="b2595-131">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="b2595-132">Les noms des propriétés sont également stockés dans le message. vous pouvez raccourcir les noms des propriétés à l’aide de l’attribut `JsonProperty`.</span><span class="sxs-lookup"><span data-stu-id="b2595-132">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="b2595-133">L’exemple de code suivant montre comment exclure une propriété de l’envoi au client et comment raccourcir les noms de propriété :</span><span class="sxs-lookup"><span data-stu-id="b2595-133">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="b2595-134">**Code du serveur .NET illustrant l’attribut JsonIgnore pour empêcher l’envoi de données au client et l’attribut JsonProperty pour réduire la taille des messages**</span><span class="sxs-lookup"><span data-stu-id="b2595-134">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="b2595-135">Afin de conserver la lisibilité et la maintenabilité dans le code client, les noms de propriété abrégés peuvent être remappés à des noms conviviaux une fois le message reçu.</span><span class="sxs-lookup"><span data-stu-id="b2595-135">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="b2595-136">L’exemple de code suivant montre une manière possible de remapper les noms abrégés à des noms plus longs, en définissant un contrat de message (mappage) et en utilisant la fonction `reMap` pour appliquer le contrat à la classe de message optimisée :</span><span class="sxs-lookup"><span data-stu-id="b2595-136">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="b2595-137">**Code JavaScript côté client qui remappe les noms de propriété abrégés à des noms explicites**</span><span class="sxs-lookup"><span data-stu-id="b2595-137">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="b2595-138">Il est également possible de raccourcir les noms des messages du client au serveur, en utilisant la même méthode.</span><span class="sxs-lookup"><span data-stu-id="b2595-138">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="b2595-139">La réduction de l’encombrement mémoire (c’est-à-dire la quantité de mémoire utilisée pour le message) de l’objet message peut également améliorer les performances.</span><span class="sxs-lookup"><span data-stu-id="b2595-139">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="b2595-140">Par exemple, si la plage complète d’un `int` n’est pas nécessaire, un `short` ou `byte` peut être utilisé à la place.</span><span class="sxs-lookup"><span data-stu-id="b2595-140">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="b2595-141">Étant donné que les messages sont stockés dans le bus de messages dans la mémoire du serveur, la réduction de la taille des messages peut également résoudre les problèmes de mémoire du serveur.</span><span class="sxs-lookup"><span data-stu-id="b2595-141">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="b2595-142">Réglage des performances de votre serveur Signalr</span><span class="sxs-lookup"><span data-stu-id="b2595-142">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="b2595-143">Les paramètres de configuration suivants peuvent être utilisés pour adapter votre serveur afin d’améliorer les performances dans une application Signalr.</span><span class="sxs-lookup"><span data-stu-id="b2595-143">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="b2595-144">Pour obtenir des informations générales sur l’amélioration des performances dans une application ASP.NET, consultez [amélioration des performances des ASP.net](https://msdn.microsoft.com/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="b2595-144">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="b2595-145">**Paramètres de configuration de signalr**</span><span class="sxs-lookup"><span data-stu-id="b2595-145">**SignalR configuration settings**</span></span>

- <span data-ttu-id="b2595-146">**DefaultMessageBufferSize**: par défaut, signalr conserve 1000 messages en mémoire par concentrateur par connexion.</span><span class="sxs-lookup"><span data-stu-id="b2595-146">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="b2595-147">Si des messages volumineux sont utilisés, cela peut créer des problèmes de mémoire qui peuvent être atténués en réduisant cette valeur.</span><span class="sxs-lookup"><span data-stu-id="b2595-147">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="b2595-148">Ce paramètre peut être défini dans le gestionnaire d’événements `Application_Start` dans une application ASP.NET ou dans la méthode `Configuration` d’une classe de démarrage OWIN dans une application auto-hébergée.</span><span class="sxs-lookup"><span data-stu-id="b2595-148">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="b2595-149">L’exemple suivant montre comment réduire cette valeur afin de réduire l’encombrement mémoire de votre application afin de réduire la quantité de mémoire du serveur utilisée :</span><span class="sxs-lookup"><span data-stu-id="b2595-149">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="b2595-150">**Code du serveur .NET dans Startup.cs pour la réduction de la taille de la mémoire tampon des messages par défaut**</span><span class="sxs-lookup"><span data-stu-id="b2595-150">**.NET server code in Startup.cs for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="b2595-151">**Paramètres de configuration IIS**</span><span class="sxs-lookup"><span data-stu-id="b2595-151">**IIS configuration settings**</span></span>

- <span data-ttu-id="b2595-152">Nombre **maximal de demandes simultanées par application**: l’augmentation du nombre de demandes IIS simultanées augmente les ressources serveur disponibles pour traiter les demandes.</span><span class="sxs-lookup"><span data-stu-id="b2595-152">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="b2595-153">La valeur par défaut est 5000 ; pour augmenter ce paramètre, exécutez les commandes suivantes dans une invite de commandes avec élévation de privilèges :</span><span class="sxs-lookup"><span data-stu-id="b2595-153">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- <span data-ttu-id="b2595-154">**ApplicationPool QueueLength**: il s’agit du nombre maximal de demandes que les files d’attente http. sys pour le pool d’applications.</span><span class="sxs-lookup"><span data-stu-id="b2595-154">**ApplicationPool QueueLength**: This is the maximum number of requests that Http.sys queues for the application pool.</span></span> <span data-ttu-id="b2595-155">Lorsque la file d’attente est pleine, les nouvelles demandes reçoivent une réponse 503 « Service indisponible ».</span><span class="sxs-lookup"><span data-stu-id="b2595-155">When the queue is full, new requests receive a 503 "Service Unavailable" response.</span></span> <span data-ttu-id="b2595-156">La valeur par défaut est 1000.</span><span class="sxs-lookup"><span data-stu-id="b2595-156">The default value is 1000.</span></span>

    <span data-ttu-id="b2595-157">La réduction de la longueur de la file d’attente pour le processus de travail dans le pool d’applications qui héberge votre application permet d’économiser les ressources mémoire.</span><span class="sxs-lookup"><span data-stu-id="b2595-157">Shortening the queue length for the worker process in the application pool hosting your application will conserve memory resources.</span></span> <span data-ttu-id="b2595-158">Pour plus d’informations, consultez [gestion, paramétrage et configuration des pools d’applications](https://technet.microsoft.com/library/cc745955.aspx).</span><span class="sxs-lookup"><span data-stu-id="b2595-158">For more information, see [Managing, Tuning, and Configuring Application Pools](https://technet.microsoft.com/library/cc745955.aspx).</span></span>

<span data-ttu-id="b2595-159">**Paramètres de configuration de ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="b2595-159">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="b2595-160">Cette section comprend des paramètres de configuration qui peuvent être définis dans le fichier `aspnet.config`.</span><span class="sxs-lookup"><span data-stu-id="b2595-160">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="b2595-161">Ce fichier se trouve dans l’un des deux emplacements, en fonction de la plateforme :</span><span class="sxs-lookup"><span data-stu-id="b2595-161">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="b2595-162">Les paramètres ASP.NET qui peuvent améliorer les performances de signaler sont les suivants :</span><span class="sxs-lookup"><span data-stu-id="b2595-162">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="b2595-163">**Nombre maximal de demandes simultanées par UC**: l’amélioration de ce paramètre peut réduire les goulots d’étranglement de performances.</span><span class="sxs-lookup"><span data-stu-id="b2595-163">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="b2595-164">Pour augmenter ce paramètre, ajoutez le paramètre de configuration suivant au fichier `aspnet.config` :</span><span class="sxs-lookup"><span data-stu-id="b2595-164">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="b2595-165">**Limite de la file d’attente des demandes**: lorsque le nombre total de connexions dépasse le paramètre `maxConcurrentRequestsPerCPU`, ASP.net commence à limiter les demandes à l’aide d’une file d’attente.</span><span class="sxs-lookup"><span data-stu-id="b2595-165">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="b2595-166">Pour augmenter la taille de la file d’attente, vous pouvez augmenter le paramètre `requestQueueLimit`.</span><span class="sxs-lookup"><span data-stu-id="b2595-166">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="b2595-167">Pour ce faire, ajoutez le paramètre de configuration suivant au nœud `processModel` dans `config/machine.config` (plutôt que `aspnet.config`) :</span><span class="sxs-lookup"><span data-stu-id="b2595-167">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="b2595-168">Résolution des problèmes de performances</span><span class="sxs-lookup"><span data-stu-id="b2595-168">Troubleshooting performance issues</span></span>

<span data-ttu-id="b2595-169">Cette section décrit les méthodes permettant de trouver des goulots d’étranglement au niveau des performances dans votre application.</span><span class="sxs-lookup"><span data-stu-id="b2595-169">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="b2595-170">Vérification de l’utilisation de WebSocket</span><span class="sxs-lookup"><span data-stu-id="b2595-170">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="b2595-171">Alors que Signalr peut utiliser un grand nombre de transports pour la communication entre le client et le serveur, WebSocket présente un avantage significatif en termes de performances et doit être utilisé si le client et le serveur le prennent en charge.</span><span class="sxs-lookup"><span data-stu-id="b2595-171">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="b2595-172">Pour déterminer si votre client et votre serveur satisfont à la configuration requise pour WebSocket, consultez [transports et secours](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="b2595-172">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="b2595-173">Pour déterminer le transport utilisé dans votre application, vous pouvez utiliser les outils de développement du navigateur et examiner les journaux pour voir quel transport est utilisé pour la connexion.</span><span class="sxs-lookup"><span data-stu-id="b2595-173">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="b2595-174">Pour plus d’informations sur l’utilisation des outils de développement de navigateur dans Internet Explorer et chrome, consultez [transports et secours](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="b2595-174">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="b2595-175">Utilisation des compteurs de performance Signalr</span><span class="sxs-lookup"><span data-stu-id="b2595-175">Using SignalR performance counters</span></span>

<span data-ttu-id="b2595-176">Cette section décrit comment activer et utiliser les compteurs de performance Signalr, qui se trouvent dans le package `Microsoft.AspNet.SignalR.Utils`.</span><span class="sxs-lookup"><span data-stu-id="b2595-176">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="b2595-177">Installation de signalr. exe</span><span class="sxs-lookup"><span data-stu-id="b2595-177">Installing signalr.exe</span></span>

<span data-ttu-id="b2595-178">Les compteurs de performances peuvent être ajoutés au serveur à l’aide d’un utilitaire appelé Signalr. exe.</span><span class="sxs-lookup"><span data-stu-id="b2595-178">Performance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="b2595-179">Pour installer cet utilitaire, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="b2595-179">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="b2595-180">Dans Visual Studio, sélectionnez **outils** > **Gestionnaire de package NuGet** > **gérer les packages NuGet pour la solution**</span><span class="sxs-lookup"><span data-stu-id="b2595-180">In Visual Studio, select **Tools** > **NuGet Package Manager** > **Manage NuGet Packages for Solution**</span></span>
2. <span data-ttu-id="b2595-181">Recherchez **signalr. utils**, puis sélectionnez Installer.</span><span class="sxs-lookup"><span data-stu-id="b2595-181">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="b2595-182">Acceptez le contrat de licence pour installer le package.</span><span class="sxs-lookup"><span data-stu-id="b2595-182">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="b2595-183">Signalr. exe sera installé dans `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span><span class="sxs-lookup"><span data-stu-id="b2595-183">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="b2595-184">Installation des compteurs de performances avec Signalr. exe</span><span class="sxs-lookup"><span data-stu-id="b2595-184">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="b2595-185">Pour installer les compteurs de performance Signalr, exécutez Signalr. exe dans une invite de commandes avec élévation de privilèges avec le paramètre suivant :</span><span class="sxs-lookup"><span data-stu-id="b2595-185">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="b2595-186">Pour supprimer les compteurs de performance Signalr, exécutez Signalr. exe dans une invite de commandes avec élévation de privilèges avec le paramètre suivant :</span><span class="sxs-lookup"><span data-stu-id="b2595-186">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="b2595-187">Compteurs de performance signalr</span><span class="sxs-lookup"><span data-stu-id="b2595-187">SignalR Performance counters</span></span>

<span data-ttu-id="b2595-188">Le package utilitaires installe les compteurs de performances suivants.</span><span class="sxs-lookup"><span data-stu-id="b2595-188">The utilities package installs the following performance counters.</span></span> <span data-ttu-id="b2595-189">Les compteurs « total » mesurent le nombre d’événements depuis le dernier redémarrage du pool d’applications ou du serveur.</span><span class="sxs-lookup"><span data-stu-id="b2595-189">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="b2595-190">**Métriques de connexion**</span><span class="sxs-lookup"><span data-stu-id="b2595-190">**Connection metrics**</span></span>

<span data-ttu-id="b2595-191">Les métriques suivantes mesurent les événements de durée de vie de la connexion qui se produisent.</span><span class="sxs-lookup"><span data-stu-id="b2595-191">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="b2595-192">Pour plus d’informations, consultez [comprendre et gérer les événements de durée de vie des connexions](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="b2595-192">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="b2595-193">**Connexions connectées**</span><span class="sxs-lookup"><span data-stu-id="b2595-193">**Connections Connected**</span></span>
- <span data-ttu-id="b2595-194">**Connexions reconnectées**</span><span class="sxs-lookup"><span data-stu-id="b2595-194">**Connections Reconnected**</span></span>
- <span data-ttu-id="b2595-195">**Connexions déconnectées**</span><span class="sxs-lookup"><span data-stu-id="b2595-195">**Connections Disconnected**</span></span>
- <span data-ttu-id="b2595-196">**Connexions en cours**</span><span class="sxs-lookup"><span data-stu-id="b2595-196">**Connections Current**</span></span>

<span data-ttu-id="b2595-197">**Métriques du message**</span><span class="sxs-lookup"><span data-stu-id="b2595-197">**Message metrics**</span></span>

<span data-ttu-id="b2595-198">Les métriques suivantes mesurent le trafic de messages généré par Signalr.</span><span class="sxs-lookup"><span data-stu-id="b2595-198">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="b2595-199">**Nombre total de messages de connexion reçus**</span><span class="sxs-lookup"><span data-stu-id="b2595-199">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="b2595-200">**Nombre total de messages de connexion envoyés**</span><span class="sxs-lookup"><span data-stu-id="b2595-200">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="b2595-201">**Messages de connexion reçus/s**</span><span class="sxs-lookup"><span data-stu-id="b2595-201">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="b2595-202">**Messages de connexion envoyés/s**</span><span class="sxs-lookup"><span data-stu-id="b2595-202">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="b2595-203">**Métriques du bus de messages**</span><span class="sxs-lookup"><span data-stu-id="b2595-203">**Message bus metrics**</span></span>

<span data-ttu-id="b2595-204">Les métriques suivantes mesurent le trafic via le bus de messages Signalr interne, la file d’attente dans laquelle tous les messages entrants et sortants Signalr sont placés.</span><span class="sxs-lookup"><span data-stu-id="b2595-204">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="b2595-205">Un message est **publié** lors de son envoi ou de sa diffusion.</span><span class="sxs-lookup"><span data-stu-id="b2595-205">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="b2595-206">Un **abonné** dans ce contexte est un abonnement sur le bus de messages ; Cela doit être égal au nombre de clients plus le serveur lui-même.</span><span class="sxs-lookup"><span data-stu-id="b2595-206">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="b2595-207">Un **Worker alloué** est un composant qui envoie des données à des connexions actives. un **Worker occupé est un Worker** qui envoie activement un message.</span><span class="sxs-lookup"><span data-stu-id="b2595-207">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="b2595-208">**Nombre total de messages reçus par le bus de messages**</span><span class="sxs-lookup"><span data-stu-id="b2595-208">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="b2595-209">**Messages du bus de messages reçus/s**</span><span class="sxs-lookup"><span data-stu-id="b2595-209">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="b2595-210">**Nombre total de messages publiés par le bus de messages**</span><span class="sxs-lookup"><span data-stu-id="b2595-210">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="b2595-211">**Messages du bus de messages publiés/s**</span><span class="sxs-lookup"><span data-stu-id="b2595-211">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="b2595-212">**Abonnés au bus de messages actuel**</span><span class="sxs-lookup"><span data-stu-id="b2595-212">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="b2595-213">**Nombre total d’abonnés au bus de messages**</span><span class="sxs-lookup"><span data-stu-id="b2595-213">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="b2595-214">**Abonnés au bus de messages/s**</span><span class="sxs-lookup"><span data-stu-id="b2595-214">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="b2595-215">**Threads de messagerie alloués**</span><span class="sxs-lookup"><span data-stu-id="b2595-215">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="b2595-216">**Threads occupés par le bus de messages**</span><span class="sxs-lookup"><span data-stu-id="b2595-216">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="b2595-217">**Rubriques du bus de messages en cours**</span><span class="sxs-lookup"><span data-stu-id="b2595-217">**Message Bus Topics Current**</span></span>

<span data-ttu-id="b2595-218">**Métriques d’erreur**</span><span class="sxs-lookup"><span data-stu-id="b2595-218">**Error metrics**</span></span>

<span data-ttu-id="b2595-219">Les métriques suivantes mesurent les erreurs générées par le trafic des messages Signalr.</span><span class="sxs-lookup"><span data-stu-id="b2595-219">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="b2595-220">Des erreurs de **résolution de concentrateur** se produisent lorsqu’une méthode de concentrateur ou de concentrateur ne peut pas être résolue.</span><span class="sxs-lookup"><span data-stu-id="b2595-220">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="b2595-221">Les erreurs d' **appel de concentrateur** sont des exceptions levées lors de l’appel d’une méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="b2595-221">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="b2595-222">Les erreurs de **transport** sont des erreurs de connexion levées au cours d’une requête ou d’une réponse http.</span><span class="sxs-lookup"><span data-stu-id="b2595-222">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="b2595-223">**Erreurs : total**</span><span class="sxs-lookup"><span data-stu-id="b2595-223">**Errors: All Total**</span></span>
- <span data-ttu-id="b2595-224">**Erreurs : toutes les/s**</span><span class="sxs-lookup"><span data-stu-id="b2595-224">**Errors: All/Sec**</span></span>
- <span data-ttu-id="b2595-225">**Erreurs : total de résolution de concentrateur**</span><span class="sxs-lookup"><span data-stu-id="b2595-225">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="b2595-226">**Erreurs : résolution de concentrateur/s**</span><span class="sxs-lookup"><span data-stu-id="b2595-226">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="b2595-227">**Erreurs : total d’invocation de concentrateur**</span><span class="sxs-lookup"><span data-stu-id="b2595-227">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="b2595-228">**Erreurs : appel de Hub/s**</span><span class="sxs-lookup"><span data-stu-id="b2595-228">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="b2595-229">**Erreurs : transport total**</span><span class="sxs-lookup"><span data-stu-id="b2595-229">**Errors: Transport Total**</span></span>
- <span data-ttu-id="b2595-230">**Erreurs : transport/s**</span><span class="sxs-lookup"><span data-stu-id="b2595-230">**Errors: Transport/Sec**</span></span>

<a id="scaleout_metrics"></a>

<span data-ttu-id="b2595-231">**Mesures ScaleOut**</span><span class="sxs-lookup"><span data-stu-id="b2595-231">**Scaleout metrics**</span></span>

<span data-ttu-id="b2595-232">Les métriques suivantes mesurent le trafic et les erreurs générées par le fournisseur ScaleOut.</span><span class="sxs-lookup"><span data-stu-id="b2595-232">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="b2595-233">Un **flux** dans ce contexte est une unité d’échelle utilisée par le fournisseur ScaleOut. Il s’agit d’une table si SQL Server est utilisé, une rubrique si Service Bus est utilisé et un abonnement si Redims est utilisé.</span><span class="sxs-lookup"><span data-stu-id="b2595-233">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="b2595-234">Chaque flux garantit des opérations de lecture et d’écriture ordonnées ; un flux unique est un goulot d’étranglement de mise à l’échelle potentiel. le nombre de flux peut donc être augmenté pour réduire ce goulot d’étranglement.</span><span class="sxs-lookup"><span data-stu-id="b2595-234">Each stream ensures ordered read and write operations; a single stream is a potential scale bottleneck, so the number of streams can be increased to help reduce that bottleneck.</span></span> <span data-ttu-id="b2595-235">Si plusieurs flux sont utilisés, Signalr répartit automatiquement les messages (partition) sur ces flux de manière à garantir que les messages envoyés à partir d’une connexion donnée sont dans l’ordre.</span><span class="sxs-lookup"><span data-stu-id="b2595-235">If multiple streams are used, SignalR will automatically distribute (shard) messages across these streams in a way that ensures messages sent from any given connection are in order.</span></span>

<span data-ttu-id="b2595-236">Le paramètre [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) contrôle la longueur de la file d’attente d’envoi ScaleOut conservée par signalr.</span><span class="sxs-lookup"><span data-stu-id="b2595-236">The [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) setting controls the length of the scaleout send queue maintained by SignalR.</span></span> <span data-ttu-id="b2595-237">Si vous définissez cette valeur sur une valeur supérieure à 0, tous les messages d’une file d’attente d’envoi sont envoyés un à la fois au fond de panier de la messagerie configurée.</span><span class="sxs-lookup"><span data-stu-id="b2595-237">Setting it to a value greater than 0 will place all messages in a send queue to be sent one at a time to the configured messaging backplane.</span></span> <span data-ttu-id="b2595-238">Si la taille de la file d’attente dépasse la longueur configurée, les appels suivants à Send échouent immédiatement avec une [exception InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) jusqu’à ce que le nombre de messages dans la file d’attente soit inférieur au paramètre.</span><span class="sxs-lookup"><span data-stu-id="b2595-238">If the size of the queue goes above the configured length, subsequent calls to send will fail immediately with an [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) until the number of messages in the queue is less than the setting again.</span></span> <span data-ttu-id="b2595-239">La mise en file d’attente est désactivée par défaut, car les plans de mise en file d’attente implémentés ont généralement leur propre mise en file d’attente ou contrôle de Flow en place.</span><span class="sxs-lookup"><span data-stu-id="b2595-239">Queueing is disabled by default because the implemented backplanes generally have their own queuing or flow-control in place.</span></span> <span data-ttu-id="b2595-240">Dans le cas de SQL Server, le regroupement de connexions limite efficacement le nombre d’envois en cours d’exécution à un moment donné.</span><span class="sxs-lookup"><span data-stu-id="b2595-240">In the case of SQL Server, connection pooling effectively limits the number of sends going on at any one time.</span></span>

<span data-ttu-id="b2595-241">Par défaut, un seul flux est utilisé pour les SQL Server et les ReDim, cinq flux sont utilisés pour Service Bus et la mise en file d’attente est désactivée, mais ces paramètres peuvent être modifiés via la configuration sur SQL Server et Service Bus :</span><span class="sxs-lookup"><span data-stu-id="b2595-241">By default, only one stream is used for SQL Server and Redis, five streams are used for Service Bus, and queueing is disabled, but these settings can be changed through configuration on SQL Server and Service Bus:</span></span>

<span data-ttu-id="b2595-242">**Code du serveur .NET pour la configuration du nombre de tables et de la longueur de la file d’attente pour SQL Server fond de panier**</span><span class="sxs-lookup"><span data-stu-id="b2595-242">**.NET Server Code for configuring table count and queue length for SQL Server backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

<span data-ttu-id="b2595-243">**Code du serveur .NET pour la configuration du nombre de rubriques et de la longueur de file d’attente pour Service Bus fond de panier**</span><span class="sxs-lookup"><span data-stu-id="b2595-243">**.NET Server Code for configuring topic count and queue length for Service Bus backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

<span data-ttu-id="b2595-244">Un flux de **mise en mémoire tampon** est un flux qui est entré dans un état d’erreur ; Lorsque le flux est dans l’état Faulted, tous les messages envoyés au backplane échouent immédiatement jusqu’à ce que le flux ne génère plus de défaillance.</span><span class="sxs-lookup"><span data-stu-id="b2595-244">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="b2595-245">La **longueur de la file d’attente d’envoi** est le nombre de messages qui ont été publiés mais pas encore envoyés.</span><span class="sxs-lookup"><span data-stu-id="b2595-245">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="b2595-246">**Messages du bus des messages ScaleOut reçus/s**</span><span class="sxs-lookup"><span data-stu-id="b2595-246">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="b2595-247">**Total des flux ScaleOut**</span><span class="sxs-lookup"><span data-stu-id="b2595-247">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="b2595-248">**Flux ScaleOut ouverts**</span><span class="sxs-lookup"><span data-stu-id="b2595-248">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="b2595-249">**Mise en mémoire tampon des flux ScaleOut**</span><span class="sxs-lookup"><span data-stu-id="b2595-249">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="b2595-250">**Nombre total d’erreurs ScaleOut**</span><span class="sxs-lookup"><span data-stu-id="b2595-250">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="b2595-251">**Erreurs de ScaleOut/s**</span><span class="sxs-lookup"><span data-stu-id="b2595-251">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="b2595-252">**Longueur de la file d’attente d’envoi ScaleOut**</span><span class="sxs-lookup"><span data-stu-id="b2595-252">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="b2595-253">Pour plus d’informations sur la mesure de ces compteurs, consultez [signalr ScaleOut avec Azure Service bus](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="b2595-253">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="b2595-254">Utilisation d’autres compteurs de performances</span><span class="sxs-lookup"><span data-stu-id="b2595-254">Using other performance counters</span></span>

<span data-ttu-id="b2595-255">Les compteurs de performances suivants peuvent également être utiles pour analyser les performances de votre application.</span><span class="sxs-lookup"><span data-stu-id="b2595-255">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="b2595-256">**Mémoire**</span><span class="sxs-lookup"><span data-stu-id="b2595-256">**Memory**</span></span>

- <span data-ttu-id="b2595-257">Mémoire CLR .NET\\nombre d’octets dans tous les tas (pour w3wp)</span><span class="sxs-lookup"><span data-stu-id="b2595-257">.NET CLR Memory\\# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="b2595-258">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="b2595-258">**ASP.NET**</span></span>

- <span data-ttu-id="b2595-259">ASP. asp.net \ requêtes actuel</span><span class="sxs-lookup"><span data-stu-id="b2595-259">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="b2595-260">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="b2595-260">ASP.NET\Queued</span></span>
- <span data-ttu-id="b2595-261">ASP. NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="b2595-261">ASP.NET\Rejected</span></span>

<span data-ttu-id="b2595-262">**Processeur**</span><span class="sxs-lookup"><span data-stu-id="b2595-262">**CPU**</span></span>

- <span data-ttu-id="b2595-263">Temps de Information\Processor du processeur</span><span class="sxs-lookup"><span data-stu-id="b2595-263">Processor Information\Processor Time</span></span>

<span data-ttu-id="b2595-264">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="b2595-264">**TCP/IP**</span></span>

- <span data-ttu-id="b2595-265">TCPv6/connexions établies</span><span class="sxs-lookup"><span data-stu-id="b2595-265">TCPv6/Connections Established</span></span>
- <span data-ttu-id="b2595-266">TCPv4/connexions établies</span><span class="sxs-lookup"><span data-stu-id="b2595-266">TCPv4/Connections Established</span></span>

<span data-ttu-id="b2595-267">**Service Web**</span><span class="sxs-lookup"><span data-stu-id="b2595-267">**Web Service**</span></span>

- <span data-ttu-id="b2595-268">Connexions Web Web\connexions</span><span class="sxs-lookup"><span data-stu-id="b2595-268">Web Service\Current Connections</span></span>
- <span data-ttu-id="b2595-269">Connexions Web Service\Maximum</span><span class="sxs-lookup"><span data-stu-id="b2595-269">Web Service\Maximum Connections</span></span>

<span data-ttu-id="b2595-270">**Thread**</span><span class="sxs-lookup"><span data-stu-id="b2595-270">**Threading**</span></span>

- <span data-ttu-id="b2595-271">Threads et verrous CLR .NET\\nombre de threads logiques actuels</span><span class="sxs-lookup"><span data-stu-id="b2595-271">.NET CLR Locks And Threads\\# of current logical Threads</span></span>
- <span data-ttu-id="b2595-272">Threads et verrous CLR .NET\\nombre de threads physiques actuels</span><span class="sxs-lookup"><span data-stu-id="b2595-272">.NET CLR Locks And Threads\\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="b2595-273">Autres ressources</span><span class="sxs-lookup"><span data-stu-id="b2595-273">Other Resources</span></span>

<span data-ttu-id="b2595-274">Pour plus d’informations sur l’analyse et le réglage des performances ASP.NET, consultez les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="b2595-274">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="b2595-275">[Vue d’ensemble des performances ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="b2595-275">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="b2595-276">Utilisation des threads ASP.NET sur IIS 7,5, IIS 7,0 et IIS 6,0</span><span class="sxs-lookup"><span data-stu-id="b2595-276">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="b2595-277">&lt;applicationPool&gt;, élément (paramètres Web)</span><span class="sxs-lookup"><span data-stu-id="b2595-277">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
