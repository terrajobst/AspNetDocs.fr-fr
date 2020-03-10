---
uid: signalr/overview/security/hub-authorization
title: Authentification et autorisation pour les concentrateurs Signalr | Microsoft Docs
author: bradygaster
description: Cette rubrique décrit comment restreindre les utilisateurs ou les rôles qui peuvent accéder aux méthodes de concentrateur. Versions logicielles utilisées dans cette rubrique Visual Studio 2013 .NET 4,5 Signalr...
ms.author: bradyg
ms.date: 01/05/2015
ms.assetid: a610c796-c131-473c-baef-2e6c568cb2a2
msc.legacyurl: /signalr/overview/security/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: 5006af5e623da6958a6d59949c6f2cf776c77fc3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578917"
---
# <a name="authentication-and-authorization-for-signalr-hubs"></a>Authentification et autorisation pour SignalR Hubs

de [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Cette rubrique décrit comment restreindre les utilisateurs ou les rôles qui peuvent accéder aux méthodes de concentrateur.
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

Signalr fournit l’attribut [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) pour spécifier les utilisateurs ou les rôles qui ont accès à un concentrateur ou à une méthode. Cet attribut se trouve dans l’espace de noms `Microsoft.AspNet.SignalR`. Vous appliquez l’attribut `Authorize` à un Hub ou à des méthodes particulières dans un concentrateur. Lorsque vous appliquez l’attribut `Authorize` à une classe de concentrateur, l’exigence d’autorisation spécifiée est appliquée à toutes les méthodes dans le concentrateur. Cette rubrique fournit des exemples des différents types d’exigences d’autorisation que vous pouvez appliquer. Sans l’attribut `Authorize`, un client connecté peut accéder à n’importe quelle méthode publique sur le concentrateur.

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

Vous pouvez exiger l’authentification pour tous les concentrateurs et méthodes de concentrateur dans votre application en appelant la méthode [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) au démarrage de l’application. Vous pouvez utiliser cette méthode lorsque vous avez plusieurs hubs et que vous souhaitez appliquer une exigence d’authentification pour tous. Avec cette méthode, vous ne pouvez pas spécifier les conditions requises pour le rôle, l’utilisateur ou l’autorisation sortante. Vous ne pouvez spécifier que l’accès aux méthodes de concentrateur est limité aux utilisateurs authentifiés. Toutefois, vous pouvez toujours appliquer l’attribut Authorize aux hubs ou aux méthodes pour spécifier des exigences supplémentaires. Toute spécification que vous spécifiez dans un attribut est ajoutée à l’exigence de base de l’authentification.

L’exemple suivant montre un fichier de démarrage qui restreint toutes les méthodes de concentrateur aux utilisateurs authentifiés.

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

Si vous appelez la méthode `RequireAuthentication()` une fois qu’une demande Signalr a été traitée, Signalr lèvera une exception `InvalidOperationException`. Signalr lève cette exception, car vous ne pouvez pas ajouter un module au HubPipeline une fois que le pipeline a été appelé. L’exemple précédent montre l’appel de la méthode `RequireAuthentication` dans la méthode `Configuration` qui est exécutée une fois avant la première demande.

<a id="custom"></a>

## <a name="customized-authorization"></a>Autorisation personnalisée

Si vous avez besoin de personnaliser la façon dont l’autorisation est déterminée, vous pouvez créer une classe qui dérive de `AuthorizeAttribute` et substituer la méthode [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) . Pour chaque demande, Signalr appelle cette méthode pour déterminer si l’utilisateur est autorisé à terminer la demande. Dans la méthode substituée, vous fournissez la logique nécessaire pour votre scénario d’autorisation. L’exemple suivant montre comment appliquer l’autorisation par le biais d’une identité basée sur les revendications.

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

Lorsque votre client .NET interagit avec un concentrateur qui utilise l’authentification par formulaire ASP.NET, vous devez définir manuellement le cookie d’authentification sur la connexion. Vous ajoutez le cookie à la propriété `CookieContainer` sur l’objet [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) . L’exemple suivant montre une application console qui récupère un cookie d’authentification à partir d’une page Web et ajoute ce cookie à la connexion.

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

L’application console publie les informations d’identification dans <strong>www.contoso.com/RemoteLogin</strong> qui peuvent faire référence à une page vide contenant le fichier code-behind suivant.

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
