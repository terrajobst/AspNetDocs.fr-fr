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
# <a name="katana-samples"></a>Exemples Katana

par [Microsoft](https://github.com/microsoft)

## <a name="katana-samples"></a>Exemples Katana

**Exemple de ASP.net Routes** | [code source](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)  
Dans certaines applications, vous souhaiterez raccorder des composants OWIN dans la table d’itinéraires Asp.Net côte à côte avec des composants non OWIN. Cet exemple montre comment utiliser les méthodes d’extension RouteCollection MapOwinPath et MapOwinRoute fournies par Microsoft. Owin. Host. SystemWeb.

Exemple de [code source](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines) de **branchement de pipelines** |   
Les pipelines de traitement des demandes OWIN n’ont pas besoin d’être linéaires, ils peuvent être reliés pour traiter les requêtes de différentes façons. Cet exemple montre comment construire un pipeline de branchement basé sur des chemins de requête ou d’autres données de requête telles que des en-têtes. Ces composants sont disponibles dans le package NuGet Microsoft. Owin. Mapping.

**Exemple de serveur personnalisé** | [code source](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer)   
Montre comment utiliser un serveur OWIN personnalisé lors de l’auto-hébergement de OWIN.

Exemple de [Code Source](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded) | **incorporé**  
Certains serveurs OWIN peuvent être exécutés dans votre propre processus (&quot;&quot;auto-hébergés). Cet exemple montre comment démarrer une application OWIN à l’aide des outils fournis par le package NuGet Microsoft. Owin. Hosting.

**Exemple** de [code source](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld) de | HelloWorld  
OWIN est une abstraction d’API de serveur HTTP qui permet la portabilité de l’application sur différents serveurs. Cet exemple montre comment écrire une application Hello World à l’aide de **wrappers simples** autour de l’abstraction OWIN brute et l’exécuter sur un serveur web comme ASP.net.

Exemple de [Code Source](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin) **Hello World RAW OWIN** |   
Cet exemple montre comment écrire une application Hello World à l’aide de l’abstraction OWIN **brute** et l’exécuter sur un serveur web comme ASP.net.

[Code source](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR) de l' **exemple signalr** |   
Montre comment effectuer l’auto-hébergement de Signalr à l’aide de OWIN/Katana. Pour plus d’informations sur l’auto-hébergement Signalr, consultez [Didacticiel : signalr auto-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

**Exemple de fichier statique** |   de [code source](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample)  
Montre comment prendre en charge les requêtes HTTP pour les fichiers statiques à l’aide de OWIN/Katana.

**API Web** | [code source](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi)   
Cet exemple montre comment héberger OWIN dans IIS et ajouter l’API Web au pipeline OWIN.

**Exemple de socket Web** |   de [code source](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample)  
Montre comment prendre en charge des sockets Web dans OWIN à l’aide de la classe [System .net. WebSockets. WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) .
