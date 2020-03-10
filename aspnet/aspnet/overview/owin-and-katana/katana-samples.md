---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Exemples Katana | Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 01/17/2014
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: 1238f7d09492a6856d49dece5de75184ccfa4838
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584559"
---
# <a name="katana-samples"></a><span data-ttu-id="ca33b-102">Exemples Katana</span><span class="sxs-lookup"><span data-stu-id="ca33b-102">Katana Samples</span></span>

<span data-ttu-id="ca33b-103">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="ca33b-103">by [Microsoft](https://github.com/microsoft)</span></span>

## <a name="katana-samples"></a><span data-ttu-id="ca33b-104">Exemples Katana</span><span class="sxs-lookup"><span data-stu-id="ca33b-104">Katana Samples</span></span>

<span data-ttu-id="ca33b-105">**Exemple de ASP.net Routes** | [code source](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)</span><span class="sxs-lookup"><span data-stu-id="ca33b-105">**ASP.NET Routes Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)</span></span>  
<span data-ttu-id="ca33b-106">Dans certaines applications, vous souhaiterez raccorder des composants OWIN dans la table d’itinéraires Asp.Net côte à côte avec des composants non OWIN.</span><span class="sxs-lookup"><span data-stu-id="ca33b-106">In some applications you will want to hook up OWIN components in the Asp.Net route table side by side with non-OWIN components.</span></span> <span data-ttu-id="ca33b-107">Cet exemple montre comment utiliser les méthodes d’extension RouteCollection MapOwinPath et MapOwinRoute fournies par Microsoft. Owin. Host. SystemWeb.</span><span class="sxs-lookup"><span data-stu-id="ca33b-107">This sample shows how to use the RouteCollection extension methods MapOwinPath and MapOwinRoute provided by Microsoft.Owin.Host.SystemWeb.</span></span>

<span data-ttu-id="ca33b-108">Exemple de [code source](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines) de **branchement de pipelines** | </span><span class="sxs-lookup"><span data-stu-id="ca33b-108">**Branching Pipelines Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)</span></span>  
<span data-ttu-id="ca33b-109">Les pipelines de traitement des demandes OWIN n’ont pas besoin d’être linéaires, ils peuvent être reliés pour traiter les requêtes de différentes façons.</span><span class="sxs-lookup"><span data-stu-id="ca33b-109">OWIN request processing pipelines do not need to be linear, they can be branched to process requests in different ways.</span></span> <span data-ttu-id="ca33b-110">Cet exemple montre comment construire un pipeline de branchement basé sur des chemins de requête ou d’autres données de requête telles que des en-têtes.</span><span class="sxs-lookup"><span data-stu-id="ca33b-110">This sample shows how to construct a branching pipeline based on request paths or other request data such as headers.</span></span> <span data-ttu-id="ca33b-111">Ces composants sont disponibles dans le package NuGet Microsoft. Owin. Mapping.</span><span class="sxs-lookup"><span data-stu-id="ca33b-111">These components are available in the Microsoft.Owin.Mapping nuget package.</span></span>

<span data-ttu-id="ca33b-112">**Exemple de serveur personnalisé** | [code source](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer) </span><span class="sxs-lookup"><span data-stu-id="ca33b-112">**Custom Server Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer) </span></span>  
<span data-ttu-id="ca33b-113">Montre comment utiliser un serveur OWIN personnalisé lors de l’auto-hébergement de OWIN.</span><span class="sxs-lookup"><span data-stu-id="ca33b-113">Shows how to use a custom OWIN server when self-hosting OWIN.</span></span>

<span data-ttu-id="ca33b-114">Exemple de [Code Source](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded) | **incorporé**</span><span class="sxs-lookup"><span data-stu-id="ca33b-114">**Embedded Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded)</span></span>  
<span data-ttu-id="ca33b-115">Certains serveurs OWIN peuvent être exécutés dans votre propre processus (&quot;&quot;auto-hébergés).</span><span class="sxs-lookup"><span data-stu-id="ca33b-115">Some OWIN servers can be run inside of your own process (&quot;self-hosted&quot;).</span></span> <span data-ttu-id="ca33b-116">Cet exemple montre comment démarrer une application OWIN à l’aide des outils fournis par le package NuGet Microsoft. Owin. Hosting.</span><span class="sxs-lookup"><span data-stu-id="ca33b-116">This sample shows how to start an OWIN application using the tools provided by the Microsoft.Owin.Hosting nuget package.</span></span>

