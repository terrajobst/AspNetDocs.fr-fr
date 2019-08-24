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
ms.sourcegitcommit: b95316530fa51087d6c400ff91814fe37e73f7e8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/23/2019
ms.locfileid: "70000685"
---
# <a name="owin-oauth-20-authorization-server"></a>Serveur d’autorisation OAuth 2.0 OWIN

L' [infrastructure OAuth 2,0](http://tools.ietf.org/html/rfc6749) permet à une application tierce d’obtenir un accès limité à un service http. Au lieu d’utiliser les informations d’identification du propriétaire de la ressource pour accéder à une ressource protégée, le client obtient un jeton d’accès (qui est une chaîne indiquant une étendue spécifique, une durée de vie et d’autres attributs d’accès). Les jetons d’accès sont émis aux clients tiers par un serveur d’autorisation avec l’approbation du propriétaire de la ressource.

OWIN définit une interface standard entre les serveurs Web .NET et les applications Web. L’objectif de l’interface OWIN consiste à découpler le serveur et l’application, à encourager le développement de modules simples pour le développement Web .NET et, en tant que norme ouverte, à stimuler l’écosystème Open source des outils de développement Web .NET.

Pour plus d’informations, consultez [OWIN](http://owin.org/)
