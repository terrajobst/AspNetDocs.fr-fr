---
uid: signalr/overview/older-versions/hub-authorization
title: Authentification et autorisation pour les concentrateurs Signalr (Signalr 1. x) | Microsoft Docs
author: bradygaster
description: Cette rubrique décrit comment restreindre les utilisateurs ou les rôles qui peuvent accéder aux méthodes de concentrateur.
ms.author: bradyg
ms.date: 10/17/2013
ms.assetid: 3d2dfc0e-eac2-4076-a468-325d3d01cc7b
msc.legacyurl: /signalr/overview/older-versions/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: 8182677c8931f060d98d17008b16ad545bee4e69
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558533"
---
# <a name="authentication-and-authorization-for-signalr-hubs-signalr-1x"></a>Authentification et autorisation pour SignalR Hubs (SignalR 1.x)

de [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Cette rubrique décrit comment restreindre les utilisateurs ou les rôles qui peuvent accéder aux méthodes de concentrateur.

## <a name="overview"></a>Présentation

Cette rubrique contient les sections suivantes :

- [Autorisation d’attribut](#authorizeattribute)
- [Exiger l’authentification pour tous les hubs](#requireauth)
- [Autorisation personnalisée](#custom)
- [Transmettre les informations d’authentification aux clients](#passauth)
- [Options d’authentification pour les clients .NET](#authoptions)

    - [Cookie avec l’authentification par formulaire](#cookie)
    - [Authentification Windows](#windows)
    - [En-tête de connexion](#header)
    - [Certificate](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a>Autorisation d’attribut

Signalr fournit l’attribut [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) pour spécifier les utilisateurs ou les rôles qui ont accès à un concentrateur ou à une méthode. Cet attribut se trouve dans l’espace de noms `Microsoft.AspNet.SignalR`. Vous appliquez l’attribut `Authorize` à un Hub ou à des méthodes particulières dans un concentrateur. Lorsque vous appliquez l’attribut `Authorize` à une classe de concentrateur, l’exigence d’autorisation spécifiée est appliquée à toutes les méthodes dans le concentrateur. Les différents types d’exigences d’autorisation que vous pouvez appliquer sont illustrés ci-dessous. Sans l’attribut `Authorize`, toutes les méthodes publiques sur le concentrateur sont disponibles pour un client qui est connecté au concentrateur.

Si vous avez défini un rôle nommé « admin » dans votre application Web, vous pouvez spécifier que seuls les utilisateurs de ce rôle peuvent accéder à un concentrateur à l’aide du code suivant.

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

Vous pouvez aussi spécifier qu’un concentrateur contient une méthode qui est disponible pour tous les utilisateurs, et une deuxième méthode qui est uniquement disponible pour les utilisateurs authentifiés, comme indiqué ci-dessous.

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

Les exemples suivants répondent à différents scénarios d’autorisation :

- `[Authorize]` : uniquement les utilisateurs authentifiés
- `[Authorize(Roles = "Admin,Manager")]` : seuls les utilisateurs authentifiés dans les rôles spécifiés
- `[Authorize(Users = "user1,user2")]` : seuls les utilisateurs authentifiés avec les noms d’utilisateurs spécifiés
- `[Authorize(RequireOutgoing=false)]` : seuls les utilisateurs authentifiés peuvent appeler le Hub, mais les appels à partir du serveur vers les clients ne sont pas limités par l’autorisation, par exemple, lorsque seuls certains utilisateurs peuvent envoyer un message, mais que les autres peuvent recevoir le message. La propriété RequireOutgoing peut uniquement être appliquée à l’ensemble du concentrateur, et non à des méthodes individuelles dans le concentrateur. Quand RequireOutgoing n’est pas défini sur false, seuls les utilisateurs qui satisfont à l’exigence d’autorisation sont appelés à partir du serveur.

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a>Exiger l’authentification pour tous les hubs

Vous pouvez exiger l’authentification pour tous les concentrateurs et méthodes de concentrateur dans votre application en appelant la méthode [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) au démarrage de l’application. Vous pouvez utiliser cette méthode lorsque vous avez plusieurs hubs et que vous souhaitez appliquer une exigence d’authentification pour tous. Avec cette méthode, vous ne pouvez pas spécifier un rôle, un utilisateur ou une autorisation sortante. Vous ne pouvez spécifier que l’accès aux méthodes de concentrateur est limité aux utilisateurs authentifiés. Toutefois, vous pouvez toujours appliquer l’attribut Authorize aux hubs ou aux méthodes pour spécifier des exigences supplémentaires. Toutes les exigences que vous spécifiez dans les attributs sont appliquées en plus de l’exigence de base de l’authentification.

L’exemple suivant montre un fichier global. asax qui limite toutes les méthodes de concentrateur aux utilisateurs authentifiés.

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

Si vous appelez la méthode `RequireAuthentication()` une fois qu’une demande Signalr a été traitée, Signalr lèvera une exception `InvalidOperationException`. Cette exception est levée, car vous ne pouvez pas ajouter un module au HubPipeline une fois que le pipeline a été appelé. L’exemple précédent montre l’appel de la méthode `RequireAuthentication` dans la méthode `Application_Start` qui est exécutée une fois avant la première demande.

<a id="custom"></a>

## <a name="customized-authorization"></a>Autorisation personnalisée

Si vous avez besoin de personnaliser la façon dont l’autorisation est déterminée, vous pouvez créer une classe qui dérive de `AuthorizeAttribute` et substituer la méthode [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) . Cette méthode est appelée pour chaque demande afin de déterminer si l’utilisateur est autorisé à terminer la demande. Dans la méthode substituée, vous fournissez la logique nécessaire pour votre scénario d’autorisation. L’exemple suivant montre comment appliquer l’autorisation par le biais d’une identité basée sur les revendications.

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a>Transmettre les informations d’authentification aux clients

Vous devrez peut-être utiliser les informations d’authentification dans le code qui s’exécute sur le client. Vous transmettez les informations requises lors de l’appel des méthodes sur le client. Par exemple, une méthode d’application de conversation peut passer en tant que paramètre le nom d’utilisateur de la personne qui publie un message, comme indiqué ci-dessous.

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

Vous pouvez aussi créer un objet pour représenter les informations d’authentification et passer cet objet en tant que paramètre, comme indiqué ci-dessous.

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

Vous ne devez jamais transmettre l’ID de connexion d’un client à d’autres clients, car un utilisateur malveillant pourrait l’utiliser pour imiter une requête de ce client.

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a>Options d’authentification pour les clients .NET

Si vous disposez d’un client .NET, tel qu’une application console, qui interagit avec un concentrateur qui est limité aux utilisateurs authentifiés, vous pouvez transmettre les informations d’identification d’authentification dans un cookie, l’en-tête de connexion ou un certificat. Les exemples de cette section montrent comment utiliser ces différentes méthodes pour authentifier un utilisateur. Ce ne sont pas des applications Signalr entièrement fonctionnelles. Pour plus d’informations sur les clients .NET avec Signalr, consultez [Guide de l’API hubs-client .net](../guide-to-the-api/hubs-api-guide-net-client.md).

<a id="cookie"></a>

### <a name="cookie"></a>Cookie

Lorsque votre client .NET interagit avec un concentrateur qui utilise l’authentification par formulaire ASP.NET, vous devez définir manuellement le cookie d’authentification sur la connexion. Vous ajoutez le cookie à la propriété `CookieContainer` sur l’objet [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) . L’exemple suivant montre une application console qui récupère un cookie d’authentification à partir d’une page Web et ajoute ce cookie à la connexion. L’URL `https://www.contoso.com/RemoteLogin` dans l’exemple pointe vers une page Web que vous devez créer. La page récupère le nom d’utilisateur et le mot de passe publiés et tente de se connecter à l’utilisateur avec les informations d’identification.

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

L’application console publie les informations d’identification dans www.contoso.com/RemoteLogin qui peuvent faire référence à une page vide contenant le fichier code-behind suivant.

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a>Authentification Windows

Lorsque vous utilisez l’authentification Windows, vous pouvez passer les informations d’identification de l’utilisateur actuel à l’aide de la propriété [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) . Vous définissez les informations d’identification pour la connexion à la valeur de DefaultCredentials.

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a>En-tête de connexion

Si votre application n’utilise pas de cookies, vous pouvez transmettre les informations utilisateur dans l’en-tête de connexion. Par exemple, vous pouvez passer un jeton dans l’en-tête de connexion.

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

Ensuite, dans le Hub, vous devez vérifier le jeton de l’utilisateur.

<a id="certificate"></a>

### <a name="certificate"></a>Certificat

Vous pouvez transmettre un certificat client pour vérifier l’utilisateur. Vous ajoutez le certificat lors de la création de la connexion. L’exemple suivant montre uniquement comment ajouter un certificat client à la connexion ; elle n’affiche pas l’application console complète. Elle utilise la classe [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) qui fournit plusieurs façons différentes de créer le certificat.

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
