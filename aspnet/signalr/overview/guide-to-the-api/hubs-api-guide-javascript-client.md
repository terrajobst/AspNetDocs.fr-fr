---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
title: Guide de l’API des hubs ASP.NET Signalr-client JavaScript | Microsoft Docs
author: bradygaster
description: Ce document fournit une introduction à l’utilisation de l’API hubs pour Signalr version 2 dans les clients JavaScript, tels que les navigateurs et le Windows Store (WinJS) applicat...
ms.author: bradyg
ms.date: 01/15/2019
ms.assetid: a9fd4dc0-1b96-4443-82ca-932a5b4a8ea4
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 8befe133c3627dac1f7d011959c68e2054d345da
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536658"
---
# <a name="aspnet-signalr-hubs-api-guide---javascript-client"></a><span data-ttu-id="f2e75-103">Guide de l’API Hub Signalr ASP.NET-client JavaScript</span><span class="sxs-lookup"><span data-stu-id="f2e75-103">ASP.NET SignalR Hubs API Guide - JavaScript Client</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="f2e75-104">Ce document fournit une introduction à l’utilisation de l’API hubs pour Signalr version 2 dans les clients JavaScript, tels que les navigateurs et les applications du Windows Store (WinJS).</span><span class="sxs-lookup"><span data-stu-id="f2e75-104">This document provides an introduction to using the Hubs API for SignalR version 2 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
>
> <span data-ttu-id="f2e75-105">L’API Signalr hubs vous permet d’effectuer des appels de procédure distante (RPC) à partir d’un serveur vers des clients connectés et des clients vers le serveur.</span><span class="sxs-lookup"><span data-stu-id="f2e75-105">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="f2e75-106">Dans le code serveur, vous définissez des méthodes qui peuvent être appelées par les clients, et vous appelez des méthodes qui s’exécutent sur le client.</span><span class="sxs-lookup"><span data-stu-id="f2e75-106">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="f2e75-107">Dans le code client, vous définissez des méthodes qui peuvent être appelées à partir du serveur, et vous appelez des méthodes qui s’exécutent sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="f2e75-107">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="f2e75-108">Signalr prend en charge tous les éléments de la plombation client à serveur pour vous.</span><span class="sxs-lookup"><span data-stu-id="f2e75-108">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
>
> <span data-ttu-id="f2e75-109">Signalr offre également une API de bas niveau appelée connexions persistantes.</span><span class="sxs-lookup"><span data-stu-id="f2e75-109">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="f2e75-110">Pour une présentation de Signalr, de hubs et de connexions persistantes, consultez [Présentation de signalr](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="f2e75-110">For an introduction to SignalR, Hubs, and Persistent Connections, see [Introduction to SignalR](../getting-started/introduction-to-signalr.md).</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="f2e75-111">Versions logicielles utilisées dans cette rubrique</span><span class="sxs-lookup"><span data-stu-id="f2e75-111">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="f2e75-112">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="f2e75-112">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/)
> - <span data-ttu-id="f2e75-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="f2e75-113">.NET 4.5</span></span>
> - <span data-ttu-id="f2e75-114">Signalr version 2</span><span class="sxs-lookup"><span data-stu-id="f2e75-114">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="f2e75-115">Versions précédentes de cette rubrique</span><span class="sxs-lookup"><span data-stu-id="f2e75-115">Previous versions of this topic</span></span>
>
> <span data-ttu-id="f2e75-116">Pour plus d’informations sur les versions antérieures de Signalr, consultez [versions antérieures de signalr](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="f2e75-116">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="f2e75-117">Questions et commentaires</span><span class="sxs-lookup"><span data-stu-id="f2e75-117">Questions and comments</span></span>
>
> <span data-ttu-id="f2e75-118">N’hésitez pas à nous faire part de vos commentaires sur la façon dont vous aimez ce didacticiel et sur ce que nous pourrions améliorer dans les commentaires en bas de la page.</span><span class="sxs-lookup"><span data-stu-id="f2e75-118">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="f2e75-119">Si vous avez des questions qui ne sont pas directement liées au didacticiel, vous pouvez les poster sur le [forum ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="f2e75-119">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="f2e75-120">Présentation</span><span class="sxs-lookup"><span data-stu-id="f2e75-120">Overview</span></span>

<span data-ttu-id="f2e75-121">Ce document contient les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="f2e75-121">This document contains the following sections:</span></span>

- [<span data-ttu-id="f2e75-122">Le proxy généré et ce qu’il fait pour vous</span><span class="sxs-lookup"><span data-stu-id="f2e75-122">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="f2e75-123">Quand utiliser le proxy généré</span><span class="sxs-lookup"><span data-stu-id="f2e75-123">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="f2e75-124">Installation du client</span><span class="sxs-lookup"><span data-stu-id="f2e75-124">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="f2e75-125">Comment référencer le proxy généré dynamiquement</span><span class="sxs-lookup"><span data-stu-id="f2e75-125">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="f2e75-126">Comment créer un fichier physique pour le proxy généré par Signalr</span><span class="sxs-lookup"><span data-stu-id="f2e75-126">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="f2e75-127">Comment établir une connexion</span><span class="sxs-lookup"><span data-stu-id="f2e75-127">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="f2e75-128">$. Connection. Hub est le même objet que $. hubConnection () crée</span><span class="sxs-lookup"><span data-stu-id="f2e75-128">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="f2e75-129">Exécution asynchrone de la méthode Start</span><span class="sxs-lookup"><span data-stu-id="f2e75-129">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="f2e75-130">Comment établir une connexion inter-domaines</span><span class="sxs-lookup"><span data-stu-id="f2e75-130">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="f2e75-131">Comment configurer la connexion</span><span class="sxs-lookup"><span data-stu-id="f2e75-131">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="f2e75-132">Comment spécifier des paramètres de chaîne de requête</span><span class="sxs-lookup"><span data-stu-id="f2e75-132">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="f2e75-133">Comment spécifier la méthode de transport</span><span class="sxs-lookup"><span data-stu-id="f2e75-133">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="f2e75-134">Obtention d’un proxy pour une classe de concentrateur</span><span class="sxs-lookup"><span data-stu-id="f2e75-134">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="f2e75-135">Comment définir des méthodes sur le client que le serveur peut appeler</span><span class="sxs-lookup"><span data-stu-id="f2e75-135">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="f2e75-136">Comment appeler des méthodes de serveur à partir du client</span><span class="sxs-lookup"><span data-stu-id="f2e75-136">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="f2e75-137">Comment gérer les événements de durée de vie de connexion</span><span class="sxs-lookup"><span data-stu-id="f2e75-137">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="f2e75-138">Gestion des erreurs</span><span class="sxs-lookup"><span data-stu-id="f2e75-138">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="f2e75-139">Comment activer la journalisation côté client</span><span class="sxs-lookup"><span data-stu-id="f2e75-139">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="f2e75-140">Pour obtenir de la documentation sur la programmation du serveur ou des clients .NET, consultez les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="f2e75-140">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="f2e75-141">Guide de l’API signalr hubs-serveur</span><span class="sxs-lookup"><span data-stu-id="f2e75-141">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="f2e75-142">Guide de l’API signalr hubs-client .NET</span><span class="sxs-lookup"><span data-stu-id="f2e75-142">SignalR Hubs API Guide - .NET Client</span></span>](hubs-api-guide-net-client.md)

<span data-ttu-id="f2e75-143">Le composant serveur Signalr 2 est disponible uniquement sur .NET 4,5 (bien qu’il existe un client .NET pour Signalr 2 sur .NET 4,0).</span><span class="sxs-lookup"><span data-stu-id="f2e75-143">The SignalR 2 server component is only available on .NET 4.5 (though there is a .NET client for SignalR 2 on .NET 4.0).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="f2e75-144">Le proxy généré et ce qu’il fait pour vous</span><span class="sxs-lookup"><span data-stu-id="f2e75-144">The generated proxy and what it does for you</span></span>

<span data-ttu-id="f2e75-145">Vous pouvez programmer un client JavaScript pour communiquer avec un service Signalr avec ou sans proxy généré par Signalr.</span><span class="sxs-lookup"><span data-stu-id="f2e75-145">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="f2e75-146">Le proxy vous permet de simplifier la syntaxe du code que vous utilisez pour vous connecter, d’écrire les méthodes appelées par le serveur et d’appeler des méthodes sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="f2e75-146">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="f2e75-147">Lorsque vous écrivez du code pour appeler des méthodes de serveur, le proxy généré vous permet d’utiliser une syntaxe qui semble que vous exécutiez une fonction locale : vous pouvez écrire `serverMethod(arg1, arg2)` au lieu de `invoke('serverMethod', arg1, arg2)`.</span><span class="sxs-lookup"><span data-stu-id="f2e75-147">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="f2e75-148">La syntaxe de proxy générée active également une erreur immédiate et intelligible côté client si vous tapez un nom de méthode de serveur de manière invoulue.</span><span class="sxs-lookup"><span data-stu-id="f2e75-148">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="f2e75-149">Et si vous créez manuellement le fichier qui définit les proxies, vous pouvez également obtenir la prise en charge IntelliSense pour écrire du code qui appelle des méthodes de serveur.</span><span class="sxs-lookup"><span data-stu-id="f2e75-149">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="f2e75-150">Supposons, par exemple, que vous disposiez de la classe de concentrateur suivante sur le serveur :</span><span class="sxs-lookup"><span data-stu-id="f2e75-150">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="f2e75-151">Les exemples de code suivants montrent le code JavaScript qui ressemble à l’appel de la méthode `NewContosoChatMessage` sur le serveur et la réception des appels de la méthode `addContosoChatMessageToPage` à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="f2e75-151">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="f2e75-152">**Avec le proxy généré**</span><span class="sxs-lookup"><span data-stu-id="f2e75-152">**With the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="f2e75-153">**Sans le proxy généré**</span><span class="sxs-lookup"><span data-stu-id="f2e75-153">**Without the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="f2e75-154">Quand utiliser le proxy généré</span><span class="sxs-lookup"><span data-stu-id="f2e75-154">When to use the generated proxy</span></span>

<span data-ttu-id="f2e75-155">Si vous souhaitez inscrire plusieurs gestionnaires d’événements pour une méthode cliente appelée par le serveur, vous ne pouvez pas utiliser le proxy généré.</span><span class="sxs-lookup"><span data-stu-id="f2e75-155">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="f2e75-156">Dans le cas contraire, vous pouvez choisir d’utiliser le proxy généré ou non en fonction de vos préférences de codage.</span><span class="sxs-lookup"><span data-stu-id="f2e75-156">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="f2e75-157">Si vous choisissez de ne pas l’utiliser, vous ne devez pas faire référence à l’URL « signalr/hubs » dans un élément `script` dans votre code client.</span><span class="sxs-lookup"><span data-stu-id="f2e75-157">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="f2e75-158">Configuration cliente</span><span class="sxs-lookup"><span data-stu-id="f2e75-158">Client setup</span></span>

<span data-ttu-id="f2e75-159">Un client JavaScript requiert des références à jQuery et au fichier JavaScript du noyau Signalr.</span><span class="sxs-lookup"><span data-stu-id="f2e75-159">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="f2e75-160">La version jQuery doit être 1.6.4 ou les versions majeures ultérieures, telles que 1.7.2, 1.8.2 ou 1.9.1.</span><span class="sxs-lookup"><span data-stu-id="f2e75-160">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="f2e75-161">Si vous décidez d’utiliser le proxy généré, vous avez également besoin d’une référence au fichier JavaScript du proxy généré par Signalr.</span><span class="sxs-lookup"><span data-stu-id="f2e75-161">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="f2e75-162">L’exemple suivant montre à quoi les références peuvent ressembler dans une page HTML qui utilise le proxy généré.</span><span class="sxs-lookup"><span data-stu-id="f2e75-162">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="f2e75-163">Ces références doivent être incluses dans cet ordre : jQuery First, Signalr Core après cela et les proxys de Signalr en dernier.</span><span class="sxs-lookup"><span data-stu-id="f2e75-163">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="f2e75-164">Comment référencer le proxy généré dynamiquement</span><span class="sxs-lookup"><span data-stu-id="f2e75-164">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="f2e75-165">Dans l’exemple précédent, la référence au proxy généré par Signalr consiste à générer dynamiquement du code JavaScript, et non un fichier physique.</span><span class="sxs-lookup"><span data-stu-id="f2e75-165">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="f2e75-166">Signalr crée le code JavaScript pour le proxy à la volée et le transmet au client en réponse à l’URL « /signalr/hubs ».</span><span class="sxs-lookup"><span data-stu-id="f2e75-166">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="f2e75-167">Si vous avez spécifié une autre URL de base pour les connexions Signalr sur le serveur dans votre méthode `MapSignalR`, l’URL du fichier proxy généré dynamiquement est votre URL personnalisée avec « /hubs » ajouté.</span><span class="sxs-lookup"><span data-stu-id="f2e75-167">If you specified a different base URL for SignalR connections on the server in your `MapSignalR` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="f2e75-168">Pour les clients JavaScript Windows 8 (Windows Store), utilisez le fichier de proxy physique au lieu de celui généré dynamiquement.</span><span class="sxs-lookup"><span data-stu-id="f2e75-168">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="f2e75-169">Pour plus d’informations, consultez [comment créer un fichier physique pour le proxy généré par signalr](#manualproxy) plus loin dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="f2e75-169">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>

<span data-ttu-id="f2e75-170">Dans une vue Razor ASP.NET MVC 4 ou 5, utilisez le tilde pour faire référence à la racine de l’application dans votre référence de fichier proxy :</span><span class="sxs-lookup"><span data-stu-id="f2e75-170">In an ASP.NET MVC 4 or 5 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="f2e75-171">Pour plus d’informations sur l’utilisation de Signalr dans MVC 5, consultez [prise en main avec signalr et MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="f2e75-171">For more information about using SignalR in MVC 5, see [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="f2e75-172">Dans une vue Razor ASP.NET MVC 3, utilisez `Url.Content` pour la référence de votre fichier proxy :</span><span class="sxs-lookup"><span data-stu-id="f2e75-172">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="f2e75-173">Dans une application ASP.NET Web Forms, utilisez `ResolveClientUrl` pour la référence de votre fichier proxys ou enregistrez-la via ScriptManager à l’aide d’un chemin d’accès relatif à la racine de l’application (à partir d’un tilde) :</span><span class="sxs-lookup"><span data-stu-id="f2e75-173">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="f2e75-174">En règle générale, utilisez la même méthode pour spécifier l’URL « /signalr/hubs » que vous utilisez pour les fichiers CSS ou JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f2e75-174">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="f2e75-175">Si vous spécifiez une URL sans utiliser de tilde, dans certains scénarios, votre application fonctionnera correctement lorsque vous testerez dans Visual Studio à l’aide de IIS Express mais échouera avec une erreur 404 quand vous effectuerez le déploiement sur IIS complet.</span><span class="sxs-lookup"><span data-stu-id="f2e75-175">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="f2e75-176">Pour plus d’informations, consultez **résolution des références aux ressources au niveau** de la racine dans les [serveurs Web dans Visual Studio pour les projets Web ASP.net](https://msdn.microsoft.com/library/58wxa9w5.aspx) sur le site MSDN.</span><span class="sxs-lookup"><span data-stu-id="f2e75-176">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="f2e75-177">Quand vous exécutez un projet Web dans Visual Studio 2017 en mode débogage et que vous utilisez Internet Explorer comme navigateur, vous pouvez voir le fichier proxy dans **Explorateur de solutions** sous **scripts**.</span><span class="sxs-lookup"><span data-stu-id="f2e75-177">When you run a web project in Visual Studio 2017 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Scripts**.</span></span>

<span data-ttu-id="f2e75-178">Pour afficher le contenu du fichier, double-cliquez sur **hubs**.</span><span class="sxs-lookup"><span data-stu-id="f2e75-178">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="f2e75-179">Si vous n’utilisez pas Visual Studio 2012 ou 2013 et Internet Explorer, ou si vous n’êtes pas en mode débogage, vous pouvez également récupérer le contenu du fichier en accédant à l’URL « /signalR/hubs ».</span><span class="sxs-lookup"><span data-stu-id="f2e75-179">If you are not using Visual Studio 2012 or 2013 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="f2e75-180">Par exemple, si votre site s’exécute sur `http://localhost:56699`, accédez à `http://localhost:56699/SignalR/hubs` dans votre navigateur.</span><span class="sxs-lookup"><span data-stu-id="f2e75-180">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="f2e75-181">Comment créer un fichier physique pour le proxy généré par Signalr</span><span class="sxs-lookup"><span data-stu-id="f2e75-181">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="f2e75-182">Comme alternative au proxy généré dynamiquement, vous pouvez créer un fichier physique qui contient le code proxy et référencer ce fichier.</span><span class="sxs-lookup"><span data-stu-id="f2e75-182">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="f2e75-183">Vous pouvez le faire pour contrôler le comportement de la mise en cache ou du regroupement, ou pour obtenir IntelliSense quand vous encodez des appels à des méthodes de serveur.</span><span class="sxs-lookup"><span data-stu-id="f2e75-183">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="f2e75-184">Pour créer un fichier proxy, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="f2e75-184">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="f2e75-185">Installez le package NuGet [Microsoft. Aspnet. signalr. utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) .</span><span class="sxs-lookup"><span data-stu-id="f2e75-185">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="f2e75-186">Ouvrez une invite de commandes et accédez au dossier *Tools* qui contient le fichier signalr. exe.</span><span class="sxs-lookup"><span data-stu-id="f2e75-186">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="f2e75-187">Le dossier Tools se trouve à l’emplacement suivant :</span><span class="sxs-lookup"><span data-stu-id="f2e75-187">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. <span data-ttu-id="f2e75-188">Entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="f2e75-188">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="f2e75-189">Le chemin d’accès à votre *fichier. dll* est généralement le dossier *bin* dans le dossier de votre projet.</span><span class="sxs-lookup"><span data-stu-id="f2e75-189">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="f2e75-190">Cette commande crée un fichier nommé *Server. js* dans le même dossier que *signalr. exe*.</span><span class="sxs-lookup"><span data-stu-id="f2e75-190">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="f2e75-191">Placez le fichier *Server. js* dans un dossier approprié de votre projet, renommez-le en fonction de votre application et ajoutez une référence à celui-ci à la place de la référence « signalr/hubs ».</span><span class="sxs-lookup"><span data-stu-id="f2e75-191">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="f2e75-192">Comment établir une connexion</span><span class="sxs-lookup"><span data-stu-id="f2e75-192">How to establish a connection</span></span>

<span data-ttu-id="f2e75-193">Avant de pouvoir établir une connexion, vous devez créer un objet de connexion, créer un proxy et inscrire des gestionnaires d’événements pour les méthodes qui peuvent être appelées à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="f2e75-193">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="f2e75-194">Lorsque le proxy et les gestionnaires d’événements sont configurés, établissez la connexion en appelant la méthode `start`.</span><span class="sxs-lookup"><span data-stu-id="f2e75-194">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="f2e75-195">Si vous utilisez le proxy généré, vous n’êtes pas obligé de créer l’objet de connexion dans votre propre code, car le code proxy généré le fait pour vous.</span><span class="sxs-lookup"><span data-stu-id="f2e75-195">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="f2e75-196">**Établir une connexion (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="f2e75-196">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="f2e75-197">**Établir une connexion (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="f2e75-197">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="f2e75-198">L’exemple de code utilise l’URL « /signalr » par défaut pour se connecter à votre service Signalr.</span><span class="sxs-lookup"><span data-stu-id="f2e75-198">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="f2e75-199">Pour plus d’informations sur la façon de spécifier une autre URL de base, consultez [Guide de l’API ASP.net signalr hubs-Server-The/SIGNALR URL](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="f2e75-199">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="f2e75-200">Par défaut, l’emplacement du concentrateur est le serveur actif. Si vous vous connectez à un autre serveur, spécifiez l’URL avant d’appeler la méthode `start`, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="f2e75-200">By default, the hub location is the current server; if you are connecting to a different server, specify the URL before calling the `start` method, as shown in the following example:</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> <span data-ttu-id="f2e75-201">Normalement, vous inscrivez les gestionnaires d’événements avant d’appeler la méthode `start` pour établir la connexion.</span><span class="sxs-lookup"><span data-stu-id="f2e75-201">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="f2e75-202">Si vous souhaitez inscrire certains gestionnaires d’événements après avoir établi la connexion, vous pouvez le faire, mais vous devez inscrire au moins l’un de vos gestionnaires d’événements avant d’appeler la méthode `start`.</span><span class="sxs-lookup"><span data-stu-id="f2e75-202">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="f2e75-203">Cela peut être dû au fait qu’il peut y avoir de nombreux hubs dans une application, mais que vous ne souhaitez pas déclencher l’événement `OnConnected` sur chaque concentrateur si vous n’utilisez que l’un d’entre eux.</span><span class="sxs-lookup"><span data-stu-id="f2e75-203">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="f2e75-204">Lorsque la connexion est établie, la présence d’une méthode cliente sur le proxy d’un concentrateur est ce qui indique à Signalr de déclencher l’événement `OnConnected`.</span><span class="sxs-lookup"><span data-stu-id="f2e75-204">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="f2e75-205">Si vous n’inscrivez aucun gestionnaire d’événements avant d’appeler la méthode `start`, vous serez en mesure d’appeler des méthodes sur le concentrateur, mais la méthode `OnConnected` du concentrateur ne sera pas appelée et aucune méthode client ne sera appelée à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="f2e75-205">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>

<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="f2e75-206">$. Connection. Hub est le même objet que $. hubConnection () crée</span><span class="sxs-lookup"><span data-stu-id="f2e75-206">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="f2e75-207">Comme vous pouvez le voir dans les exemples, lorsque vous utilisez le proxy généré, `$.connection.hub` fait référence à l’objet de connexion.</span><span class="sxs-lookup"><span data-stu-id="f2e75-207">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="f2e75-208">Il s’agit du même objet que celui obtenu en appelant `$.hubConnection()` lorsque vous n’utilisez pas le proxy généré.</span><span class="sxs-lookup"><span data-stu-id="f2e75-208">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="f2e75-209">Le code proxy généré crée la connexion pour vous en exécutant l’instruction suivante :</span><span class="sxs-lookup"><span data-stu-id="f2e75-209">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![Création d’une connexion dans le fichier proxy généré](hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="f2e75-211">Lorsque vous utilisez le proxy généré, vous pouvez faire quoi que ce soit avec `$.connection.hub` que vous pouvez faire avec un objet de connexion lorsque vous n’utilisez pas le proxy généré.</span><span class="sxs-lookup"><span data-stu-id="f2e75-211">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="f2e75-212">Exécution asynchrone de la méthode Start</span><span class="sxs-lookup"><span data-stu-id="f2e75-212">Asynchronous execution of the start method</span></span>

<span data-ttu-id="f2e75-213">La méthode `start` s’exécute de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="f2e75-213">The `start` method executes asynchronously.</span></span> <span data-ttu-id="f2e75-214">Elle retourne un [objet différé jQuery](http://api.jquery.com/category/deferred-object/), ce qui signifie que vous pouvez ajouter des fonctions de rappel en appelant des méthodes telles que `pipe`, `done`et `fail`.</span><span class="sxs-lookup"><span data-stu-id="f2e75-214">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="f2e75-215">Si vous avez du code que vous souhaitez exécuter après l’établissement de la connexion, par exemple un appel à une méthode de serveur, placez ce code dans une fonction de rappel ou appelez-le à partir d’une fonction de rappel.</span><span class="sxs-lookup"><span data-stu-id="f2e75-215">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="f2e75-216">La méthode de rappel `.done` est exécutée une fois que la connexion a été établie, et après la fin de l’exécution du code que vous avez dans votre méthode de gestionnaire d’événements `OnConnected` sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="f2e75-216">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="f2e75-217">Si vous placez l’instruction « Now Connected » de l’exemple précédent comme ligne de code suivante après l’appel de la méthode `start` (et non dans un rappel `.done`), la ligne `console.log` s’exécutera avant l’établissement de la connexion, comme illustré dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="f2e75-217">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![Méthode incorrecte pour écrire du code qui s’exécute après l’établissement de la connexion](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="f2e75-219">Comment établir une connexion inter-domaines</span><span class="sxs-lookup"><span data-stu-id="f2e75-219">How to establish a cross-domain connection</span></span>

<span data-ttu-id="f2e75-220">En général, si le navigateur charge une page à partir de `http://contoso.com`, la connexion Signalr se trouve dans le même domaine, à `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="f2e75-220">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="f2e75-221">Si la page de `http://contoso.com` établit une connexion à `http://fabrikam.com/signalr`, il s’agit d’une connexion inter-domaines.</span><span class="sxs-lookup"><span data-stu-id="f2e75-221">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="f2e75-222">Pour des raisons de sécurité, les connexions inter-domaines sont désactivées par défaut.</span><span class="sxs-lookup"><span data-stu-id="f2e75-222">For security reasons, cross-domain connections are disabled by default.</span></span>

<span data-ttu-id="f2e75-223">Dans Signalr 1. x, les demandes inter-domaines étaient contrôlées par un seul indicateur EnableCrossDomain.</span><span class="sxs-lookup"><span data-stu-id="f2e75-223">In SignalR 1.x, cross domain requests were controlled by a single EnableCrossDomain flag.</span></span> <span data-ttu-id="f2e75-224">Cet indicateur contrôlait à la fois les demandes JSONP et CORS.</span><span class="sxs-lookup"><span data-stu-id="f2e75-224">This flag controlled both JSONP and CORS requests.</span></span> <span data-ttu-id="f2e75-225">Pour une plus grande flexibilité, toute la prise en charge de CORS a été supprimée du composant serveur de Signalr (les clients JavaScript continuent d’utiliser CORS normalement s’il est détecté que le navigateur le prend en charge) et le nouveau middleware OWIN a été mis à disposition pour prendre en charge ces scénarios.</span><span class="sxs-lookup"><span data-stu-id="f2e75-225">For greater flexibility, all CORS support has been removed from the server component of SignalR (JavaScript clients still use CORS normally if it is detected that the browser supports it), and new OWIN middleware has been made available to support these scenarios.</span></span>

<span data-ttu-id="f2e75-226">Si JSONP est requis sur le client (pour prendre en charge les requêtes inter-domaines dans les navigateurs plus anciens), vous devez l’activer explicitement en définissant `EnableJSONP` sur l’objet `HubConfiguration` à `true`, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="f2e75-226">If JSONP is required on the client (to support cross-domain requests in older browsers), it will need to be enabled explicitly by setting `EnableJSONP` on the `HubConfiguration` object to `true`, as shown below.</span></span> <span data-ttu-id="f2e75-227">JSONP est désactivé par défaut, car il est moins sécurisé que CORS.</span><span class="sxs-lookup"><span data-stu-id="f2e75-227">JSONP is disabled by default, as it is less secure than CORS.</span></span>

<span data-ttu-id="f2e75-228">**Ajout de Microsoft. Owin. cors à votre projet :** Pour installer cette bibliothèque, exécutez la commande suivante dans la console du gestionnaire de package :</span><span class="sxs-lookup"><span data-stu-id="f2e75-228">**Adding Microsoft.Owin.Cors to your project:** To install this library, run the following command in the Package Manager Console:</span></span>

`Install-Package Microsoft.Owin.Cors`

<span data-ttu-id="f2e75-229">Cette commande ajoute la version 2.1.0 du package à votre projet.</span><span class="sxs-lookup"><span data-stu-id="f2e75-229">This command will add the 2.1.0 version of the package to your project.</span></span>

### <a name="calling-usecors"></a><span data-ttu-id="f2e75-230">Appel de UseCors</span><span class="sxs-lookup"><span data-stu-id="f2e75-230">Calling UseCors</span></span>

 <span data-ttu-id="f2e75-231">L’extrait de code suivant montre comment implémenter des connexions inter-domaines dans Signalr 2.</span><span class="sxs-lookup"><span data-stu-id="f2e75-231">The following code snippet demonstrates how to implement cross-domain connections in SignalR 2.</span></span>

<span data-ttu-id="f2e75-232">**Implémentation de requêtes inter-domaines dans Signalr 2**</span><span class="sxs-lookup"><span data-stu-id="f2e75-232">**Implementing cross-domain requests in SignalR 2**</span></span>

<span data-ttu-id="f2e75-233">Le code suivant montre comment activer CORS ou JSONP dans un projet Signalr 2.</span><span class="sxs-lookup"><span data-stu-id="f2e75-233">The following code demonstrates how to enable CORS or JSONP in a SignalR 2 project.</span></span> <span data-ttu-id="f2e75-234">Cet exemple de code utilise `Map` et `RunSignalR` au lieu de `MapSignalR`, de sorte que l’intergiciel (middleware) CORS s’exécute uniquement pour les demandes Signalr qui requièrent la prise en charge de CORS (plutôt que pour tout le trafic au niveau du chemin spécifié dans `MapSignalR`). La carte peut également être utilisée pour tout autre intergiciel qui doit s’exécuter pour un préfixe d’URL spécifique, plutôt que pour l’application entière.</span><span class="sxs-lookup"><span data-stu-id="f2e75-234">This code sample uses `Map` and `RunSignalR` instead of `MapSignalR`, so that the CORS middleware runs only for the SignalR requests that require CORS support (rather than for all traffic at the path specified in `MapSignalR`.) Map can also be used for any other middleware that needs to run for a specific URL prefix, rather than for the entire application.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE]
>
> - <span data-ttu-id="f2e75-235">N’affectez pas la valeur true à `jQuery.support.cors` dans votre code.</span><span class="sxs-lookup"><span data-stu-id="f2e75-235">Don't set `jQuery.support.cors` to true in your code.</span></span>
>
>     ![N’affectez pas la valeur true à jQuery. support. cors](hubs-api-guide-javascript-client/_static/image7.png)
>
>     <span data-ttu-id="f2e75-237">Signalr gère l’utilisation de CORS.</span><span class="sxs-lookup"><span data-stu-id="f2e75-237">SignalR handles the use of CORS.</span></span> <span data-ttu-id="f2e75-238">L’affectation de la valeur true à `jQuery.support.cors` désactive JSONP car Signalr peut supposer que le navigateur prend en charge CORS.</span><span class="sxs-lookup"><span data-stu-id="f2e75-238">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="f2e75-239">Lorsque vous vous connectez à une URL localhost, Internet Explorer 10 ne considère pas qu’il s’agit d’une connexion inter-domaines. l’application fonctionnera donc localement avec IE 10, même si vous n’avez pas activé les connexions inter-domaines sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="f2e75-239">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="f2e75-240">Pour plus d’informations sur l’utilisation de connexions inter-domaines avec Internet Explorer 9, consultez [ce thread StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span><span class="sxs-lookup"><span data-stu-id="f2e75-240">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="f2e75-241">Pour plus d’informations sur l’utilisation de connexions inter-domaines avec chrome, consultez [ce thread StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span><span class="sxs-lookup"><span data-stu-id="f2e75-241">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="f2e75-242">L’exemple de code utilise l’URL « /signalr » par défaut pour se connecter à votre service Signalr.</span><span class="sxs-lookup"><span data-stu-id="f2e75-242">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="f2e75-243">Pour plus d’informations sur la façon de spécifier une autre URL de base, consultez [Guide de l’API ASP.net signalr hubs-Server-The/SIGNALR URL](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="f2e75-243">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="f2e75-244">Comment configurer la connexion</span><span class="sxs-lookup"><span data-stu-id="f2e75-244">How to configure the connection</span></span>

<span data-ttu-id="f2e75-245">Avant d’établir une connexion, vous pouvez spécifier des paramètres de chaîne de requête ou spécifier la méthode de transport.</span><span class="sxs-lookup"><span data-stu-id="f2e75-245">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="f2e75-246">Comment spécifier des paramètres de chaîne de requête</span><span class="sxs-lookup"><span data-stu-id="f2e75-246">How to specify query string parameters</span></span>

<span data-ttu-id="f2e75-247">Si vous souhaitez envoyer des données au serveur lorsque le client se connecte, vous pouvez ajouter des paramètres de chaîne de requête à l’objet de connexion.</span><span class="sxs-lookup"><span data-stu-id="f2e75-247">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="f2e75-248">Les exemples suivants montrent comment définir un paramètre de chaîne de requête dans le code client.</span><span class="sxs-lookup"><span data-stu-id="f2e75-248">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="f2e75-249">**Définir une valeur de chaîne de requête avant d’appeler la méthode Start (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="f2e75-249">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="f2e75-250">**Définir une valeur de chaîne de requête avant d’appeler la méthode Start (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="f2e75-250">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

<span data-ttu-id="f2e75-251">L’exemple suivant montre comment lire un paramètre de chaîne de requête dans le code serveur.</span><span class="sxs-lookup"><span data-stu-id="f2e75-251">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="f2e75-252">Comment spécifier la méthode de transport</span><span class="sxs-lookup"><span data-stu-id="f2e75-252">How to specify the transport method</span></span>

<span data-ttu-id="f2e75-253">Dans le cadre du processus de connexion, un client Signalr négocie normalement avec le serveur pour déterminer le meilleur transport pris en charge par le serveur et le client.</span><span class="sxs-lookup"><span data-stu-id="f2e75-253">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="f2e75-254">Si vous connaissez déjà le transport que vous souhaitez utiliser, vous pouvez contourner ce processus de négociation en spécifiant la méthode de transport lorsque vous appelez la méthode `start`.</span><span class="sxs-lookup"><span data-stu-id="f2e75-254">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="f2e75-255">**Code client qui spécifie la méthode de transport (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="f2e75-255">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

<span data-ttu-id="f2e75-256">**Code client qui spécifie la méthode de transport (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="f2e75-256">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

<span data-ttu-id="f2e75-257">Vous pouvez également spécifier plusieurs méthodes de transport dans l’ordre dans lequel vous souhaitez que Signalr les teste :</span><span class="sxs-lookup"><span data-stu-id="f2e75-257">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="f2e75-258">**Code client qui spécifie un schéma de secours de transport personnalisé (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="f2e75-258">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="f2e75-259">**Code client qui spécifie un schéma de secours de transport personnalisé (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="f2e75-259">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="f2e75-260">Vous pouvez utiliser les valeurs suivantes pour spécifier la méthode de transport :</span><span class="sxs-lookup"><span data-stu-id="f2e75-260">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="f2e75-261">WebSockets</span><span class="sxs-lookup"><span data-stu-id="f2e75-261">"webSockets"</span></span>
- <span data-ttu-id="f2e75-262">"foreverFrame"</span><span class="sxs-lookup"><span data-stu-id="f2e75-262">"foreverFrame"</span></span>
- <span data-ttu-id="f2e75-263">"serverSentEvents"</span><span class="sxs-lookup"><span data-stu-id="f2e75-263">"serverSentEvents"</span></span>
- <span data-ttu-id="f2e75-264">"longPolling"</span><span class="sxs-lookup"><span data-stu-id="f2e75-264">"longPolling"</span></span>

<span data-ttu-id="f2e75-265">Les exemples suivants montrent comment déterminer quelle méthode de transport est utilisée par une connexion.</span><span class="sxs-lookup"><span data-stu-id="f2e75-265">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="f2e75-266">**Code client qui affiche la méthode de transport utilisée par une connexion (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="f2e75-266">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

<span data-ttu-id="f2e75-267">**Code client qui affiche la méthode de transport utilisée par une connexion (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="f2e75-267">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

<span data-ttu-id="f2e75-268">Pour plus d’informations sur la vérification de la méthode de transport dans le code serveur, consultez Guide de l' [API ASP.net signalr Hub-Server-How pour obtenir des informations sur le client à partir de la propriété de contexte](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="f2e75-268">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="f2e75-269">Pour plus d’informations sur les transports et les secours, consultez [Présentation de signalr-transports et de secours](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="f2e75-269">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="f2e75-270">Obtention d’un proxy pour une classe de concentrateur</span><span class="sxs-lookup"><span data-stu-id="f2e75-270">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="f2e75-271">Chaque objet de connexion que vous créez encapsule des informations sur une connexion à un service Signalr qui contient une ou plusieurs classes de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="f2e75-271">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="f2e75-272">Pour communiquer avec une classe de concentrateur, vous utilisez un objet proxy que vous créez vous-même (si vous n’utilisez pas le proxy généré) ou qui est généré pour vous.</span><span class="sxs-lookup"><span data-stu-id="f2e75-272">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="f2e75-273">Sur le client, le nom du proxy est une version à casse mixte du nom de la classe de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="f2e75-273">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="f2e75-274">Signalr effectue automatiquement cette modification afin que le code JavaScript puisse être conforme aux conventions JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f2e75-274">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="f2e75-275">**Classe de concentrateur sur le serveur**</span><span class="sxs-lookup"><span data-stu-id="f2e75-275">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

<span data-ttu-id="f2e75-276">**Obtenir une référence au proxy client généré pour le Hub**</span><span class="sxs-lookup"><span data-stu-id="f2e75-276">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

<span data-ttu-id="f2e75-277">**Créer un proxy client pour la classe de concentrateur (sans proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="f2e75-277">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="f2e75-278">Si vous décorez votre classe de concentrateur avec un attribut `HubName`, utilisez le nom exact sans changer la casse.</span><span class="sxs-lookup"><span data-stu-id="f2e75-278">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="f2e75-279">**Classe de concentrateur sur le serveur avec l’attribut HubName**</span><span class="sxs-lookup"><span data-stu-id="f2e75-279">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

<span data-ttu-id="f2e75-280">**Obtenir une référence au proxy client généré pour le Hub**</span><span class="sxs-lookup"><span data-stu-id="f2e75-280">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

<span data-ttu-id="f2e75-281">**Créer un proxy client pour la classe de concentrateur (sans proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="f2e75-281">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="f2e75-282">Comment définir des méthodes sur le client que le serveur peut appeler</span><span class="sxs-lookup"><span data-stu-id="f2e75-282">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="f2e75-283">Pour définir une méthode que le serveur peut appeler à partir d’un concentrateur, ajoutez un gestionnaire d’événements au proxy de concentrateur à l’aide de la propriété `client` du proxy généré, ou appelez la méthode `on` si vous n’utilisez pas le proxy généré.</span><span class="sxs-lookup"><span data-stu-id="f2e75-283">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="f2e75-284">Les paramètres peuvent être des objets complexes.</span><span class="sxs-lookup"><span data-stu-id="f2e75-284">The parameters can be complex objects.</span></span>

<span data-ttu-id="f2e75-285">Ajoutez le gestionnaire d’événements avant d’appeler la méthode `start` pour établir la connexion.</span><span class="sxs-lookup"><span data-stu-id="f2e75-285">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="f2e75-286">(Si vous souhaitez ajouter des gestionnaires d’événements après avoir appelé la méthode `start`, consultez la remarque dans [Comment établir une connexion](#establishconnection) plus haut dans ce document et utilisez la syntaxe indiquée pour définir une méthode sans utiliser le proxy généré.)</span><span class="sxs-lookup"><span data-stu-id="f2e75-286">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="f2e75-287">La correspondance de nom de méthode ne respecte pas la casse.</span><span class="sxs-lookup"><span data-stu-id="f2e75-287">Method name matching is case-insensitive.</span></span> <span data-ttu-id="f2e75-288">Par exemple, `Clients.All.addContosoChatMessageToPage` sur le serveur exécute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`ou `addcontosochatmessagetopage` sur le client.</span><span class="sxs-lookup"><span data-stu-id="f2e75-288">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="f2e75-289">**Définir la méthode sur le client (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="f2e75-289">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

<span data-ttu-id="f2e75-290">**Autre façon de définir la méthode sur le client (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="f2e75-290">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

<span data-ttu-id="f2e75-291">**Définir la méthode sur le client (sans le proxy généré, ou lors de l’ajout après l’appel de la méthode Start)**</span><span class="sxs-lookup"><span data-stu-id="f2e75-291">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

<span data-ttu-id="f2e75-292">**Code serveur qui appelle la méthode cliente**</span><span class="sxs-lookup"><span data-stu-id="f2e75-292">**Server code that calls the client method**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

<span data-ttu-id="f2e75-293">Les exemples suivants incluent un objet complexe en tant que paramètre de méthode.</span><span class="sxs-lookup"><span data-stu-id="f2e75-293">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="f2e75-294">**Définir la méthode sur le client qui prend un objet complexe (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="f2e75-294">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

<span data-ttu-id="f2e75-295">**Définir la méthode sur le client qui prend un objet complexe (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="f2e75-295">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

<span data-ttu-id="f2e75-296">**Code serveur qui définit l’objet complexe**</span><span class="sxs-lookup"><span data-stu-id="f2e75-296">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

<span data-ttu-id="f2e75-297">**Code serveur qui appelle la méthode cliente à l’aide d’un objet complexe**</span><span class="sxs-lookup"><span data-stu-id="f2e75-297">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="f2e75-298">Comment appeler des méthodes de serveur à partir du client</span><span class="sxs-lookup"><span data-stu-id="f2e75-298">How to call server methods from the client</span></span>

<span data-ttu-id="f2e75-299">Pour appeler une méthode de serveur à partir du client, utilisez la propriété `server` du proxy généré ou la méthode `invoke` sur le proxy Hub si vous n’utilisez pas le proxy généré.</span><span class="sxs-lookup"><span data-stu-id="f2e75-299">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="f2e75-300">Les paramètres ou la valeur de retour peuvent être des objets complexes.</span><span class="sxs-lookup"><span data-stu-id="f2e75-300">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="f2e75-301">Transmettez une version à casse mixte du nom de la méthode sur le concentrateur.</span><span class="sxs-lookup"><span data-stu-id="f2e75-301">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="f2e75-302">Signalr effectue automatiquement cette modification afin que le code JavaScript puisse être conforme aux conventions JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f2e75-302">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="f2e75-303">Les exemples suivants montrent comment appeler une méthode de serveur qui n’a pas de valeur de retour et comment appeler une méthode de serveur qui a une valeur de retour.</span><span class="sxs-lookup"><span data-stu-id="f2e75-303">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="f2e75-304">**Méthode serveur sans attribut HubMethodName**</span><span class="sxs-lookup"><span data-stu-id="f2e75-304">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

<span data-ttu-id="f2e75-305">**Code serveur qui définit l’objet complexe passé dans un paramètre**</span><span class="sxs-lookup"><span data-stu-id="f2e75-305">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

<span data-ttu-id="f2e75-306">**Code client qui appelle la méthode serveur (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="f2e75-306">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

<span data-ttu-id="f2e75-307">**Code client qui appelle la méthode serveur (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="f2e75-307">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

<span data-ttu-id="f2e75-308">Si vous avez décoré la méthode de concentrateur avec un attribut `HubMethodName`, utilisez ce nom sans modifier la casse.</span><span class="sxs-lookup"><span data-stu-id="f2e75-308">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="f2e75-309">**Méthode serveur** avec un attribut HubMethodName</span><span class="sxs-lookup"><span data-stu-id="f2e75-309">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

<span data-ttu-id="f2e75-310">**Code client qui appelle la méthode serveur (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="f2e75-310">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="f2e75-311">**Code client qui appelle la méthode serveur (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="f2e75-311">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

<span data-ttu-id="f2e75-312">Les exemples précédents montrent comment appeler une méthode de serveur qui n’a pas de valeur de retour.</span><span class="sxs-lookup"><span data-stu-id="f2e75-312">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="f2e75-313">Les exemples suivants montrent comment appeler une méthode de serveur qui a une valeur de retour.</span><span class="sxs-lookup"><span data-stu-id="f2e75-313">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="f2e75-314">**Code serveur pour une méthode qui a une valeur de retour**</span><span class="sxs-lookup"><span data-stu-id="f2e75-314">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

<span data-ttu-id="f2e75-315">**Classe stock utilisée pour la valeur de** retour</span><span class="sxs-lookup"><span data-stu-id="f2e75-315">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

<span data-ttu-id="f2e75-316">**Code client qui appelle la méthode serveur (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="f2e75-316">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

<span data-ttu-id="f2e75-317">**Code client qui appelle la méthode serveur (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="f2e75-317">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="f2e75-318">Comment gérer les événements de durée de vie de connexion</span><span class="sxs-lookup"><span data-stu-id="f2e75-318">How to handle connection lifetime events</span></span>

<span data-ttu-id="f2e75-319">Signalr fournit les événements de durée de vie de connexion suivants que vous pouvez gérer :</span><span class="sxs-lookup"><span data-stu-id="f2e75-319">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="f2e75-320">`starting`: déclenché avant l’envoi de données sur la connexion.</span><span class="sxs-lookup"><span data-stu-id="f2e75-320">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="f2e75-321">`received`: déclenché quand des données sont reçues sur la connexion.</span><span class="sxs-lookup"><span data-stu-id="f2e75-321">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="f2e75-322">Fournit les données reçues.</span><span class="sxs-lookup"><span data-stu-id="f2e75-322">Provides the received data.</span></span>
- <span data-ttu-id="f2e75-323">`connectionSlow`: déclenché lorsque le client détecte une connexion lente ou souvent en cours de suppression.</span><span class="sxs-lookup"><span data-stu-id="f2e75-323">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="f2e75-324">`reconnecting`: déclenché lorsque le transport sous-jacent commence à se reconnecter.</span><span class="sxs-lookup"><span data-stu-id="f2e75-324">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="f2e75-325">`reconnected`: déclenché lorsque le transport sous-jacent s’est reconnecté.</span><span class="sxs-lookup"><span data-stu-id="f2e75-325">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="f2e75-326">`stateChanged`: déclenché lorsque l’état de la connexion change.</span><span class="sxs-lookup"><span data-stu-id="f2e75-326">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="f2e75-327">Fournit l’ancien État et le nouvel État (connexion, connexion, reconnexion ou déconnexion).</span><span class="sxs-lookup"><span data-stu-id="f2e75-327">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="f2e75-328">`disconnected`: déclenché lorsque la connexion a été déconnectée.</span><span class="sxs-lookup"><span data-stu-id="f2e75-328">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="f2e75-329">Par exemple, si vous souhaitez afficher des messages d’avertissement lorsque des problèmes de connexion peuvent entraîner des retards perceptibles, gérez l’événement `connectionSlow`.</span><span class="sxs-lookup"><span data-stu-id="f2e75-329">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="f2e75-330">**Gérer l’événement connectionSlow (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="f2e75-330">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

<span data-ttu-id="f2e75-331">**Gérer l’événement connectionSlow (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="f2e75-331">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

<span data-ttu-id="f2e75-332">Pour plus d’informations, consultez [comprendre et gérer les événements de durée de vie de connexion dans signalr](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="f2e75-332">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="f2e75-333">Gestion des erreurs</span><span class="sxs-lookup"><span data-stu-id="f2e75-333">How to handle errors</span></span>

<span data-ttu-id="f2e75-334">Le client JavaScript Signalr fournit un événement `error` auquel vous pouvez ajouter un gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="f2e75-334">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="f2e75-335">Vous pouvez également utiliser la méthode Fail pour ajouter un gestionnaire pour les erreurs qui résultent d’un appel de méthode serveur.</span><span class="sxs-lookup"><span data-stu-id="f2e75-335">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="f2e75-336">Si vous n’activez pas explicitement les messages d’erreur détaillés sur le serveur, l’objet exception renvoyé par Signalr après une erreur contient un minimum d’informations sur l’erreur.</span><span class="sxs-lookup"><span data-stu-id="f2e75-336">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="f2e75-337">Par exemple, si un appel à `newContosoChatMessage` échoue, le message d’erreur dans l’objet d’erreur contient «`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`» l’envoi de messages d’erreur détaillés aux clients en production n’est pas recommandé pour des raisons de sécurité, mais si vous souhaitez activer des messages d’erreur détaillés à des fins de dépannage, utilisez le code suivant sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="f2e75-337">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

<span data-ttu-id="f2e75-338">L’exemple suivant montre comment ajouter un gestionnaire pour l’événement d’erreur.</span><span class="sxs-lookup"><span data-stu-id="f2e75-338">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="f2e75-339">**Ajouter un gestionnaire d’erreurs (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="f2e75-339">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

<span data-ttu-id="f2e75-340">**Ajouter un gestionnaire d’erreurs (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="f2e75-340">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

<span data-ttu-id="f2e75-341">L’exemple suivant montre comment gérer une erreur à partir d’un appel de méthode.</span><span class="sxs-lookup"><span data-stu-id="f2e75-341">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="f2e75-342">**Gérer une erreur à partir d’un appel de méthode (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="f2e75-342">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

<span data-ttu-id="f2e75-343">**Gérer une erreur à partir d’un appel de méthode (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="f2e75-343">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="f2e75-344">Si l’appel d’une méthode échoue, l’événement `error` est également déclenché, de sorte que votre code dans le gestionnaire de méthode `error` et dans le rappel de méthode `.fail` s’exécute.</span><span class="sxs-lookup"><span data-stu-id="f2e75-344">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="f2e75-345">Comment activer la journalisation côté client</span><span class="sxs-lookup"><span data-stu-id="f2e75-345">How to enable client-side logging</span></span>

<span data-ttu-id="f2e75-346">Pour activer la journalisation côté client sur une connexion, définissez la propriété `logging` sur l’objet de connexion avant d’appeler la méthode `start` pour établir la connexion.</span><span class="sxs-lookup"><span data-stu-id="f2e75-346">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="f2e75-347">**Activer la journalisation (avec le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="f2e75-347">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

<span data-ttu-id="f2e75-348">**Activer la journalisation (sans le proxy généré)**</span><span class="sxs-lookup"><span data-stu-id="f2e75-348">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="f2e75-349">Pour afficher les journaux, ouvrez les outils de développement de votre navigateur et accédez à l’onglet Console. Pour obtenir un didacticiel qui contient des instructions pas à pas et des captures d’écran qui montrent comment procéder, consultez diffusion sur le [serveur avec ASP.net signalr-activer la journalisation](../getting-started/tutorial-server-broadcast-with-signalr.md#enable-logging).</span><span class="sxs-lookup"><span data-stu-id="f2e75-349">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](../getting-started/tutorial-server-broadcast-with-signalr.md#enable-logging).</span></span>
