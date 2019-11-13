---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-net-client
title: Guide de l’API des hubs Signalr ASP.NET-C#client .net () | Microsoft Docs
author: bradygaster
description: Ce document fournit une introduction à l’utilisation de l’API hubs pour Signalr version 2 dans les clients .NET, tels que le Windows Store (WinRT), WPF, Silverlight et les inconvénients...
ms.author: bradyg
ms.date: 01/15/2019
ms.assetid: 6d02d9f7-94e5-4140-9f51-5a6040f274f6
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: d3536f1c15cd7dad7cd660becf0577e5c131f707
ms.sourcegitcommit: 295cf898a4c87e264b0c35c7254b0fa4169f2278
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/13/2019
ms.locfileid: "74057007"
---
# <a name="aspnet-signalr-hubs-api-guide---net-client-c"></a><span data-ttu-id="bde8f-103">Guide de l’API des hubs Signalr ASP.NET-C#client .net ()</span><span class="sxs-lookup"><span data-stu-id="bde8f-103">ASP.NET SignalR Hubs API Guide - .NET Client (C#)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="bde8f-104">Ce document fournit une introduction à l’utilisation de l’API hubs pour Signalr version 2 dans les clients .NET, tels que les applications Windows Store (WinRT), WPF, Silverlight et console.</span><span class="sxs-lookup"><span data-stu-id="bde8f-104">This document provides an introduction to using the Hubs API for SignalR version 2 in .NET clients, such as Windows Store (WinRT), WPF, Silverlight, and console applications.</span></span>
>
> <span data-ttu-id="bde8f-105">L’API Signalr hubs vous permet d’effectuer des appels de procédure distante (RPC) à partir d’un serveur vers des clients connectés et des clients vers le serveur.</span><span class="sxs-lookup"><span data-stu-id="bde8f-105">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="bde8f-106">Dans le code serveur, vous définissez des méthodes qui peuvent être appelées par les clients, et vous appelez des méthodes qui s’exécutent sur le client.</span><span class="sxs-lookup"><span data-stu-id="bde8f-106">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="bde8f-107">Dans le code client, vous définissez des méthodes qui peuvent être appelées à partir du serveur, et vous appelez des méthodes qui s’exécutent sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="bde8f-107">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="bde8f-108">Signalr prend en charge tous les éléments de la plombation client à serveur pour vous.</span><span class="sxs-lookup"><span data-stu-id="bde8f-108">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
>
> <span data-ttu-id="bde8f-109">Signalr offre également une API de bas niveau appelée connexions persistantes.</span><span class="sxs-lookup"><span data-stu-id="bde8f-109">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="bde8f-110">Pour obtenir une présentation de Signalr, de hubs et de connexions persistantes, ou pour obtenir un didacticiel qui montre comment générer une application Signalr complète, consultez [signalr-prise en main](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="bde8f-110">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="bde8f-111">Versions logicielles utilisées dans cette rubrique</span><span class="sxs-lookup"><span data-stu-id="bde8f-111">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="bde8f-112">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="bde8f-112">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/)
> - <span data-ttu-id="bde8f-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="bde8f-113">.NET 4.5</span></span>
> - <span data-ttu-id="bde8f-114">Signalr version 2</span><span class="sxs-lookup"><span data-stu-id="bde8f-114">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="bde8f-115">Versions précédentes de cette rubrique</span><span class="sxs-lookup"><span data-stu-id="bde8f-115">Previous versions of this topic</span></span>
>
> <span data-ttu-id="bde8f-116">Pour plus d’informations sur les versions antérieures de Signalr, consultez [versions antérieures de signalr](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="bde8f-116">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="bde8f-117">Questions et commentaires</span><span class="sxs-lookup"><span data-stu-id="bde8f-117">Questions and comments</span></span>
>
> <span data-ttu-id="bde8f-118">N’hésitez pas à nous faire part de vos commentaires sur la façon dont vous aimez ce didacticiel et sur ce que nous pourrions améliorer dans les commentaires en bas de la page.</span><span class="sxs-lookup"><span data-stu-id="bde8f-118">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="bde8f-119">Si vous avez des questions qui ne sont pas directement liées au didacticiel, vous pouvez les poster sur le [forum ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="bde8f-119">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="bde8f-120">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="bde8f-120">Overview</span></span>

<span data-ttu-id="bde8f-121">Ce document contient les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="bde8f-121">This document contains the following sections:</span></span>

- [<span data-ttu-id="bde8f-122">Installation du client</span><span class="sxs-lookup"><span data-stu-id="bde8f-122">Client Setup</span></span>](#clientsetup)
- [<span data-ttu-id="bde8f-123">Comment établir une connexion</span><span class="sxs-lookup"><span data-stu-id="bde8f-123">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="bde8f-124">Connexions inter-domaines à partir de clients Silverlight</span><span class="sxs-lookup"><span data-stu-id="bde8f-124">Cross-domain connections from Silverlight clients</span></span>](#slcrossdomain)
- [<span data-ttu-id="bde8f-125">Comment configurer la connexion</span><span class="sxs-lookup"><span data-stu-id="bde8f-125">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="bde8f-126">Comment définir le nombre maximal de connexions simultanées dans les clients WPF</span><span class="sxs-lookup"><span data-stu-id="bde8f-126">How to set the maximum number of concurrent connections in WPF clients</span></span>](#maxconnections)
    - [<span data-ttu-id="bde8f-127">Comment spécifier des paramètres de chaîne de requête</span><span class="sxs-lookup"><span data-stu-id="bde8f-127">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="bde8f-128">Comment spécifier la méthode de transport</span><span class="sxs-lookup"><span data-stu-id="bde8f-128">How to specify the transport method</span></span>](#transport)
    - [<span data-ttu-id="bde8f-129">Comment spécifier des en-têtes HTTP</span><span class="sxs-lookup"><span data-stu-id="bde8f-129">How to specify HTTP headers</span></span>](#httpheaders)
    - [<span data-ttu-id="bde8f-130">Comment spécifier des certificats clients</span><span class="sxs-lookup"><span data-stu-id="bde8f-130">How to specify client certificates</span></span>](#clientcertificate)
- [<span data-ttu-id="bde8f-131">Comment créer le proxy Hub</span><span class="sxs-lookup"><span data-stu-id="bde8f-131">How to create the Hub proxy</span></span>](#proxy)
- [<span data-ttu-id="bde8f-132">Comment définir des méthodes sur le client que le serveur peut appeler</span><span class="sxs-lookup"><span data-stu-id="bde8f-132">How to define methods on the client that the server can call</span></span>](#callclient)

    - [<span data-ttu-id="bde8f-133">Méthodes sans paramètres</span><span class="sxs-lookup"><span data-stu-id="bde8f-133">Methods without parameters</span></span>](#clientmethodswithoutparms)
    - [<span data-ttu-id="bde8f-134">Méthodes avec des paramètres, spécification de types de paramètres</span><span class="sxs-lookup"><span data-stu-id="bde8f-134">Methods with parameters, specifying parameter types</span></span>](#clientmethodswithparmtypes)
    - [<span data-ttu-id="bde8f-135">Méthodes avec des paramètres, spécification d’objets dynamiques pour les paramètres</span><span class="sxs-lookup"><span data-stu-id="bde8f-135">Methods with parameters, specifying dynamic objects for the parameters</span></span>](#clientmethodswithdynamparms)
    - [<span data-ttu-id="bde8f-136">Comment supprimer un gestionnaire</span><span class="sxs-lookup"><span data-stu-id="bde8f-136">How to remove a handler</span></span>](#removehandler)
- [<span data-ttu-id="bde8f-137">Comment appeler des méthodes de serveur à partir du client</span><span class="sxs-lookup"><span data-stu-id="bde8f-137">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="bde8f-138">Comment gérer les événements de durée de vie de connexion</span><span class="sxs-lookup"><span data-stu-id="bde8f-138">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="bde8f-139">Gestion des erreurs</span><span class="sxs-lookup"><span data-stu-id="bde8f-139">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="bde8f-140">Comment activer la journalisation côté client</span><span class="sxs-lookup"><span data-stu-id="bde8f-140">How to enable client-side logging</span></span>](#logging)
- [<span data-ttu-id="bde8f-141">Exemples de code d’application de console WPF, Silverlight et console pour les méthodes clientes que le serveur peut appeler</span><span class="sxs-lookup"><span data-stu-id="bde8f-141">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>](#wpfsl)

<span data-ttu-id="bde8f-142">Pour obtenir un exemple de projets clients .NET, consultez les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="bde8f-142">For a sample .NET client projects, see the following resources:</span></span>

- <span data-ttu-id="bde8f-143">[Gustavo-Armenta/signalr-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on github.com (exemples d’applications de console WinRT, Silverlight).</span><span class="sxs-lookup"><span data-stu-id="bde8f-143">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, console app examples).</span></span>
- <span data-ttu-id="bde8f-144">[DamianEdwards/signalr-MoveShapeDemo/MoveShape. Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) sur GitHub.com (exemple WPF).</span><span class="sxs-lookup"><span data-stu-id="bde8f-144">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF example).</span></span>
- <span data-ttu-id="bde8f-145">[Signalr/Microsoft. Aspnet. signalr. client. Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) sur GitHub.com (exemple d’application console).</span><span class="sxs-lookup"><span data-stu-id="bde8f-145">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).</span></span>

<span data-ttu-id="bde8f-146">Pour obtenir de la documentation sur la programmation du serveur ou des clients JavaScript, consultez les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="bde8f-146">For documentation on how to program the server or JavaScript clients, see the following resources:</span></span>

- [<span data-ttu-id="bde8f-147">Guide de l’API signalr hubs-serveur</span><span class="sxs-lookup"><span data-stu-id="bde8f-147">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="bde8f-148">Guide de l’API signalr hubs-client JavaScript</span><span class="sxs-lookup"><span data-stu-id="bde8f-148">SignalR Hubs API Guide - JavaScript Client</span></span>](hubs-api-guide-javascript-client.md)

<span data-ttu-id="bde8f-149">Les liens vers les rubriques de référence sur les API concernent la version .NET 4,5 de l’API.</span><span class="sxs-lookup"><span data-stu-id="bde8f-149">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="bde8f-150">Si vous utilisez .NET 4, consultez [la version .net 4 des rubriques de l’API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="bde8f-150">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="bde8f-151">Installation du client</span><span class="sxs-lookup"><span data-stu-id="bde8f-151">Client setup</span></span>

<span data-ttu-id="bde8f-152">Installez le package NuGet [Microsoft. Aspnet. signalr. client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) (et non le package [Microsoft. Aspnet. signalr](http://nuget.org/packages/microsoft.aspnet.signalr) ).</span><span class="sxs-lookup"><span data-stu-id="bde8f-152">Install the [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet package (not the [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span></span> <span data-ttu-id="bde8f-153">Ce package prend en charge WinRT, Silverlight, WPF, l’application console et les clients Windows Phone, pour .NET 4 et .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="bde8f-153">This package supports WinRT, Silverlight, WPF, console application, and Windows Phone clients, for both .NET 4 and .NET 4.5.</span></span>

<span data-ttu-id="bde8f-154">Si la version de Signalr sur le client est différente de celle que vous avez sur le serveur, Signalr est souvent en mesure de s’adapter à la différence.</span><span class="sxs-lookup"><span data-stu-id="bde8f-154">If the version of SignalR that you have on the client is different from the version that you have on the server, SignalR is often able to adapt to the difference.</span></span> <span data-ttu-id="bde8f-155">Par exemple, un serveur exécutant Signalr version 2 prend en charge les clients sur lesquels 1.1. x est installé, ainsi que les clients sur lesquels la version 2 est installée.</span><span class="sxs-lookup"><span data-stu-id="bde8f-155">For example, a server running SignalR version 2 will support clients that have 1.1.x installed as well as clients that have version 2 installed.</span></span> <span data-ttu-id="bde8f-156">Si la différence entre la version sur le serveur et la version sur le client est trop importante ou si le client est plus récent que le serveur, Signalr lève une exception `InvalidOperationException` lorsque le client tente d’établir une connexion.</span><span class="sxs-lookup"><span data-stu-id="bde8f-156">If the difference between the version on the server and the version on the client is too great, or if the client is newer than the server, SignalR throws an `InvalidOperationException` exception when the client tries to establish a connection.</span></span> <span data-ttu-id="bde8f-157">Le message d’erreur est «`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`».</span><span class="sxs-lookup"><span data-stu-id="bde8f-157">The error message is "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="bde8f-158">Comment établir une connexion</span><span class="sxs-lookup"><span data-stu-id="bde8f-158">How to establish a connection</span></span>

<span data-ttu-id="bde8f-159">Avant de pouvoir établir une connexion, vous devez créer un objet `HubConnection` et créer un proxy.</span><span class="sxs-lookup"><span data-stu-id="bde8f-159">Before you can establish a connection, you have to create a `HubConnection` object and create a proxy.</span></span> <span data-ttu-id="bde8f-160">Pour établir la connexion, appelez la méthode `Start` sur l’objet `HubConnection`.</span><span class="sxs-lookup"><span data-stu-id="bde8f-160">To establish the connection, call the `Start` method on the `HubConnection` object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample1.cs?highlight=1,5)]

> [!NOTE]
> <span data-ttu-id="bde8f-161">Pour les clients JavaScript, vous devez inscrire au moins un gestionnaire d’événements avant d’appeler la méthode `Start` pour établir la connexion.</span><span class="sxs-lookup"><span data-stu-id="bde8f-161">For JavaScript clients you have to register at least one event handler before calling the `Start` method to establish the connection.</span></span> <span data-ttu-id="bde8f-162">Cela n’est pas nécessaire pour les clients .NET.</span><span class="sxs-lookup"><span data-stu-id="bde8f-162">This is not necessary for .NET clients.</span></span> <span data-ttu-id="bde8f-163">Pour les clients JavaScript, le code proxy généré crée automatiquement des proxies pour tous les concentrateurs qui existent sur le serveur, et l’inscription d’un gestionnaire est la manière dont vous indiquez les hubs que votre client envisage d’utiliser.</span><span class="sxs-lookup"><span data-stu-id="bde8f-163">For JavaScript clients, the generated proxy code automatically creates proxies for all Hubs that exist on the server, and registering a handler is how you indicate which Hubs your client intends to use.</span></span> <span data-ttu-id="bde8f-164">Mais pour un client .NET que vous créez manuellement des proxies de Hub, Signalr suppose que vous utiliserez un Hub pour lequel vous créez un proxy.</span><span class="sxs-lookup"><span data-stu-id="bde8f-164">But for a .NET client you create Hub proxies manually, so SignalR assumes that you will be using any Hub that you create a proxy for.</span></span>

<span data-ttu-id="bde8f-165">L’exemple de code utilise l’URL « /signalr » par défaut pour se connecter à votre service Signalr.</span><span class="sxs-lookup"><span data-stu-id="bde8f-165">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="bde8f-166">Pour plus d’informations sur la façon de spécifier une autre URL de base, consultez [Guide de l’API ASP.net signalr hubs-Server-The/SIGNALR URL](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="bde8f-166">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="bde8f-167">La méthode `Start` s’exécute de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="bde8f-167">The `Start` method executes asynchronously.</span></span> <span data-ttu-id="bde8f-168">Pour vous assurer que les lignes de code suivantes ne s’exécutent pas tant que la connexion n’est pas établie, utilisez `await` dans une méthode asynchrone ASP.NET 4,5 ou `.Wait()` dans une méthode synchrone.</span><span class="sxs-lookup"><span data-stu-id="bde8f-168">To make sure that subsequent lines of code don't execute until after the connection is established, use `await` in an ASP.NET 4.5 asynchronous method or `.Wait()` in a synchronous method.</span></span> <span data-ttu-id="bde8f-169">N’utilisez pas `.Wait()` dans un client WinRT.</span><span class="sxs-lookup"><span data-stu-id="bde8f-169">Don't use `.Wait()` in a WinRT client.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a><span data-ttu-id="bde8f-170">Connexions inter-domaines à partir de clients Silverlight</span><span class="sxs-lookup"><span data-stu-id="bde8f-170">Cross-domain connections from Silverlight clients</span></span>

<span data-ttu-id="bde8f-171">Pour plus d’informations sur l’activation des connexions inter-domaines à partir de clients Silverlight, consultez Mise à [disposition d’un service au-delà des limites de domaine](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span><span class="sxs-lookup"><span data-stu-id="bde8f-171">For information about how to enable cross-domain connections from Silverlight clients, see [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="bde8f-172">Comment configurer la connexion</span><span class="sxs-lookup"><span data-stu-id="bde8f-172">How to configure the connection</span></span>

<span data-ttu-id="bde8f-173">Avant d’établir une connexion, vous pouvez spécifier l’une des options suivantes :</span><span class="sxs-lookup"><span data-stu-id="bde8f-173">Before you establish a connection, you can specify any of the following options:</span></span>

- <span data-ttu-id="bde8f-174">Limite de connexions simultanées.</span><span class="sxs-lookup"><span data-stu-id="bde8f-174">Concurrent connections limit.</span></span>
- <span data-ttu-id="bde8f-175">Paramètres de chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="bde8f-175">Query string parameters.</span></span>
- <span data-ttu-id="bde8f-176">Méthode de transport.</span><span class="sxs-lookup"><span data-stu-id="bde8f-176">The transport method.</span></span>
- <span data-ttu-id="bde8f-177">En-têtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="bde8f-177">HTTP headers.</span></span>
- <span data-ttu-id="bde8f-178">Certificats clients.</span><span class="sxs-lookup"><span data-stu-id="bde8f-178">Client certificates.</span></span>

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a><span data-ttu-id="bde8f-179">Comment définir le nombre maximal de connexions simultanées dans les clients WPF</span><span class="sxs-lookup"><span data-stu-id="bde8f-179">How to set the maximum number of concurrent connections in WPF clients</span></span>

<span data-ttu-id="bde8f-180">Dans les clients WPF, vous devrez peut-être augmenter le nombre maximal de connexions simultanées de la valeur 2 par défaut.</span><span class="sxs-lookup"><span data-stu-id="bde8f-180">In WPF clients, you might have to increase the maximum number of concurrent connections from its default value of 2.</span></span> <span data-ttu-id="bde8f-181">La valeur recommandée est 10.</span><span class="sxs-lookup"><span data-stu-id="bde8f-181">The recommended value is 10.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample4.cs?highlight=5)]

<span data-ttu-id="bde8f-182">Pour plus d’informations, consultez [ServicePointManager. DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span><span class="sxs-lookup"><span data-stu-id="bde8f-182">For more information, see [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="bde8f-183">Comment spécifier des paramètres de chaîne de requête</span><span class="sxs-lookup"><span data-stu-id="bde8f-183">How to specify query string parameters</span></span>

<span data-ttu-id="bde8f-184">Si vous souhaitez envoyer des données au serveur lorsque le client se connecte, vous pouvez ajouter des paramètres de chaîne de requête à l’objet de connexion.</span><span class="sxs-lookup"><span data-stu-id="bde8f-184">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="bde8f-185">L’exemple suivant montre comment définir un paramètre de chaîne de requête dans le code client.</span><span class="sxs-lookup"><span data-stu-id="bde8f-185">The following example shows how to set a query string parameter in client code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample5.cs)]

<span data-ttu-id="bde8f-186">L’exemple suivant montre comment lire un paramètre de chaîne de requête dans le code serveur.</span><span class="sxs-lookup"><span data-stu-id="bde8f-186">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="bde8f-187">Comment spécifier la méthode de transport</span><span class="sxs-lookup"><span data-stu-id="bde8f-187">How to specify the transport method</span></span>

<span data-ttu-id="bde8f-188">Dans le cadre du processus de connexion, un client Signalr négocie normalement avec le serveur pour déterminer le meilleur transport pris en charge par le serveur et le client.</span><span class="sxs-lookup"><span data-stu-id="bde8f-188">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="bde8f-189">Si vous savez déjà quel transport vous souhaitez utiliser, vous pouvez ignorer ce processus de négociation.</span><span class="sxs-lookup"><span data-stu-id="bde8f-189">If you already know which transport you want to use, you can bypass this negotiation process.</span></span> <span data-ttu-id="bde8f-190">Pour spécifier la méthode de transport, transmettez un objet de transport à la méthode Start.</span><span class="sxs-lookup"><span data-stu-id="bde8f-190">To specify the transport method, pass in a transport object to the Start method.</span></span> <span data-ttu-id="bde8f-191">L’exemple suivant montre comment spécifier la méthode de transport dans le code client.</span><span class="sxs-lookup"><span data-stu-id="bde8f-191">The following example shows how to specify the transport method in client code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample7.cs?highlight=5)]

<span data-ttu-id="bde8f-192">L’espace de noms [Microsoft. Aspnet. signalr. client. Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) contient les classes suivantes que vous pouvez utiliser pour spécifier le transport.</span><span class="sxs-lookup"><span data-stu-id="bde8f-192">The [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) namespace includes the following classes that you can use to specify the transport.</span></span>

- <span data-ttu-id="bde8f-193">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="bde8f-193">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="bde8f-194">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="bde8f-194">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="bde8f-195">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (disponible uniquement lorsque le serveur et le client utilisent le .net 4,5.)</span><span class="sxs-lookup"><span data-stu-id="bde8f-195">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (Available only when both server and client use .NET 4.5.)</span></span>
- <span data-ttu-id="bde8f-196">[Autotransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (choisit automatiquement le transport le mieux pris en charge par le client et le serveur.</span><span class="sxs-lookup"><span data-stu-id="bde8f-196">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (Automatically chooses the best transport that is supported by both the client and the server.</span></span> <span data-ttu-id="bde8f-197">Il s’agit du transport par défaut.</span><span class="sxs-lookup"><span data-stu-id="bde8f-197">This is the default transport.</span></span> <span data-ttu-id="bde8f-198">Le passage de cette valeur à la méthode `Start` a le même effet que de ne pas passer quoi que ce soit.)</span><span class="sxs-lookup"><span data-stu-id="bde8f-198">Passing this in to the `Start` method has the same effect as not passing in anything.)</span></span>

<span data-ttu-id="bde8f-199">Le transport ForeverFrame n’est pas inclus dans cette liste, car il n’est utilisé que par les navigateurs.</span><span class="sxs-lookup"><span data-stu-id="bde8f-199">The ForeverFrame transport is not included in this list because it is used only by browsers.</span></span>

<span data-ttu-id="bde8f-200">Pour plus d’informations sur la vérification de la méthode de transport dans le code serveur, consultez Guide de l' [API ASP.net signalr Hub-Server-How pour obtenir des informations sur le client à partir de la propriété de contexte](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="bde8f-200">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="bde8f-201">Pour plus d’informations sur les transports et les secours, consultez [Présentation de signalr-transports et de secours](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="bde8f-201">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a><span data-ttu-id="bde8f-202">Comment spécifier des en-têtes HTTP</span><span class="sxs-lookup"><span data-stu-id="bde8f-202">How to specify HTTP headers</span></span>

<span data-ttu-id="bde8f-203">Pour définir des en-têtes HTTP, utilisez la propriété `Headers` sur l’objet de connexion.</span><span class="sxs-lookup"><span data-stu-id="bde8f-203">To set HTTP headers, use the `Headers` property on the connection object.</span></span> <span data-ttu-id="bde8f-204">L’exemple suivant montre comment ajouter un en-tête HTTP.</span><span class="sxs-lookup"><span data-stu-id="bde8f-204">The following example shows how to add an HTTP header.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a><span data-ttu-id="bde8f-205">Comment spécifier des certificats clients</span><span class="sxs-lookup"><span data-stu-id="bde8f-205">How to specify client certificates</span></span>

<span data-ttu-id="bde8f-206">Pour ajouter des certificats clients, utilisez la méthode `AddClientCertificate` sur l’objet de connexion.</span><span class="sxs-lookup"><span data-stu-id="bde8f-206">To add client certificates, use the `AddClientCertificate` method on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a><span data-ttu-id="bde8f-207">Comment créer le proxy Hub</span><span class="sxs-lookup"><span data-stu-id="bde8f-207">How to create the Hub proxy</span></span>

<span data-ttu-id="bde8f-208">Pour définir des méthodes sur le client qu’un hub peut appeler à partir du serveur, et appeler des méthodes sur un concentrateur sur le serveur, créez un proxy pour le concentrateur en appelant `CreateHubProxy` sur l’objet de connexion.</span><span class="sxs-lookup"><span data-stu-id="bde8f-208">In order to define methods on the client that a Hub can call from the server, and to invoke methods on a Hub at the server, create a proxy for the Hub by calling `CreateHubProxy` on the connection object.</span></span> <span data-ttu-id="bde8f-209">La chaîne que vous transmettez à `CreateHubProxy` est le nom de votre classe de concentrateur, ou le nom spécifié par l’attribut `HubName` si celui-ci a été utilisé sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="bde8f-209">The string you pass in to `CreateHubProxy` is the name of your Hub class, or the name specified by the `HubName` attribute if one was used on the server.</span></span> <span data-ttu-id="bde8f-210">La correspondance de noms ne respecte pas la casse.</span><span class="sxs-lookup"><span data-stu-id="bde8f-210">Name matching is case-insensitive.</span></span>

<span data-ttu-id="bde8f-211">**Classe de concentrateur sur le serveur**</span><span class="sxs-lookup"><span data-stu-id="bde8f-211">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

<span data-ttu-id="bde8f-212">**Créer un proxy client pour la classe de concentrateur**</span><span class="sxs-lookup"><span data-stu-id="bde8f-212">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample11.cs?highlight=3)]

<span data-ttu-id="bde8f-213">Si vous décorez votre classe de concentrateur avec un attribut `HubName`, utilisez ce nom.</span><span class="sxs-lookup"><span data-stu-id="bde8f-213">If you decorate your Hub class with a `HubName` attribute, use that name.</span></span>

<span data-ttu-id="bde8f-214">**Classe de concentrateur sur le serveur**</span><span class="sxs-lookup"><span data-stu-id="bde8f-214">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample12.cs)]

<span data-ttu-id="bde8f-215">**Créer un proxy client pour la classe de concentrateur**</span><span class="sxs-lookup"><span data-stu-id="bde8f-215">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample13.cs?highlight=3)]

<span data-ttu-id="bde8f-216">Si vous appelez `HubConnection.CreateHubProxy` plusieurs fois avec le même `hubName`, vous recevez le même objet de `IHubProxy` mis en cache.</span><span class="sxs-lookup"><span data-stu-id="bde8f-216">If you call `HubConnection.CreateHubProxy` multiple times with the same `hubName`, you get the same cached `IHubProxy` object.</span></span>

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="bde8f-217">Comment définir des méthodes sur le client que le serveur peut appeler</span><span class="sxs-lookup"><span data-stu-id="bde8f-217">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="bde8f-218">Pour définir une méthode que le serveur peut appeler, utilisez la méthode `On` du proxy pour inscrire un gestionnaire d’événements.</span><span class="sxs-lookup"><span data-stu-id="bde8f-218">To define a method that the server can call, use the proxy's `On` method to register an event handler.</span></span>

<span data-ttu-id="bde8f-219">La correspondance de nom de méthode ne respecte pas la casse.</span><span class="sxs-lookup"><span data-stu-id="bde8f-219">Method name matching is case-insensitive.</span></span> <span data-ttu-id="bde8f-220">Par exemple, `Clients.All.UpdateStockPrice` sur le serveur exécute `updateStockPrice`, `updatestockprice`ou `UpdateStockPrice` sur le client.</span><span class="sxs-lookup"><span data-stu-id="bde8f-220">For example, `Clients.All.UpdateStockPrice` on the server will execute `updateStockPrice`, `updatestockprice`, or `UpdateStockPrice` on the client.</span></span>

<span data-ttu-id="bde8f-221">Différentes plateformes clientes ont des exigences différentes quant à la façon dont vous écrivez le code de la méthode pour mettre à jour l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="bde8f-221">Different client platforms have different requirements for how you write method code to update the UI.</span></span> <span data-ttu-id="bde8f-222">Les exemples indiqués concernent les clients WinRT (Windows Store .NET).</span><span class="sxs-lookup"><span data-stu-id="bde8f-222">The examples shown are for WinRT (Windows Store .NET) clients.</span></span> <span data-ttu-id="bde8f-223">Les exemples d’applications WPF, Silverlight et de console sont fournis dans [une section distincte plus loin dans cette rubrique](#wpfsl).</span><span class="sxs-lookup"><span data-stu-id="bde8f-223">WPF, Silverlight, and console application examples are provided in [a separate section later in this topic](#wpfsl).</span></span>

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a><span data-ttu-id="bde8f-224">Méthodes sans paramètres</span><span class="sxs-lookup"><span data-stu-id="bde8f-224">Methods without parameters</span></span>

<span data-ttu-id="bde8f-225">Si la méthode que vous gérez n’a pas de paramètres, utilisez la surcharge non générique de la méthode `On` :</span><span class="sxs-lookup"><span data-stu-id="bde8f-225">If the method you're handling does not have parameters, use the non-generic overload of the `On` method:</span></span>

<span data-ttu-id="bde8f-226">**Code serveur appelant la méthode cliente sans paramètres**</span><span class="sxs-lookup"><span data-stu-id="bde8f-226">**Server code calling client method without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

<span data-ttu-id="bde8f-227">**Code client WinRT pour la méthode appelée à partir du serveur sans paramètres ([voir les exemples WPF et Silverlight plus loin dans cette rubrique](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="bde8f-227">**WinRT Client code for method called from server without parameters ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="bde8f-228">Méthodes avec des paramètres, spécification des types de paramètres</span><span class="sxs-lookup"><span data-stu-id="bde8f-228">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="bde8f-229">Si la méthode que vous gérez possède des paramètres, spécifiez les types des paramètres comme types génériques de la méthode `On`.</span><span class="sxs-lookup"><span data-stu-id="bde8f-229">If the method you're handling has parameters, specify the types of the parameters as the generic types of the `On` method.</span></span> <span data-ttu-id="bde8f-230">Il existe des surcharges génériques de la méthode `On` pour vous permettre de spécifier jusqu’à 8 paramètres (4 sur Windows Phone 7).</span><span class="sxs-lookup"><span data-stu-id="bde8f-230">There are generic overloads of the `On` method to enable you to specify up to 8 parameters (4 on Windows Phone 7).</span></span> <span data-ttu-id="bde8f-231">Dans l’exemple suivant, un paramètre est envoyé à la méthode `UpdateStockPrice`.</span><span class="sxs-lookup"><span data-stu-id="bde8f-231">In the following example, one parameter is sent to the `UpdateStockPrice` method.</span></span>

<span data-ttu-id="bde8f-232">**Code serveur appelant la méthode cliente avec un paramètre**</span><span class="sxs-lookup"><span data-stu-id="bde8f-232">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

<span data-ttu-id="bde8f-233">**Classe stock utilisée pour le paramètre**</span><span class="sxs-lookup"><span data-stu-id="bde8f-233">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample17.cs)]

<span data-ttu-id="bde8f-234">**Code client WinRT pour une méthode appelée à partir du serveur avec un paramètre ([Voir exemples WPF et Silverlight plus loin dans cette rubrique](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="bde8f-234">**WinRT Client code for a method called from server with a parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="bde8f-235">Méthodes avec des paramètres, spécification d’objets dynamiques pour les paramètres</span><span class="sxs-lookup"><span data-stu-id="bde8f-235">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="bde8f-236">Comme alternative à la spécification de paramètres comme types génériques de la méthode `On`, vous pouvez spécifier des paramètres en tant qu’objets dynamiques :</span><span class="sxs-lookup"><span data-stu-id="bde8f-236">As an alternative to specifying parameters as generic types of the `On` method, you can specify parameters as dynamic objects:</span></span>

<span data-ttu-id="bde8f-237">**Code serveur appelant la méthode cliente avec un paramètre**</span><span class="sxs-lookup"><span data-stu-id="bde8f-237">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

<span data-ttu-id="bde8f-238">**Classe stock utilisée pour le paramètre**</span><span class="sxs-lookup"><span data-stu-id="bde8f-238">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample20.cs)]

<span data-ttu-id="bde8f-239">**Code client WinRT pour une méthode appelée à partir du serveur avec un paramètre, à l’aide d’un objet dynamique pour le paramètre ([consultez Exemples WPF et Silverlight plus loin dans cette rubrique](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="bde8f-239">**WinRT Client code for a method called from server with a parameter, using a dynamic object for the parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a><span data-ttu-id="bde8f-240">Comment supprimer un gestionnaire</span><span class="sxs-lookup"><span data-stu-id="bde8f-240">How to remove a handler</span></span>

<span data-ttu-id="bde8f-241">Pour supprimer un gestionnaire, appelez sa méthode `Dispose`.</span><span class="sxs-lookup"><span data-stu-id="bde8f-241">To remove a handler, call its `Dispose` method.</span></span>

<span data-ttu-id="bde8f-242">**Code client pour une méthode appelée à partir du serveur**</span><span class="sxs-lookup"><span data-stu-id="bde8f-242">**Client code for a method called from server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

<span data-ttu-id="bde8f-243">**Code client pour supprimer le gestionnaire**</span><span class="sxs-lookup"><span data-stu-id="bde8f-243">**Client code to remove the handler**</span></span>

[!code-css[Main](hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="bde8f-244">Comment appeler des méthodes de serveur à partir du client</span><span class="sxs-lookup"><span data-stu-id="bde8f-244">How to call server methods from the client</span></span>

<span data-ttu-id="bde8f-245">Pour appeler une méthode sur le serveur, utilisez la méthode `Invoke` sur le proxy Hub.</span><span class="sxs-lookup"><span data-stu-id="bde8f-245">To call a method on the server, use the `Invoke` method on the Hub proxy.</span></span>

<span data-ttu-id="bde8f-246">Si la méthode serveur n’a pas de valeur de retour, utilisez la surcharge non générique de la méthode `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="bde8f-246">If the server method has no return value, use the non-generic overload of the `Invoke` method.</span></span>

<span data-ttu-id="bde8f-247">**Code serveur pour une méthode qui n’a pas de valeur de retour**</span><span class="sxs-lookup"><span data-stu-id="bde8f-247">**Server code for a method that has no return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

<span data-ttu-id="bde8f-248">**Code client appelant une méthode qui n’a pas de valeur de retour**</span><span class="sxs-lookup"><span data-stu-id="bde8f-248">**Client code calling a method that has no return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="bde8f-249">Si la méthode serveur a une valeur de retour, spécifiez le type de retour comme type générique de la méthode `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="bde8f-249">If the server method has a return value, specify the return type as the generic type of the `Invoke` method.</span></span>

<span data-ttu-id="bde8f-250">**Code serveur pour une méthode qui a une valeur de retour et prend un paramètre de type complexe**</span><span class="sxs-lookup"><span data-stu-id="bde8f-250">**Server code for a method that has a return value and takes a complex type parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="bde8f-251">**Classe stock utilisée pour le paramètre et la valeur de retour**</span><span class="sxs-lookup"><span data-stu-id="bde8f-251">**The Stock class used for the parameter and return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample27.cs)]

<span data-ttu-id="bde8f-252">**Code client appelant une méthode qui a une valeur de retour et accepte un paramètre de type complexe, dans une méthode Async ASP.NET 4,5**</span><span class="sxs-lookup"><span data-stu-id="bde8f-252">**Client code calling a method that has a return value and takes a complex type parameter, in an ASP.NET 4.5 async method**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

<span data-ttu-id="bde8f-253">**Code client appelant une méthode qui a une valeur de retour et prend un paramètre de type complexe, dans une méthode synchrone**</span><span class="sxs-lookup"><span data-stu-id="bde8f-253">**Client code calling a method that has a return value and takes a complex type parameter, in a synchronous method**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

<span data-ttu-id="bde8f-254">La méthode `Invoke` s’exécute de façon asynchrone et retourne un objet `Task`.</span><span class="sxs-lookup"><span data-stu-id="bde8f-254">The `Invoke` method executes asynchronously and returns a `Task` object.</span></span> <span data-ttu-id="bde8f-255">Si vous ne spécifiez pas `await` ou `.Wait()`, la ligne de code suivante s’exécutera avant la fin de l’exécution de la méthode que vous appelez.</span><span class="sxs-lookup"><span data-stu-id="bde8f-255">If you don't specify `await` or `.Wait()`, the next line of code will execute before the method that you invoke has finished executing.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="bde8f-256">Comment gérer les événements de durée de vie de connexion</span><span class="sxs-lookup"><span data-stu-id="bde8f-256">How to handle connection lifetime events</span></span>

<span data-ttu-id="bde8f-257">Signalr fournit les événements de durée de vie de connexion suivants que vous pouvez gérer :</span><span class="sxs-lookup"><span data-stu-id="bde8f-257">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="bde8f-258">`Received`: déclenché quand des données sont reçues sur la connexion.</span><span class="sxs-lookup"><span data-stu-id="bde8f-258">`Received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="bde8f-259">Fournit les données reçues.</span><span class="sxs-lookup"><span data-stu-id="bde8f-259">Provides the received data.</span></span>
- <span data-ttu-id="bde8f-260">`ConnectionSlow`: déclenché lorsque le client détecte une connexion lente ou souvent en cours de suppression.</span><span class="sxs-lookup"><span data-stu-id="bde8f-260">`ConnectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="bde8f-261">`Reconnecting`: déclenché lorsque le transport sous-jacent commence à se reconnecter.</span><span class="sxs-lookup"><span data-stu-id="bde8f-261">`Reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="bde8f-262">`Reconnected`: déclenché lorsque le transport sous-jacent s’est reconnecté.</span><span class="sxs-lookup"><span data-stu-id="bde8f-262">`Reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="bde8f-263">`StateChanged`: déclenché lorsque l’état de la connexion change.</span><span class="sxs-lookup"><span data-stu-id="bde8f-263">`StateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="bde8f-264">Fournit l’ancien État et le nouvel État.</span><span class="sxs-lookup"><span data-stu-id="bde8f-264">Provides the old state and the new state.</span></span> <span data-ttu-id="bde8f-265">Pour plus d’informations sur les valeurs d’état de connexion [, consultez énumération ConnectionState](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="bde8f-265">For information about connection state values see [ConnectionState Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span></span>
- <span data-ttu-id="bde8f-266">`Closed`: déclenché lorsque la connexion a été déconnectée.</span><span class="sxs-lookup"><span data-stu-id="bde8f-266">`Closed`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="bde8f-267">Par exemple, si vous souhaitez afficher des messages d’avertissement pour les erreurs qui ne sont pas irrécupérables, mais qui entraînent des problèmes de connexion intermittents, tels que la lenteur ou la suppression fréquente de la connexion, gérez l’événement `ConnectionSlow`.</span><span class="sxs-lookup"><span data-stu-id="bde8f-267">For example, if you want to display warning messages for errors that are not fatal but cause intermittent connection problems, such as slowness or frequent dropping of the connection, handle the `ConnectionSlow` event.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample30.cs)]

<span data-ttu-id="bde8f-268">Pour plus d’informations, consultez [comprendre et gérer les événements de durée de vie de connexion dans signalr](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="bde8f-268">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="bde8f-269">Gestion des erreurs</span><span class="sxs-lookup"><span data-stu-id="bde8f-269">How to handle errors</span></span>

<span data-ttu-id="bde8f-270">Si vous n’activez pas explicitement les messages d’erreur détaillés sur le serveur, l’objet exception renvoyé par Signalr après une erreur contient un minimum d’informations sur l’erreur.</span><span class="sxs-lookup"><span data-stu-id="bde8f-270">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="bde8f-271">Par exemple, si un appel à `newContosoChatMessage` échoue, le message d’erreur dans l’objet d’erreur contient «`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`» l’envoi de messages d’erreur détaillés aux clients en production n’est pas recommandé pour des raisons de sécurité, mais si vous souhaitez activer des messages d’erreur détaillés à des fins de dépannage, utilisez le code suivant sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="bde8f-271">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

<span data-ttu-id="bde8f-272">Pour gérer les erreurs déclenchées par Signalr, vous pouvez ajouter un gestionnaire pour l’événement `Error` sur l’objet de connexion.</span><span class="sxs-lookup"><span data-stu-id="bde8f-272">To handle errors that SignalR raises, you can add a handler for the `Error` event on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample32.cs)]

<span data-ttu-id="bde8f-273">Pour gérer les erreurs des appels de méthode, encapsulez le code dans un bloc try-catch.</span><span class="sxs-lookup"><span data-stu-id="bde8f-273">To handle errors from method invocations, wrap the code in a try-catch block.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="bde8f-274">Comment activer la journalisation côté client</span><span class="sxs-lookup"><span data-stu-id="bde8f-274">How to enable client-side logging</span></span>

<span data-ttu-id="bde8f-275">Pour activer la journalisation côté client, définissez les propriétés `TraceLevel` et `TraceWriter` sur l’objet de connexion.</span><span class="sxs-lookup"><span data-stu-id="bde8f-275">To enable client-side logging, set the `TraceLevel` and `TraceWriter` properties on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample34.cs?highlight=3-4)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a><span data-ttu-id="bde8f-276">Exemples de code d’application de console WPF, Silverlight et console pour les méthodes clientes que le serveur peut appeler</span><span class="sxs-lookup"><span data-stu-id="bde8f-276">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>

<span data-ttu-id="bde8f-277">Les exemples de code présentés précédemment pour la définition des méthodes clientes que le serveur peut appeler s’appliquent aux clients WinRT.</span><span class="sxs-lookup"><span data-stu-id="bde8f-277">The code samples shown earlier for defining client methods that the server can call apply to WinRT clients.</span></span> <span data-ttu-id="bde8f-278">Les exemples suivants montrent le code équivalent pour les clients de l’application console, Silverlight et WPF.</span><span class="sxs-lookup"><span data-stu-id="bde8f-278">The following samples show the equivalent code for WPF, Silverlight, and console application clients.</span></span>

### <a name="methods-without-parameters"></a><span data-ttu-id="bde8f-279">Méthodes sans paramètres</span><span class="sxs-lookup"><span data-stu-id="bde8f-279">Methods without parameters</span></span>

<span data-ttu-id="bde8f-280">**Code client WPF pour la méthode appelée à partir du serveur sans paramètres**</span><span class="sxs-lookup"><span data-stu-id="bde8f-280">**WPF client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="bde8f-281">**Code client Silverlight pour la méthode appelée à partir du serveur sans paramètres**</span><span class="sxs-lookup"><span data-stu-id="bde8f-281">**Silverlight client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

<span data-ttu-id="bde8f-282">**Code client d’application console pour la méthode appelée à partir du serveur sans paramètres**</span><span class="sxs-lookup"><span data-stu-id="bde8f-282">**Console application client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="bde8f-283">Méthodes avec des paramètres, spécification des types de paramètres</span><span class="sxs-lookup"><span data-stu-id="bde8f-283">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="bde8f-284">**Code client WPF pour une méthode appelée à partir du serveur avec un paramètre**</span><span class="sxs-lookup"><span data-stu-id="bde8f-284">**WPF client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

<span data-ttu-id="bde8f-285">**Code client Silverlight pour une méthode appelée à partir du serveur avec un paramètre**</span><span class="sxs-lookup"><span data-stu-id="bde8f-285">**Silverlight client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

<span data-ttu-id="bde8f-286">**Code client de l’application console pour une méthode appelée à partir du serveur avec un paramètre**</span><span class="sxs-lookup"><span data-stu-id="bde8f-286">**Console application client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="bde8f-287">Méthodes avec des paramètres, spécification d’objets dynamiques pour les paramètres</span><span class="sxs-lookup"><span data-stu-id="bde8f-287">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="bde8f-288">**Code client WPF pour une méthode appelée à partir du serveur avec un paramètre, à l’aide d’un objet dynamique pour le paramètre**</span><span class="sxs-lookup"><span data-stu-id="bde8f-288">**WPF client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

<span data-ttu-id="bde8f-289">**Code client Silverlight pour une méthode appelée à partir du serveur avec un paramètre, à l’aide d’un objet dynamique pour le paramètre**</span><span class="sxs-lookup"><span data-stu-id="bde8f-289">**Silverlight client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

<span data-ttu-id="bde8f-290">**Code client d’application console pour une méthode appelée à partir du serveur avec un paramètre, à l’aide d’un objet dynamique pour le paramètre**</span><span class="sxs-lookup"><span data-stu-id="bde8f-290">**Console application client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
