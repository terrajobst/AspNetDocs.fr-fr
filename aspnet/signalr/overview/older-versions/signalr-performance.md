---
uid: signalr/overview/older-versions/signalr-performance
title: Performances de signalr (Signalr 1. x) | Microsoft Docs
author: bradygaster
description: Performances de SignalR
ms.author: bradyg
ms.date: 07/03/2013
ms.assetid: 9594d644-66b6-4223-acdd-23e29a6e4c46
msc.legacyurl: /signalr/overview/older-versions/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: 915fd822caae9bbcb0a688c6dd7a5b2bda12c9df
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579610"
---
# <a name="signalr-performance-signalr-1x"></a><span data-ttu-id="5a098-103">Performances de SignalR (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="5a098-103">SignalR Performance (SignalR 1.x)</span></span>

<span data-ttu-id="5a098-104">de [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="5a098-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="5a098-105">Cette rubrique explique comment concevoir, mesurer et améliorer les performances dans une application Signalr.</span><span class="sxs-lookup"><span data-stu-id="5a098-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>

<span data-ttu-id="5a098-106">Pour obtenir une présentation récente des performances et de la mise à l’échelle de Signalr, consultez [mise à l’échelle du Web en temps réel avec ASP.net signalr](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="5a098-106">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="5a098-107">Cette rubrique contient les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="5a098-107">This topic contains the following sections:</span></span>

- [<span data-ttu-id="5a098-108">Remarques relatives à la conception</span><span class="sxs-lookup"><span data-stu-id="5a098-108">Design considerations</span></span>](#design)
- [<span data-ttu-id="5a098-109">Réglage des performances de votre serveur Signalr</span><span class="sxs-lookup"><span data-stu-id="5a098-109">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="5a098-110">Résolution des problèmes de performances</span><span class="sxs-lookup"><span data-stu-id="5a098-110">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="5a098-111">Utilisation des compteurs de performance Signalr</span><span class="sxs-lookup"><span data-stu-id="5a098-111">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="5a098-112">Utilisation d’autres compteurs de performances</span><span class="sxs-lookup"><span data-stu-id="5a098-112">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="5a098-113">Autres ressources</span><span class="sxs-lookup"><span data-stu-id="5a098-113">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="5a098-114">Remarques relatives à la conception</span><span class="sxs-lookup"><span data-stu-id="5a098-114">Design considerations</span></span>

<span data-ttu-id="5a098-115">Cette section décrit les modèles qui peuvent être implémentés lors de la conception d’une application Signalr, afin de garantir que les performances ne sont pas gênées par la génération d’un trafic réseau inutile.</span><span class="sxs-lookup"><span data-stu-id="5a098-115">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="5a098-116">Limitation de la fréquence des messages</span><span class="sxs-lookup"><span data-stu-id="5a098-116">Throttling message frequency</span></span>

<span data-ttu-id="5a098-117">Même dans une application qui envoie des messages à une fréquence élevée (par exemple, une application de jeu en temps réel), la plupart des applications n’ont pas besoin d’envoyer plus de quelques messages par seconde.</span><span class="sxs-lookup"><span data-stu-id="5a098-117">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="5a098-118">Pour réduire le volume de trafic généré par chaque client, il est possible d’implémenter une boucle de messages qui met en file d’attente et envoie des messages plus fréquemment qu’un taux fixe (autrement dit, jusqu’à un certain nombre de messages sont envoyés chaque seconde, s’il y a des messages dans ce délai en onstruction à envoyer).</span><span class="sxs-lookup"><span data-stu-id="5a098-118">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="5a098-119">Pour obtenir un exemple d’application qui limite les messages à un certain taux (à partir du client et du serveur), consultez [temps réel haute fréquence avec signalr](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="5a098-119">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="5a098-120">Réduction de la taille des messages</span><span class="sxs-lookup"><span data-stu-id="5a098-120">Reducing message size</span></span>

<span data-ttu-id="5a098-121">Vous pouvez réduire la taille d’un message Signalr en réduisant la taille de vos objets sérialisés.</span><span class="sxs-lookup"><span data-stu-id="5a098-121">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="5a098-122">Dans le code serveur, si vous envoyez un objet qui contient des propriétés qui n’ont pas besoin d’être transmises, empêchez la sérialisation de ces propriétés à l’aide de l’attribut `JsonIgnore`.</span><span class="sxs-lookup"><span data-stu-id="5a098-122">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="5a098-123">Les noms des propriétés sont également stockés dans le message. vous pouvez raccourcir les noms des propriétés à l’aide de l’attribut `JsonProperty`.</span><span class="sxs-lookup"><span data-stu-id="5a098-123">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="5a098-124">L’exemple de code suivant montre comment exclure une propriété de l’envoi au client et comment raccourcir les noms de propriété :</span><span class="sxs-lookup"><span data-stu-id="5a098-124">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="5a098-125">**Code du serveur .NET illustrant l’attribut JsonIgnore pour empêcher l’envoi de données au client et l’attribut JsonProperty pour réduire la taille des messages**</span><span class="sxs-lookup"><span data-stu-id="5a098-125">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="5a098-126">Afin de conserver la lisibilité et la maintenabilité dans le code client, les noms de propriété abrégés peuvent être remappés à des noms conviviaux une fois le message reçu.</span><span class="sxs-lookup"><span data-stu-id="5a098-126">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="5a098-127">L’exemple de code suivant montre une manière possible de remapper les noms abrégés à des noms plus longs, en définissant un contrat de message (mappage) et en utilisant la fonction `reMap` pour appliquer le contrat à la classe de message optimisée :</span><span class="sxs-lookup"><span data-stu-id="5a098-127">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="5a098-128">**Code JavaScript côté client qui remappe les noms de propriété abrégés à des noms explicites**</span><span class="sxs-lookup"><span data-stu-id="5a098-128">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="5a098-129">Il est également possible de raccourcir les noms des messages du client au serveur, en utilisant la même méthode.</span><span class="sxs-lookup"><span data-stu-id="5a098-129">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="5a098-130">La réduction de l’encombrement mémoire (c’est-à-dire la quantité de mémoire utilisée pour le message) de l’objet message peut également améliorer les performances.</span><span class="sxs-lookup"><span data-stu-id="5a098-130">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="5a098-131">Par exemple, si la plage complète d’un `int` n’est pas nécessaire, un `short` ou `byte` peut être utilisé à la place.</span><span class="sxs-lookup"><span data-stu-id="5a098-131">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="5a098-132">Étant donné que les messages sont stockés dans le bus de messages dans la mémoire du serveur, la réduction de la taille des messages peut également résoudre les problèmes de mémoire du serveur.</span><span class="sxs-lookup"><span data-stu-id="5a098-132">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="5a098-133">Réglage des performances de votre serveur Signalr</span><span class="sxs-lookup"><span data-stu-id="5a098-133">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="5a098-134">Les paramètres de configuration suivants peuvent être utilisés pour adapter votre serveur afin d’améliorer les performances dans une application Signalr.</span><span class="sxs-lookup"><span data-stu-id="5a098-134">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="5a098-135">Pour obtenir des informations générales sur l’amélioration des performances dans une application ASP.NET, consultez [amélioration des performances des ASP.net](https://msdn.microsoft.com/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="5a098-135">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="5a098-136">**Paramètres de configuration de signalr**</span><span class="sxs-lookup"><span data-stu-id="5a098-136">**SignalR configuration settings**</span></span>

- <span data-ttu-id="5a098-137">**DefaultMessageBufferSize**: par défaut, signalr conserve 1000 messages en mémoire par concentrateur par connexion.</span><span class="sxs-lookup"><span data-stu-id="5a098-137">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="5a098-138">Si des messages volumineux sont utilisés, cela peut créer des problèmes de mémoire qui peuvent être atténués en réduisant cette valeur.</span><span class="sxs-lookup"><span data-stu-id="5a098-138">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="5a098-139">Ce paramètre peut être défini dans le gestionnaire d’événements `Application_Start` dans une application ASP.NET ou dans la méthode `Configuration` d’une classe de démarrage OWIN dans une application auto-hébergée.</span><span class="sxs-lookup"><span data-stu-id="5a098-139">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="5a098-140">L’exemple suivant montre comment réduire cette valeur afin de réduire l’encombrement mémoire de votre application afin de réduire la quantité de mémoire du serveur utilisée :</span><span class="sxs-lookup"><span data-stu-id="5a098-140">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="5a098-141">**Code du serveur .NET dans global. asax pour la réduction de la taille de la mémoire tampon des messages par défaut**</span><span class="sxs-lookup"><span data-stu-id="5a098-141">**.NET server code in Global.asax for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="5a098-142">**Paramètres de configuration IIS**</span><span class="sxs-lookup"><span data-stu-id="5a098-142">**IIS configuration settings**</span></span>

- <span data-ttu-id="5a098-143">Nombre **maximal de demandes simultanées par application**: l’augmentation du nombre de demandes IIS simultanées augmente les ressources serveur disponibles pour traiter les demandes.</span><span class="sxs-lookup"><span data-stu-id="5a098-143">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="5a098-144">La valeur par défaut est 5000 ; pour augmenter ce paramètre, exécutez les commandes suivantes dans une invite de commandes avec élévation de privilèges :</span><span class="sxs-lookup"><span data-stu-id="5a098-144">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]

<span data-ttu-id="5a098-145">**Paramètres de configuration de ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="5a098-145">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="5a098-146">Cette section comprend des paramètres de configuration qui peuvent être définis dans le fichier `aspnet.config`.</span><span class="sxs-lookup"><span data-stu-id="5a098-146">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="5a098-147">Ce fichier se trouve dans l’un des deux emplacements, en fonction de la plateforme :</span><span class="sxs-lookup"><span data-stu-id="5a098-147">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="5a098-148">Les paramètres ASP.NET qui peuvent améliorer les performances de signaler sont les suivants :</span><span class="sxs-lookup"><span data-stu-id="5a098-148">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="5a098-149">**Nombre maximal de demandes simultanées par UC**: l’amélioration de ce paramètre peut réduire les goulots d’étranglement de performances.</span><span class="sxs-lookup"><span data-stu-id="5a098-149">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="5a098-150">Pour augmenter ce paramètre, ajoutez le paramètre de configuration suivant au fichier `aspnet.config` :</span><span class="sxs-lookup"><span data-stu-id="5a098-150">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="5a098-151">**Limite de la file d’attente des demandes**: lorsque le nombre total de connexions dépasse le paramètre `maxConcurrentRequestsPerCPU`, ASP.net commence à limiter les demandes à l’aide d’une file d’attente.</span><span class="sxs-lookup"><span data-stu-id="5a098-151">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="5a098-152">Pour augmenter la taille de la file d’attente, vous pouvez augmenter le paramètre `requestQueueLimit`.</span><span class="sxs-lookup"><span data-stu-id="5a098-152">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="5a098-153">Pour ce faire, ajoutez le paramètre de configuration suivant au nœud `processModel` dans `config/machine.config` (plutôt que `aspnet.config`) :</span><span class="sxs-lookup"><span data-stu-id="5a098-153">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="5a098-154">Résolution des problèmes de performances</span><span class="sxs-lookup"><span data-stu-id="5a098-154">Troubleshooting performance issues</span></span>

<span data-ttu-id="5a098-155">Cette section décrit les méthodes permettant de trouver des goulots d’étranglement au niveau des performances dans votre application.</span><span class="sxs-lookup"><span data-stu-id="5a098-155">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="5a098-156">Vérification de l’utilisation de WebSocket</span><span class="sxs-lookup"><span data-stu-id="5a098-156">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="5a098-157">Alors que Signalr peut utiliser un grand nombre de transports pour la communication entre le client et le serveur, WebSocket présente un avantage significatif en termes de performances et doit être utilisé si le client et le serveur le prennent en charge.</span><span class="sxs-lookup"><span data-stu-id="5a098-157">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="5a098-158">Pour déterminer si votre client et votre serveur satisfont à la configuration requise pour WebSocket, consultez [transports et secours](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="5a098-158">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="5a098-159">Pour déterminer le transport utilisé dans votre application, vous pouvez utiliser les outils de développement du navigateur et examiner les journaux pour voir quel transport est utilisé pour la connexion.</span><span class="sxs-lookup"><span data-stu-id="5a098-159">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="5a098-160">Pour plus d’informations sur l’utilisation des outils de développement de navigateur dans Internet Explorer et chrome, consultez [transports et secours](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="5a098-160">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="5a098-161">Utilisation des compteurs de performance Signalr</span><span class="sxs-lookup"><span data-stu-id="5a098-161">Using SignalR performance counters</span></span>

<span data-ttu-id="5a098-162">Cette section décrit comment activer et utiliser les compteurs de performance Signalr, qui se trouvent dans le package `Microsoft.AspNet.SignalR.Utils`.</span><span class="sxs-lookup"><span data-stu-id="5a098-162">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="5a098-163">Installation de signalr. exe</span><span class="sxs-lookup"><span data-stu-id="5a098-163">Installing signalr.exe</span></span>

<span data-ttu-id="5a098-164">Les compteurs de performances peuvent être ajoutés au serveur à l’aide d’un utilitaire appelé Signalr. exe.</span><span class="sxs-lookup"><span data-stu-id="5a098-164">Performance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="5a098-165">Pour installer cet utilitaire, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="5a098-165">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="5a098-166">Dans Visual Studio, sélectionnez **outils** > **Gestionnaire de package NuGet** > **gérer les packages NuGet pour la solution**</span><span class="sxs-lookup"><span data-stu-id="5a098-166">In Visual Studio, select **Tools** > **NuGet Package Manager** > **Manage NuGet Packages for Solution**</span></span>
2. <span data-ttu-id="5a098-167">Recherchez **signalr. utils**, puis sélectionnez Installer.</span><span class="sxs-lookup"><span data-stu-id="5a098-167">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="5a098-168">Acceptez le contrat de licence pour installer le package.</span><span class="sxs-lookup"><span data-stu-id="5a098-168">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="5a098-169">Signalr. exe sera installé dans `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span><span class="sxs-lookup"><span data-stu-id="5a098-169">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="5a098-170">Installation des compteurs de performances avec Signalr. exe</span><span class="sxs-lookup"><span data-stu-id="5a098-170">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="5a098-171">Pour installer les compteurs de performance Signalr, exécutez Signalr. exe dans une invite de commandes avec élévation de privilèges avec le paramètre suivant :</span><span class="sxs-lookup"><span data-stu-id="5a098-171">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="5a098-172">Pour supprimer les compteurs de performance Signalr, exécutez Signalr. exe dans une invite de commandes avec élévation de privilèges avec le paramètre suivant :</span><span class="sxs-lookup"><span data-stu-id="5a098-172">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="5a098-173">Compteurs de performance signalr</span><span class="sxs-lookup"><span data-stu-id="5a098-173">SignalR Performance counters</span></span>

<span data-ttu-id="5a098-174">Le package utilitaires installe les compteurs de performances suivants.</span><span class="sxs-lookup"><span data-stu-id="5a098-174">The utilities package installs the following performance counters.</span></span> <span data-ttu-id="5a098-175">Les compteurs « total » mesurent le nombre d’événements depuis le dernier redémarrage du pool d’applications ou du serveur.</span><span class="sxs-lookup"><span data-stu-id="5a098-175">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="5a098-176">**Métriques de connexion**</span><span class="sxs-lookup"><span data-stu-id="5a098-176">**Connection metrics**</span></span>

<span data-ttu-id="5a098-177">Les métriques suivantes mesurent les événements de durée de vie de la connexion qui se produisent.</span><span class="sxs-lookup"><span data-stu-id="5a098-177">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="5a098-178">Pour plus d’informations, consultez [comprendre et gérer les événements de durée de vie des connexions](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="5a098-178">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="5a098-179">**Connexions connectées**</span><span class="sxs-lookup"><span data-stu-id="5a098-179">**Connections Connected**</span></span>
- <span data-ttu-id="5a098-180">**Connexions reconnectées**</span><span class="sxs-lookup"><span data-stu-id="5a098-180">**Connections Reconnected**</span></span>
- <span data-ttu-id="5a098-181">**Connexions déconnectées**</span><span class="sxs-lookup"><span data-stu-id="5a098-181">**Connections Disconnected**</span></span>
- <span data-ttu-id="5a098-182">**Connexions en cours**</span><span class="sxs-lookup"><span data-stu-id="5a098-182">**Connections Current**</span></span>

<span data-ttu-id="5a098-183">**Métriques du message**</span><span class="sxs-lookup"><span data-stu-id="5a098-183">**Message metrics**</span></span>

<span data-ttu-id="5a098-184">Les métriques suivantes mesurent le trafic de messages généré par Signalr.</span><span class="sxs-lookup"><span data-stu-id="5a098-184">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="5a098-185">**Nombre total de messages de connexion reçus**</span><span class="sxs-lookup"><span data-stu-id="5a098-185">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="5a098-186">**Nombre total de messages de connexion envoyés**</span><span class="sxs-lookup"><span data-stu-id="5a098-186">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="5a098-187">**Messages de connexion reçus/s**</span><span class="sxs-lookup"><span data-stu-id="5a098-187">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="5a098-188">**Messages de connexion envoyés/s**</span><span class="sxs-lookup"><span data-stu-id="5a098-188">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="5a098-189">**Métriques du bus de messages**</span><span class="sxs-lookup"><span data-stu-id="5a098-189">**Message bus metrics**</span></span>

<span data-ttu-id="5a098-190">Les métriques suivantes mesurent le trafic via le bus de messages Signalr interne, la file d’attente dans laquelle tous les messages entrants et sortants Signalr sont placés.</span><span class="sxs-lookup"><span data-stu-id="5a098-190">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="5a098-191">Un message est **publié** lors de son envoi ou de sa diffusion.</span><span class="sxs-lookup"><span data-stu-id="5a098-191">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="5a098-192">Un **abonné** dans ce contexte est un abonnement sur le bus de messages ; Cela doit être égal au nombre de clients plus le serveur lui-même.</span><span class="sxs-lookup"><span data-stu-id="5a098-192">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="5a098-193">Un **Worker alloué** est un composant qui envoie des données à des connexions actives. un **Worker occupé est un Worker** qui envoie activement un message.</span><span class="sxs-lookup"><span data-stu-id="5a098-193">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="5a098-194">**Nombre total de messages reçus par le bus de messages**</span><span class="sxs-lookup"><span data-stu-id="5a098-194">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="5a098-195">**Messages du bus de messages reçus/s**</span><span class="sxs-lookup"><span data-stu-id="5a098-195">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="5a098-196">**Nombre total de messages publiés par le bus de messages**</span><span class="sxs-lookup"><span data-stu-id="5a098-196">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="5a098-197">**Messages du bus de messages publiés/s**</span><span class="sxs-lookup"><span data-stu-id="5a098-197">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="5a098-198">**Abonnés au bus de messages actuel**</span><span class="sxs-lookup"><span data-stu-id="5a098-198">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="5a098-199">**Nombre total d’abonnés au bus de messages**</span><span class="sxs-lookup"><span data-stu-id="5a098-199">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="5a098-200">**Abonnés au bus de messages/s**</span><span class="sxs-lookup"><span data-stu-id="5a098-200">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="5a098-201">**Threads de messagerie alloués**</span><span class="sxs-lookup"><span data-stu-id="5a098-201">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="5a098-202">**Threads occupés par le bus de messages**</span><span class="sxs-lookup"><span data-stu-id="5a098-202">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="5a098-203">**Rubriques du bus de messages en cours**</span><span class="sxs-lookup"><span data-stu-id="5a098-203">**Message Bus Topics Current**</span></span>

<span data-ttu-id="5a098-204">**Métriques d’erreur**</span><span class="sxs-lookup"><span data-stu-id="5a098-204">**Error metrics**</span></span>

<span data-ttu-id="5a098-205">Les métriques suivantes mesurent les erreurs générées par le trafic des messages Signalr.</span><span class="sxs-lookup"><span data-stu-id="5a098-205">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="5a098-206">Des erreurs de **résolution de concentrateur** se produisent lorsqu’une méthode de concentrateur ou de concentrateur ne peut pas être résolue.</span><span class="sxs-lookup"><span data-stu-id="5a098-206">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="5a098-207">Les erreurs d' **appel de concentrateur** sont des exceptions levées lors de l’appel d’une méthode de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="5a098-207">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="5a098-208">Les erreurs de **transport** sont des erreurs de connexion levées au cours d’une requête ou d’une réponse http.</span><span class="sxs-lookup"><span data-stu-id="5a098-208">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="5a098-209">**Erreurs : total**</span><span class="sxs-lookup"><span data-stu-id="5a098-209">**Errors: All Total**</span></span>
- <span data-ttu-id="5a098-210">**Erreurs : toutes les/s**</span><span class="sxs-lookup"><span data-stu-id="5a098-210">**Errors: All/Sec**</span></span>
- <span data-ttu-id="5a098-211">**Erreurs : total de résolution de concentrateur**</span><span class="sxs-lookup"><span data-stu-id="5a098-211">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="5a098-212">**Erreurs : résolution de concentrateur/s**</span><span class="sxs-lookup"><span data-stu-id="5a098-212">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="5a098-213">**Erreurs : total d’invocation de concentrateur**</span><span class="sxs-lookup"><span data-stu-id="5a098-213">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="5a098-214">**Erreurs : appel de Hub/s**</span><span class="sxs-lookup"><span data-stu-id="5a098-214">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="5a098-215">**Erreurs : transport total**</span><span class="sxs-lookup"><span data-stu-id="5a098-215">**Errors: Transport Total**</span></span>
- <span data-ttu-id="5a098-216">**Erreurs : transport/s**</span><span class="sxs-lookup"><span data-stu-id="5a098-216">**Errors: Transport/Sec**</span></span>

<span data-ttu-id="5a098-217">**Mesures ScaleOut**</span><span class="sxs-lookup"><span data-stu-id="5a098-217">**Scaleout metrics**</span></span>

<span data-ttu-id="5a098-218">Les métriques suivantes mesurent le trafic et les erreurs générées par le fournisseur ScaleOut.</span><span class="sxs-lookup"><span data-stu-id="5a098-218">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="5a098-219">Un **flux** dans ce contexte est une unité d’échelle utilisée par le fournisseur ScaleOut. Il s’agit d’une table si SQL Server est utilisé, une rubrique si Service Bus est utilisé et un abonnement si Redims est utilisé.</span><span class="sxs-lookup"><span data-stu-id="5a098-219">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="5a098-220">Par défaut, un seul flux est utilisé, mais il peut être augmenté via la configuration de SQL Server et Service Bus.</span><span class="sxs-lookup"><span data-stu-id="5a098-220">By default, only one stream is used, but this can be increased through configuration on SQL Server and Service Bus.</span></span> <span data-ttu-id="5a098-221">Un flux de **mise en mémoire tampon** est un flux qui est entré dans un état d’erreur ; Lorsque le flux est dans l’état Faulted, tous les messages envoyés au backplane échouent immédiatement jusqu’à ce que le flux ne génère plus de défaillance.</span><span class="sxs-lookup"><span data-stu-id="5a098-221">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="5a098-222">La **longueur de la file d’attente d’envoi** est le nombre de messages qui ont été publiés mais pas encore envoyés.</span><span class="sxs-lookup"><span data-stu-id="5a098-222">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="5a098-223">**Messages du bus des messages ScaleOut reçus/s**</span><span class="sxs-lookup"><span data-stu-id="5a098-223">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="5a098-224">**Total des flux ScaleOut**</span><span class="sxs-lookup"><span data-stu-id="5a098-224">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="5a098-225">**Flux ScaleOut ouverts**</span><span class="sxs-lookup"><span data-stu-id="5a098-225">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="5a098-226">**Mise en mémoire tampon des flux ScaleOut**</span><span class="sxs-lookup"><span data-stu-id="5a098-226">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="5a098-227">**Nombre total d’erreurs ScaleOut**</span><span class="sxs-lookup"><span data-stu-id="5a098-227">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="5a098-228">**Erreurs de ScaleOut/s**</span><span class="sxs-lookup"><span data-stu-id="5a098-228">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="5a098-229">**Longueur de la file d’attente d’envoi ScaleOut**</span><span class="sxs-lookup"><span data-stu-id="5a098-229">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="5a098-230">Pour plus d’informations sur la mesure de ces compteurs, consultez [signalr ScaleOut avec Azure Service bus](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="5a098-230">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="5a098-231">Utilisation d’autres compteurs de performances</span><span class="sxs-lookup"><span data-stu-id="5a098-231">Using other performance counters</span></span>

<span data-ttu-id="5a098-232">Les compteurs de performances suivants peuvent également être utiles pour analyser les performances de votre application.</span><span class="sxs-lookup"><span data-stu-id="5a098-232">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="5a098-233">**Mémoire**</span><span class="sxs-lookup"><span data-stu-id="5a098-233">**Memory**</span></span>

- <span data-ttu-id="5a098-234">Mémoire CLR .NET nombre d’octets dans tous les tas (pour w3wp)</span><span class="sxs-lookup"><span data-stu-id="5a098-234">.NET CLR Memory# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="5a098-235">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="5a098-235">**ASP.NET**</span></span>

- <span data-ttu-id="5a098-236">ASP. asp.net \ requêtes actuel</span><span class="sxs-lookup"><span data-stu-id="5a098-236">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="5a098-237">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="5a098-237">ASP.NET\Queued</span></span>
- <span data-ttu-id="5a098-238">ASP. NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="5a098-238">ASP.NET\Rejected</span></span>

<span data-ttu-id="5a098-239">**Processeur**</span><span class="sxs-lookup"><span data-stu-id="5a098-239">**CPU**</span></span>

- <span data-ttu-id="5a098-240">Temps de Information\Processor du processeur</span><span class="sxs-lookup"><span data-stu-id="5a098-240">Processor Information\Processor Time</span></span>

<span data-ttu-id="5a098-241">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="5a098-241">**TCP/IP**</span></span>

- <span data-ttu-id="5a098-242">TCPv6/connexions établies</span><span class="sxs-lookup"><span data-stu-id="5a098-242">TCPv6/Connections Established</span></span>
- <span data-ttu-id="5a098-243">TCPv4/connexions établies</span><span class="sxs-lookup"><span data-stu-id="5a098-243">TCPv4/Connections Established</span></span>

<span data-ttu-id="5a098-244">**Service Web**</span><span class="sxs-lookup"><span data-stu-id="5a098-244">**Web Service**</span></span>

- <span data-ttu-id="5a098-245">Connexions Web Web\connexions</span><span class="sxs-lookup"><span data-stu-id="5a098-245">Web Service\Current Connections</span></span>
- <span data-ttu-id="5a098-246">Connexions Web Service\Maximum</span><span class="sxs-lookup"><span data-stu-id="5a098-246">Web Service\Maximum Connections</span></span>

<span data-ttu-id="5a098-247">**Thread**</span><span class="sxs-lookup"><span data-stu-id="5a098-247">**Threading**</span></span>

- <span data-ttu-id="5a098-248">\# CLR .NET LocksAndThreads de threads logiques actuels</span><span class="sxs-lookup"><span data-stu-id="5a098-248">.NET CLR LocksAndThreads\# of current logical Threads</span></span>
- <span data-ttu-id="5a098-249">Threads .NET CLR LocksAnd\# de threads physiques actuels</span><span class="sxs-lookup"><span data-stu-id="5a098-249">.NET CLR LocksAnd Threads\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="5a098-250">Autres ressources</span><span class="sxs-lookup"><span data-stu-id="5a098-250">Other Resources</span></span>

<span data-ttu-id="5a098-251">Pour plus d’informations sur l’analyse et le réglage des performances ASP.NET, consultez les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="5a098-251">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="5a098-252">[Vue d’ensemble des performances ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="5a098-252">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="5a098-253">Utilisation des threads ASP.NET sur IIS 7,5, IIS 7,0 et IIS 6,0</span><span class="sxs-lookup"><span data-stu-id="5a098-253">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="5a098-254">&lt;applicationPool&gt;, élément (paramètres Web)</span><span class="sxs-lookup"><span data-stu-id="5a098-254">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