<span data-ttu-id="ca33b-117">**Exemple** de [code source](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld) de | HelloWorld</span><span class="sxs-lookup"><span data-stu-id="ca33b-117">**HelloWorld Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld)</span></span>  
<span data-ttu-id="ca33b-118">OWIN est une abstraction d’API de serveur HTTP qui permet la portabilité de l’application sur différents serveurs.</span><span class="sxs-lookup"><span data-stu-id="ca33b-118">OWIN is a HTTP server API abstraction that enables application portability across various servers.</span></span> <span data-ttu-id="ca33b-119">Cet exemple montre comment écrire une application Hello World à l’aide de **wrappers simples** autour de l’abstraction OWIN brute et l’exécuter sur un serveur web comme ASP.net.</span><span class="sxs-lookup"><span data-stu-id="ca33b-119">This sample demonstrates how to write a Hello World application using some **simple wrappers** around the raw OWIN abstraction and run it on a web server like ASP.NET.</span></span>

<span data-ttu-id="ca33b-120">Exemple de [Code Source](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin) **Hello World RAW OWIN** | </span><span class="sxs-lookup"><span data-stu-id="ca33b-120">**Hello World Raw OWIN Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin)</span></span>  
<span data-ttu-id="ca33b-121">Cet exemple montre comment écrire une application Hello World à l’aide de l’abstraction OWIN **brute** et l’exécuter sur un serveur web comme ASP.net.</span><span class="sxs-lookup"><span data-stu-id="ca33b-121">This sample demonstrates how to write a Hello World application using the **raw** OWIN abstraction and run it on a web server like Asp.Net.</span></span>

<span data-ttu-id="ca33b-122">[Code source](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR) de l' **exemple signalr** | </span><span class="sxs-lookup"><span data-stu-id="ca33b-122">**SignalR Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR)</span></span>  
<span data-ttu-id="ca33b-123">Montre comment effectuer l’auto-hébergement de Signalr à l’aide de OWIN/Katana.</span><span class="sxs-lookup"><span data-stu-id="ca33b-123">Shows how to self-host SignalR using OWIN / Katana.</span></span> <span data-ttu-id="ca33b-124">Pour plus d’informations sur l’auto-hébergement Signalr, consultez [Didacticiel : signalr auto-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="ca33b-124">For more info about self-hosting SignalR, see [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<span data-ttu-id="ca33b-125">**Exemple de fichier statique** |   de [code source](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample)</span><span class="sxs-lookup"><span data-stu-id="ca33b-125">**Static Files Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample) </span></span>  
<span data-ttu-id="ca33b-126">Montre comment prendre en charge les requêtes HTTP pour les fichiers statiques à l’aide de OWIN/Katana.</span><span class="sxs-lookup"><span data-stu-id="ca33b-126">Shows how to support HTTP requests for static files using OWIN / Katana.</span></span>

<span data-ttu-id="ca33b-127">**API Web** | [code source](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi) </span><span class="sxs-lookup"><span data-stu-id="ca33b-127">**Web API** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi) </span></span>  
<span data-ttu-id="ca33b-128">Cet exemple montre comment héberger OWIN dans IIS et ajouter l’API Web au pipeline OWIN.</span><span class="sxs-lookup"><span data-stu-id="ca33b-128">This sample shows how to host OWIN in IIS and add Web API to the OWIN pipeline.</span></span>

<span data-ttu-id="ca33b-129">**Exemple de socket Web** |   de [code source](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample)</span><span class="sxs-lookup"><span data-stu-id="ca33b-129">**Web Socket Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample) </span></span>  
<span data-ttu-id="ca33b-130">Montre comment prendre en charge des sockets Web dans OWIN à l’aide de la classe [System .net. WebSockets. WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) .</span><span class="sxs-lookup"><span data-stu-id="ca33b-130">Shows how to support Web Sockets in OWIN by using the [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) class.</span></span>
