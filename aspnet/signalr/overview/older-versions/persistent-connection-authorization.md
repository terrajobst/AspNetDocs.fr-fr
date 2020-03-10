---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: Authentification et autorisation pour les connexions persistantes Signalr (Signalr 1. x) | Microsoft Docs
author: bradygaster
description: Cette rubrique décrit comment appliquer l’autorisation sur une connexion permanente. Pour obtenir des informations générales sur l’intégration de la sécurité dans une application Signalr,...
ms.author: bradyg
ms.date: 10/21/2013
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 9ccc59e3ea502daf12ce82382ab30ca73ca0f9b5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536539"
---
# <a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a>Authentification et autorisation pour les connexions persistantes SignalR (SignalR 1.x)

de [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Cette rubrique décrit comment appliquer l’autorisation sur une connexion permanente. Pour obtenir des informations générales sur l’intégration de la sécurité dans une application Signalr, consultez [Présentation de la sécurité](index.md).

## <a name="enforce-authorization"></a>Appliquer l’autorisation

Pour appliquer des règles d’autorisation lors de l’utilisation d’un [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) , vous devez remplacer la méthode `AuthorizeRequest`. Vous ne pouvez pas utiliser l’attribut `Authorize` avec des connexions persistantes. La méthode `AuthorizeRequest` est appelée par l’infrastructure Signalr avant chaque demande pour vérifier que l’utilisateur est autorisé à effectuer l’action demandée. La méthode `AuthorizeRequest` n’est pas appelée à partir du client ; au lieu de cela, vous authentifiez l’utilisateur via le mécanisme d’authentification standard de votre application.

L’exemple ci-dessous montre comment limiter les demandes aux utilisateurs authentifiés.

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

Vous pouvez ajouter n’importe quelle logique d’autorisation personnalisée dans la méthode AuthorizeRequest. par exemple, pour vérifier si un utilisateur appartient à un rôle particulier.
