---
uid: signalr/overview/security/persistent-connection-authorization
title: Authentification et autorisation pour les connexions persistantes Signalr | Microsoft Docs
author: bradygaster
description: Cette rubrique décrit comment appliquer l’autorisation sur une connexion permanente. Pour obtenir des informations générales sur l’intégration de la sécurité dans une application Signalr,...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: e264677b-9c01-47ec-94f9-3cd8f08f94af
msc.legacyurl: /signalr/overview/security/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 7e69d3c1a18f1239142891734ba58cd2b0078f84
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578875"
---
# <a name="authentication-and-authorization-for-signalr-persistent-connections"></a>Authentification et autorisation pour les connexions persistantes SignalR

de [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Cette rubrique décrit comment appliquer l’autorisation sur une connexion permanente. Pour obtenir des informations générales sur l’intégration de la sécurité dans une application Signalr, consultez [Présentation de la sécurité](introduction-to-security.md).
>
> ## <a name="software-versions-used-in-this-topic"></a>Versions logicielles utilisées dans cette rubrique
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - Signalr version 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Versions précédentes de cette rubrique
>
> Pour plus d’informations sur les versions antérieures de Signalr, consultez [versions antérieures de signalr](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Questions et commentaires
>
> N’hésitez pas à nous faire part de vos commentaires sur la façon dont vous aimez ce didacticiel et sur ce que nous pourrions améliorer dans les commentaires en bas de la page. Si vous avez des questions qui ne sont pas directement liées au didacticiel, vous pouvez les poster sur le [forum ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).

## <a name="enforce-authorization"></a>Appliquer l’autorisation

Pour appliquer des règles d’autorisation lors de l’utilisation d’un [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) , vous devez remplacer la méthode `AuthorizeRequest`. Vous ne pouvez pas utiliser l’attribut `Authorize` avec des connexions persistantes. La méthode `AuthorizeRequest` est appelée par l’infrastructure Signalr avant chaque demande pour vérifier que l’utilisateur est autorisé à effectuer l’action demandée. La méthode `AuthorizeRequest` n’est pas appelée à partir du client ; au lieu de cela, vous authentifiez l’utilisateur via le mécanisme d’authentification standard de votre application.

L’exemple ci-dessous montre comment limiter les demandes aux utilisateurs authentifiés.

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

Vous pouvez ajouter n’importe quelle logique d’autorisation personnalisée dans la méthode AuthorizeRequest. par exemple, pour vérifier si un utilisateur appartient à un rôle particulier.
