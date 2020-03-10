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
# <a name="authentication-and-authorization-for-signalr-hubs-signalr-1x"></a><span data-ttu-id="5cbd7-103">Authentification et autorisation pour SignalR Hubs (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="5cbd7-103">Authentication and Authorization for SignalR Hubs (SignalR 1.x)</span></span>

<span data-ttu-id="5cbd7-104">de [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="5cbd7-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="5cbd7-105">Cette rubrique décrit comment restreindre les utilisateurs ou les rôles qui peuvent accéder aux méthodes de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-105">This topic describes how to restrict which users or roles can access hub methods.</span></span>

## <a name="overview"></a><span data-ttu-id="5cbd7-106">Présentation</span><span class="sxs-lookup"><span data-stu-id="5cbd7-106">Overview</span></span>

<span data-ttu-id="5cbd7-107">Cette rubrique contient les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="5cbd7-107">This topic contains the following sections:</span></span>

- [<span data-ttu-id="5cbd7-108">Autorisation d’attribut</span><span class="sxs-lookup"><span data-stu-id="5cbd7-108">Authorize attribute</span></span>](#authorizeattribute)
- [<span data-ttu-id="5cbd7-109">Exiger l’authentification pour tous les hubs</span><span class="sxs-lookup"><span data-stu-id="5cbd7-109">Require authentication for all hubs</span></span>](#requireauth)
- [<span data-ttu-id="5cbd7-110">Autorisation personnalisée</span><span class="sxs-lookup"><span data-stu-id="5cbd7-110">Customized authorization</span></span>](#custom)
- [<span data-ttu-id="5cbd7-111">Transmettre les informations d’authentification aux clients</span><span class="sxs-lookup"><span data-stu-id="5cbd7-111">Pass authentication information to clients</span></span>](#passauth)
- [<span data-ttu-id="5cbd7-112">Options d’authentification pour les clients .NET</span><span class="sxs-lookup"><span data-stu-id="5cbd7-112">Authentication options for .NET clients</span></span>](#authoptions)

    - [<span data-ttu-id="5cbd7-113">Cookie avec l’authentification par formulaire</span><span class="sxs-lookup"><span data-stu-id="5cbd7-113">Cookie with Forms Authentication</span></span>](#cookie)
    - [<span data-ttu-id="5cbd7-114">Authentification Windows</span><span class="sxs-lookup"><span data-stu-id="5cbd7-114">Windows Authentication</span></span>](#windows)
    - [<span data-ttu-id="5cbd7-115">En-tête de connexion</span><span class="sxs-lookup"><span data-stu-id="5cbd7-115">Connection header</span></span>](#header)
    - [<span data-ttu-id="5cbd7-116">Certificate</span><span class="sxs-lookup"><span data-stu-id="5cbd7-116">Certificate</span></span>](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a><span data-ttu-id="5cbd7-117">Autorisation d’attribut</span><span class="sxs-lookup"><span data-stu-id="5cbd7-117">Authorize attribute</span></span>

<span data-ttu-id="5cbd7-118">Signalr fournit l’attribut [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) pour spécifier les utilisateurs ou les rôles qui ont accès à un concentrateur ou à une méthode.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-118">SignalR provides the [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users or roles have access to a hub or method.</span></span> <span data-ttu-id="5cbd7-119">Cet attribut se trouve dans l’espace de noms `Microsoft.AspNet.SignalR`.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-119">This attribute is located in the `Microsoft.AspNet.SignalR` namespace.</span></span> <span data-ttu-id="5cbd7-120">Vous appliquez l’attribut `Authorize` à un Hub ou à des méthodes particulières dans un concentrateur.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-120">You apply the `Authorize` attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="5cbd7-121">Lorsque vous appliquez l’attribut `Authorize` à une classe de concentrateur, l’exigence d’autorisation spécifiée est appliquée à toutes les méthodes dans le concentrateur.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-121">When you apply the `Authorize` attribute to a hub class, the specified authorization requirement is applied to all of the methods in the hub.</span></span> <span data-ttu-id="5cbd7-122">Les différents types d’exigences d’autorisation que vous pouvez appliquer sont illustrés ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-122">The different types of authorization requirements that you can apply are shown below.</span></span> <span data-ttu-id="5cbd7-123">Sans l’attribut `Authorize`, toutes les méthodes publiques sur le concentrateur sont disponibles pour un client qui est connecté au concentrateur.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-123">Without the `Authorize` attribute, all public methods on the hub are available to a client that is connected to the hub.</span></span>

<span data-ttu-id="5cbd7-124">Si vous avez défini un rôle nommé « admin » dans votre application Web, vous pouvez spécifier que seuls les utilisateurs de ce rôle peuvent accéder à un concentrateur à l’aide du code suivant.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-124">If you have defined a role named "Admin" in your web application, you could specify that only users in that role can access a hub with the following code.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

<span data-ttu-id="5cbd7-125">Vous pouvez aussi spécifier qu’un concentrateur contient une méthode qui est disponible pour tous les utilisateurs, et une deuxième méthode qui est uniquement disponible pour les utilisateurs authentifiés, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-125">Or, you can specify that a hub contains one method that is available to all users, and a second method that is only available to authenticated users, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

<span data-ttu-id="5cbd7-126">Les exemples suivants répondent à différents scénarios d’autorisation :</span><span class="sxs-lookup"><span data-stu-id="5cbd7-126">The following examples address different authorization scenarios:</span></span>

- <span data-ttu-id="5cbd7-127">`[Authorize]` : uniquement les utilisateurs authentifiés</span><span class="sxs-lookup"><span data-stu-id="5cbd7-127">`[Authorize]` – only authenticated users</span></span>
- <span data-ttu-id="5cbd7-128">`[Authorize(Roles = "Admin,Manager")]` : seuls les utilisateurs authentifiés dans les rôles spécifiés</span><span class="sxs-lookup"><span data-stu-id="5cbd7-128">`[Authorize(Roles = "Admin,Manager")]` – only authenticated users in the specified roles</span></span>
- <span data-ttu-id="5cbd7-129">`[Authorize(Users = "user1,user2")]` : seuls les utilisateurs authentifiés avec les noms d’utilisateurs spécifiés</span><span class="sxs-lookup"><span data-stu-id="5cbd7-129">`[Authorize(Users = "user1,user2")]` – only authenticated users with the specified user names</span></span>
- <span data-ttu-id="5cbd7-130">`[Authorize(RequireOutgoing=false)]` : seuls les utilisateurs authentifiés peuvent appeler le Hub, mais les appels à partir du serveur vers les clients ne sont pas limités par l’autorisation, par exemple, lorsque seuls certains utilisateurs peuvent envoyer un message, mais que les autres peuvent recevoir le message.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-130">`[Authorize(RequireOutgoing=false)]` – only authenticated users can invoke the hub, but calls from the server back to clients are not limited by authorization, such as, when only certain users can send a message but all others can receive the message.</span></span> <span data-ttu-id="5cbd7-131">La propriété RequireOutgoing peut uniquement être appliquée à l’ensemble du concentrateur, et non à des méthodes individuelles dans le concentrateur.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-131">The RequireOutgoing property can only be applied to the entire hub, not on individuals methods within the hub.</span></span> <span data-ttu-id="5cbd7-132">Quand RequireOutgoing n’est pas défini sur false, seuls les utilisateurs qui satisfont à l’exigence d’autorisation sont appelés à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-132">When RequireOutgoing is not set to false, only users that meet the authorization requirement are called from the server.</span></span>

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a><span data-ttu-id="5cbd7-133">Exiger l’authentification pour tous les hubs</span><span class="sxs-lookup"><span data-stu-id="5cbd7-133">Require authentication for all hubs</span></span>

<span data-ttu-id="5cbd7-134">Vous pouvez exiger l’authentification pour tous les concentrateurs et méthodes de concentrateur dans votre application en appelant la méthode [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-134">You can require authentication for all hubs and hub methods in your application by calling the [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="5cbd7-135">Vous pouvez utiliser cette méthode lorsque vous avez plusieurs hubs et que vous souhaitez appliquer une exigence d’authentification pour tous.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-135">You might use this method when you have multiple hubs and want to enforce an authentication requirement for all of them.</span></span> <span data-ttu-id="5cbd7-136">Avec cette méthode, vous ne pouvez pas spécifier un rôle, un utilisateur ou une autorisation sortante.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-136">With this method, you cannot specify role, user, or outgoing authorization.</span></span> <span data-ttu-id="5cbd7-137">Vous ne pouvez spécifier que l’accès aux méthodes de concentrateur est limité aux utilisateurs authentifiés.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-137">You can only specify that access to the hub methods is restricted to authenticated users.</span></span> <span data-ttu-id="5cbd7-138">Toutefois, vous pouvez toujours appliquer l’attribut Authorize aux hubs ou aux méthodes pour spécifier des exigences supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-138">However, you can still apply the Authorize attribute to hubs or methods to specify additional requirements.</span></span> <span data-ttu-id="5cbd7-139">Toutes les exigences que vous spécifiez dans les attributs sont appliquées en plus de l’exigence de base de l’authentification.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-139">Any requirement you specify in attributes is applied in addition to the basic requirement of authentication.</span></span>

<span data-ttu-id="5cbd7-140">L’exemple suivant montre un fichier global. asax qui limite toutes les méthodes de concentrateur aux utilisateurs authentifiés.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-140">The following example shows a Global.asax file which restricts all hub methods to authenticated users.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

<span data-ttu-id="5cbd7-141">Si vous appelez la méthode `RequireAuthentication()` une fois qu’une demande Signalr a été traitée, Signalr lèvera une exception `InvalidOperationException`.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-141">If you call the `RequireAuthentication()` method after a SignalR request has been processed, SignalR will throw a `InvalidOperationException` exception.</span></span> <span data-ttu-id="5cbd7-142">Cette exception est levée, car vous ne pouvez pas ajouter un module au HubPipeline une fois que le pipeline a été appelé.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-142">This exception is thrown because you cannot add a module to the HubPipeline after the pipeline has been invoked.</span></span> <span data-ttu-id="5cbd7-143">L’exemple précédent montre l’appel de la méthode `RequireAuthentication` dans la méthode `Application_Start` qui est exécutée une fois avant la première demande.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-143">The previous example shows calling the `RequireAuthentication` method in the `Application_Start` method which is executed one time prior to handling the first request.</span></span>

<a id="custom"></a>

## <a name="customized-authorization"></a><span data-ttu-id="5cbd7-144">Autorisation personnalisée</span><span class="sxs-lookup"><span data-stu-id="5cbd7-144">Customized authorization</span></span>

<span data-ttu-id="5cbd7-145">Si vous avez besoin de personnaliser la façon dont l’autorisation est déterminée, vous pouvez créer une classe qui dérive de `AuthorizeAttribute` et substituer la méthode [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) .</span><span class="sxs-lookup"><span data-stu-id="5cbd7-145">If you need to customize how authorization is determined, you can create a class that derives from `AuthorizeAttribute` and override the [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) method.</span></span> <span data-ttu-id="5cbd7-146">Cette méthode est appelée pour chaque demande afin de déterminer si l’utilisateur est autorisé à terminer la demande.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-146">This method is called for each request to determine whether the user is authorized to complete the request.</span></span> <span data-ttu-id="5cbd7-147">Dans la méthode substituée, vous fournissez la logique nécessaire pour votre scénario d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-147">In the overridden method, you provide the necessary logic for your authorization scenario.</span></span> <span data-ttu-id="5cbd7-148">L’exemple suivant montre comment appliquer l’autorisation par le biais d’une identité basée sur les revendications.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-148">The following example shows how to enforce authorization through claims-based identity.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a><span data-ttu-id="5cbd7-149">Transmettre les informations d’authentification aux clients</span><span class="sxs-lookup"><span data-stu-id="5cbd7-149">Pass authentication information to clients</span></span>

<span data-ttu-id="5cbd7-150">Vous devrez peut-être utiliser les informations d’authentification dans le code qui s’exécute sur le client.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-150">You may need to use authentication information in the code that runs on the client.</span></span> <span data-ttu-id="5cbd7-151">Vous transmettez les informations requises lors de l’appel des méthodes sur le client.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-151">You pass the required information when calling the methods on the client.</span></span> <span data-ttu-id="5cbd7-152">Par exemple, une méthode d’application de conversation peut passer en tant que paramètre le nom d’utilisateur de la personne qui publie un message, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-152">For example, a chat application method could pass as a parameter the user name of the person posting a message, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

<span data-ttu-id="5cbd7-153">Vous pouvez aussi créer un objet pour représenter les informations d’authentification et passer cet objet en tant que paramètre, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-153">Or, you can create an object to represent the authentication information and pass that object as a parameter, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

<span data-ttu-id="5cbd7-154">Vous ne devez jamais transmettre l’ID de connexion d’un client à d’autres clients, car un utilisateur malveillant pourrait l’utiliser pour imiter une requête de ce client.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-154">You should never pass one client's connection id to other clients, as a malicious user could use it to mimic a request from that client.</span></span>

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a><span data-ttu-id="5cbd7-155">Options d’authentification pour les clients .NET</span><span class="sxs-lookup"><span data-stu-id="5cbd7-155">Authentication options for .NET clients</span></span>

<span data-ttu-id="5cbd7-156">Si vous disposez d’un client .NET, tel qu’une application console, qui interagit avec un concentrateur qui est limité aux utilisateurs authentifiés, vous pouvez transmettre les informations d’identification d’authentification dans un cookie, l’en-tête de connexion ou un certificat.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-156">When you have a .NET client, such as a console app, which interacts with a hub that is limited to authenticated users, you can pass the authentication credentials in a cookie, the connection header, or a certificate.</span></span> <span data-ttu-id="5cbd7-157">Les exemples de cette section montrent comment utiliser ces différentes méthodes pour authentifier un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-157">The examples in this section show how to use those different methods for authenticating a user.</span></span> <span data-ttu-id="5cbd7-158">Ce ne sont pas des applications Signalr entièrement fonctionnelles.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-158">They are not fully-functional SignalR apps.</span></span> <span data-ttu-id="5cbd7-159">Pour plus d’informations sur les clients .NET avec Signalr, consultez [Guide de l’API hubs-client .net](../guide-to-the-api/hubs-api-guide-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="5cbd7-159">For more information about .NET clients with SignalR, see [Hubs API Guide - .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span></span>

<a id="cookie"></a>

### <a name="cookie"></a><span data-ttu-id="5cbd7-160">Cookie</span><span class="sxs-lookup"><span data-stu-id="5cbd7-160">Cookie</span></span>

<span data-ttu-id="5cbd7-161">Lorsque votre client .NET interagit avec un concentrateur qui utilise l’authentification par formulaire ASP.NET, vous devez définir manuellement le cookie d’authentification sur la connexion.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-161">When your .NET client interacts with a hub that uses ASP.NET Forms Authentication, you will need to manually set the authentication cookie on the connection.</span></span> <span data-ttu-id="5cbd7-162">Vous ajoutez le cookie à la propriété `CookieContainer` sur l’objet [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) .</span><span class="sxs-lookup"><span data-stu-id="5cbd7-162">You add the cookie to the `CookieContainer` property on the [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) object.</span></span> <span data-ttu-id="5cbd7-163">L’exemple suivant montre une application console qui récupère un cookie d’authentification à partir d’une page Web et ajoute ce cookie à la connexion.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-163">The following example shows a console app that retrieves an authentication cookie from a web page and adds that cookie to the connection.</span></span> <span data-ttu-id="5cbd7-164">L’URL `https://www.contoso.com/RemoteLogin` dans l’exemple pointe vers une page Web que vous devez créer.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-164">The URL `https://www.contoso.com/RemoteLogin` in the example points to a web page that you would need to create.</span></span> <span data-ttu-id="5cbd7-165">La page récupère le nom d’utilisateur et le mot de passe publiés et tente de se connecter à l’utilisateur avec les informations d’identification.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-165">The page would retrieve the posted user name and password, and attempt to log in the user with the credentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

<span data-ttu-id="5cbd7-166">L’application console publie les informations d’identification dans www.contoso.com/RemoteLogin qui peuvent faire référence à une page vide contenant le fichier code-behind suivant.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-166">The console app posts the credentials to www.contoso.com/RemoteLogin which could refer to an empty page that contains the following code-behind file.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a><span data-ttu-id="5cbd7-167">Authentification Windows</span><span class="sxs-lookup"><span data-stu-id="5cbd7-167">Windows authentication</span></span>

<span data-ttu-id="5cbd7-168">Lorsque vous utilisez l’authentification Windows, vous pouvez passer les informations d’identification de l’utilisateur actuel à l’aide de la propriété [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) .</span><span class="sxs-lookup"><span data-stu-id="5cbd7-168">When using Windows authentication, you can pass the current user's credentials by using the [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) property.</span></span> <span data-ttu-id="5cbd7-169">Vous définissez les informations d’identification pour la connexion à la valeur de DefaultCredentials.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-169">You set the credentials for the connection to the value of the DefaultCredentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a><span data-ttu-id="5cbd7-170">En-tête de connexion</span><span class="sxs-lookup"><span data-stu-id="5cbd7-170">Connection header</span></span>

<span data-ttu-id="5cbd7-171">Si votre application n’utilise pas de cookies, vous pouvez transmettre les informations utilisateur dans l’en-tête de connexion.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-171">If your application is not using cookies, you can pass user information in the connection header.</span></span> <span data-ttu-id="5cbd7-172">Par exemple, vous pouvez passer un jeton dans l’en-tête de connexion.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-172">For example, you can pass a token in the connection header.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

<span data-ttu-id="5cbd7-173">Ensuite, dans le Hub, vous devez vérifier le jeton de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-173">Then, in the hub, you would verify the user's token.</span></span>

<a id="certificate"></a>

### <a name="certificate"></a><span data-ttu-id="5cbd7-174">Certificat</span><span class="sxs-lookup"><span data-stu-id="5cbd7-174">Certificate</span></span>

<span data-ttu-id="5cbd7-175">Vous pouvez transmettre un certificat client pour vérifier l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-175">You can pass a client certificate to verify the user.</span></span> <span data-ttu-id="5cbd7-176">Vous ajoutez le certificat lors de la création de la connexion.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-176">You add the certificate when creating the connection.</span></span> <span data-ttu-id="5cbd7-177">L’exemple suivant montre uniquement comment ajouter un certificat client à la connexion ; elle n’affiche pas l’application console complète.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-177">The following example shows only how to add a client certificate to the connection; it does not show the full console app.</span></span> <span data-ttu-id="5cbd7-178">Elle utilise la classe [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) qui fournit plusieurs façons différentes de créer le certificat.</span><span class="sxs-lookup"><span data-stu-id="5cbd7-178">It uses the [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) class which provides several different ways to create the certificate.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
