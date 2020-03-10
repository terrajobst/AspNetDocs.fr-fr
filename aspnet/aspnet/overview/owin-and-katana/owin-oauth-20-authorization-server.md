---
uid: aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
title: OWIN serveur d’autorisation OAuth 2,0 | Microsoft Docs
author: hongyes
description: Ce didacticiel vous explique comment implémenter un serveur d’autorisation 2,0 OAuth à l’aide de l’intergiciel (middleware) OAuth OWIN. Il s’agit d’un didacticiel avancé qui outlin uniquement...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 20acee16-c70c-41e9-b38f-92bfcf9a4c1c
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
msc.type: authoredcontent
ms.openlocfilehash: 39c78ee57f791a94af7a5fb66c3ac42d7239760f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78617109"
---
# <a name="owin-oauth-20-authorization-server"></a><span data-ttu-id="2535f-104">Serveur d’autorisation OAuth 2.0 OWIN</span><span class="sxs-lookup"><span data-stu-id="2535f-104">OWIN OAuth 2.0 Authorization Server</span></span>

<span data-ttu-id="2535f-105">L' [infrastructure OAuth 2,0](http://tools.ietf.org/html/rfc6749) permet à une application tierce d’obtenir un accès limité à un service http.</span><span class="sxs-lookup"><span data-stu-id="2535f-105">The [OAuth 2.0 framework](http://tools.ietf.org/html/rfc6749) enables a third-party app to obtain limited access to an HTTP service.</span></span> <span data-ttu-id="2535f-106">Au lieu d’utiliser les informations d’identification du propriétaire de la ressource pour accéder à une ressource protégée, le client obtient un jeton d’accès (qui est une chaîne indiquant une étendue spécifique, une durée de vie et d’autres attributs d’accès).</span><span class="sxs-lookup"><span data-stu-id="2535f-106">Instead of using the resource owner's credentials to access a protected resource, the client obtains an access token (which is a string denoting a specific scope, lifetime, and other access attributes).</span></span> <span data-ttu-id="2535f-107">Les jetons d’accès sont émis aux clients tiers par un serveur d’autorisation avec l’approbation du propriétaire de la ressource.</span><span class="sxs-lookup"><span data-stu-id="2535f-107">Access tokens are issued to third-party clients by an authorization server with the approval of the resource owner.</span></span>

<span data-ttu-id="2535f-108">OWIN définit une interface standard entre les serveurs Web .NET et les applications Web.</span><span class="sxs-lookup"><span data-stu-id="2535f-108">OWIN defines a standard interface between .NET web servers and web applications.</span></span> <span data-ttu-id="2535f-109">L’objectif de l’interface OWIN consiste à découpler le serveur et l’application, à encourager le développement de modules simples pour le développement Web .NET et, en tant que norme ouverte, à stimuler l’écosystème Open source des outils de développement Web .NET.</span><span class="sxs-lookup"><span data-stu-id="2535f-109">The goal of the OWIN interface is to decouple server and application, encourage the development of simple modules for .NET web development, and, by being an open standard, stimulate the open source ecosystem of .NET web development tools.</span></span>

<span data-ttu-id="2535f-110">Pour plus d’informations, consultez [OWIN](http://owin.org/)</span><span class="sxs-lookup"><span data-stu-id="2535f-110">For more information, see [OWIN](http://owin.org/)</span></span>
