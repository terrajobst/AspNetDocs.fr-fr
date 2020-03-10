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
# <a name="authentication-and-authorization-for-signalr-hubs"></a><span data-ttu-id="7fb89-104">Authentification et autorisation pour SignalR Hubs</span><span class="sxs-lookup"><span data-stu-id="7fb89-104">Authentication and Authorization for SignalR Hubs</span></span>

<span data-ttu-id="7fb89-105">de [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="7fb89-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="7fb89-106">Cette rubrique décrit comment restreindre les utilisateurs ou les rôles qui peuvent accéder aux méthodes de concentrateur.</span><span class="sxs-lookup"><span data-stu-id="7fb89-106">This topic describes how to restrict which users or roles can access hub methods.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="7fb89-107">Versions logicielles utilisées dans cette rubrique</span><span class="sxs-lookup"><span data-stu-id="7fb89-107">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="7fb89-108">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="7fb89-108">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="7fb89-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="7fb89-109">.NET 4.5</span></span>
> - <span data-ttu-id="7fb89-110">Signalr version 2</span><span class="sxs-lookup"><span data-stu-id="7fb89-110">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="7fb89-111">Versions précédentes de cette rubrique</span><span class="sxs-lookup"><span data-stu-id="7fb89-111">Previous versions of this topic</span></span>
>
> <span data-ttu-id="7fb89-112">Pour plus d’informations sur les versions antérieures de Signalr, consultez [versions antérieures de signalr](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="7fb89-112">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="7fb89-113">Questions et commentaires</span><span class="sxs-lookup"><span data-stu-id="7fb89-113">Questions and comments</span></span>
>
> <span data-ttu-id="7fb89-114">N’hésitez pas à nous faire part de vos commentaires sur la façon dont vous aimez ce didacticiel et sur ce que nous pourrions améliorer dans les commentaires en bas de la page.</span><span class="sxs-lookup"><span data-stu-id="7fb89-114">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="7fb89-115">Si vous avez des questions qui ne sont pas directement liées au didacticiel, vous pouvez les poster sur le [forum ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="7fb89-115">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="7fb89-116">Présentation</span><span class="sxs-lookup"><span data-stu-id="7fb89-116">Overview</span></span>

<span data-ttu-id="7fb89-117">Cette rubrique contient les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="7fb89-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="7fb89-118">Autorisation d’attribut</span><span class="sxs-lookup"><span data-stu-id="7fb89-118">Authorize attribute</span></span>](#authorizeattribute)
- [<span data-ttu-id="7fb89-119">Exiger l’authentification pour tous les hubs</span><span class="sxs-lookup"><span data-stu-id="7fb89-119">Require authentication for all hubs</span></span>](#requireauth)
- [<span data-ttu-id="7fb89-120">Autorisation personnalisée</span><span class="sxs-lookup"><span data-stu-id="7fb89-120">Customized authorization</span></span>](#custom)
- [<span data-ttu-id="7fb89-121">Transmettre les informations d’authentification aux clients</span><span class="sxs-lookup"><span data-stu-id="7fb89-121">Pass authentication information to clients</span></span>](#passauth)
- [<span data-ttu-id="7fb89-122">Options d’authentification pour les clients .NET</span><span class="sxs-lookup"><span data-stu-id="7fb89-122">Authentication options for .NET clients</span></span>](#authoptions)

    - [<span data-ttu-id="7fb89-123">Cookie avec l’authentification par formulaire</span><span class="sxs-lookup"><span data-stu-id="7fb89-123">Cookie with Forms Authentication</span></span>](#cookie)
    - [<span data-ttu-id="7fb89-124">Authentification Windows</span><span class="sxs-lookup"><span data-stu-id="7fb89-124">Windows Authentication</span></span>](#windows)
    - [<span data-ttu-id="7fb89-125">En-tête de connexion</span><span class="sxs-lookup"><span data-stu-id="7fb89-125">Connection header</span></span>](#header)
    - [<span data-ttu-id="7fb89-126">Certificate</span><span class="sxs-lookup"><span data-stu-id="7fb89-126">Certificate</span></span>](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a><span data-ttu-id="7fb89-127">Autorisation d’attribut</span><span class="sxs-lookup"><span data-stu-id="7fb89-127">Authorize attribute</span></span>

<span data-ttu-id="7fb89-128">Signalr fournit l’attribut [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) pour spécifier les utilisateurs ou les rôles qui ont accès à un concentrateur ou à une méthode.</span><span class="sxs-lookup"><span data-stu-id="7fb89-128">SignalR provides the [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users or roles have access to a hub or method.</span></span> <span data-ttu-id="7fb89-129">Cet attribut se trouve dans l’espace de noms `Microsoft.AspNet.SignalR`.</span><span class="sxs-lookup"><span data-stu-id="7fb89-129">This attribute is located in the `Microsoft.AspNet.SignalR` namespace.</span></span> <span data-ttu-id="7fb89-130">Vous appliquez l’attribut `Authorize` à un Hub ou à des méthodes particulières dans un concentrateur.</span><span class="sxs-lookup"><span data-stu-id="7fb89-130">You apply the `Authorize` attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="7fb89-131">Lorsque vous appliquez l’attribut `Authorize` à une classe de concentrateur, l’exigence d’autorisation spécifiée est appliquée à toutes les méthodes dans le concentrateur.</span><span class="sxs-lookup"><span data-stu-id="7fb89-131">When you apply the `Authorize` attribute to a hub class, the specified authorization requirement is applied to all of the methods in the hub.</span></span> <span data-ttu-id="7fb89-132">Cette rubrique fournit des exemples des différents types d’exigences d’autorisation que vous pouvez appliquer.</span><span class="sxs-lookup"><span data-stu-id="7fb89-132">This topic provides examples of the different types of authorization requirements that you can apply.</span></span> <span data-ttu-id="7fb89-133">Sans l’attribut `Authorize`, un client connecté peut accéder à n’importe quelle méthode publique sur le concentrateur.</span><span class="sxs-lookup"><span data-stu-id="7fb89-133">Without the `Authorize` attribute, a connected client can access any public method on the hub.</span></span>

<span data-ttu-id="7fb89-134">Si vous avez défini un rôle nommé « admin » dans votre application Web, vous pouvez spécifier que seuls les utilisateurs de ce rôle peuvent accéder à un concentrateur à l’aide du code suivant.</span><span class="sxs-lookup"><span data-stu-id="7fb89-134">If you have defined a role named "Admin" in your web application, you could specify that only users in that role can access a hub with the following code.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

<span data-ttu-id="7fb89-135">Vous pouvez aussi spécifier qu’un concentrateur contient une méthode qui est disponible pour tous les utilisateurs, et une deuxième méthode qui est uniquement disponible pour les utilisateurs authentifiés, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="7fb89-135">Or, you can specify that a hub contains one method that is available to all users, and a second method that is only available to authenticated users, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

<span data-ttu-id="7fb89-136">Les exemples suivants répondent à différents scénarios d’autorisation :</span><span class="sxs-lookup"><span data-stu-id="7fb89-136">The following examples address different authorization scenarios:</span></span>

- <span data-ttu-id="7fb89-137">`[Authorize]` : uniquement les utilisateurs authentifiés</span><span class="sxs-lookup"><span data-stu-id="7fb89-137">`[Authorize]` – only authenticated users</span></span>
- <span data-ttu-id="7fb89-138">`[Authorize(Roles = "Admin,Manager")]` : seuls les utilisateurs authentifiés dans les rôles spécifiés</span><span class="sxs-lookup"><span data-stu-id="7fb89-138">`[Authorize(Roles = "Admin,Manager")]` – only authenticated users in the specified roles</span></span>
- <span data-ttu-id="7fb89-139">`[Authorize(Users = "user1,user2")]` : seuls les utilisateurs authentifiés avec les noms d’utilisateurs spécifiés</span><span class="sxs-lookup"><span data-stu-id="7fb89-139">`[Authorize(Users = "user1,user2")]` – only authenticated users with the specified user names</span></span>
- <span data-ttu-id="7fb89-140">`[Authorize(RequireOutgoing=false)]` : seuls les utilisateurs authentifiés peuvent appeler le Hub, mais les appels à partir du serveur vers les clients ne sont pas limités par l’autorisation, par exemple, lorsque seuls certains utilisateurs peuvent envoyer un message, mais que les autres peuvent recevoir le message.</span><span class="sxs-lookup"><span data-stu-id="7fb89-140">`[Authorize(RequireOutgoing=false)]` – only authenticated users can invoke the hub, but calls from the server back to clients are not limited by authorization, such as, when only certain users can send a message but all others can receive the message.</span></span> <span data-ttu-id="7fb89-141">La propriété RequireOutgoing peut uniquement être appliquée à l’ensemble du concentrateur, et non à des méthodes individuelles dans le concentrateur.</span><span class="sxs-lookup"><span data-stu-id="7fb89-141">The RequireOutgoing property can only be applied to the entire hub, not on individuals methods within the hub.</span></span> <span data-ttu-id="7fb89-142">Quand RequireOutgoing n’est pas défini sur false, seuls les utilisateurs qui satisfont à l’exigence d’autorisation sont appelés à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="7fb89-142">When RequireOutgoing is not set to false, only users that meet the authorization requirement are called from the server.</span></span>

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a><span data-ttu-id="7fb89-143">Exiger l’authentification pour tous les hubs</span><span class="sxs-lookup"><span data-stu-id="7fb89-143">Require authentication for all hubs</span></span>

<span data-ttu-id="7fb89-144">Vous pouvez exiger l’authentification pour tous les concentrateurs et méthodes de concentrateur dans votre application en appelant la méthode [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="7fb89-144">You can require authentication for all hubs and hub methods in your application by calling the [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="7fb89-145">Vous pouvez utiliser cette méthode lorsque vous avez plusieurs hubs et que vous souhaitez appliquer une exigence d’authentification pour tous.</span><span class="sxs-lookup"><span data-stu-id="7fb89-145">You might use this method when you have multiple hubs and want to enforce an authentication requirement for all of them.</span></span> <span data-ttu-id="7fb89-146">Avec cette méthode, vous ne pouvez pas spécifier les conditions requises pour le rôle, l’utilisateur ou l’autorisation sortante.</span><span class="sxs-lookup"><span data-stu-id="7fb89-146">With this method, you cannot specify requirements for role, user, or outgoing authorization.</span></span> <span data-ttu-id="7fb89-147">Vous ne pouvez spécifier que l’accès aux méthodes de concentrateur est limité aux utilisateurs authentifiés.</span><span class="sxs-lookup"><span data-stu-id="7fb89-147">You can only specify that access to the hub methods is restricted to authenticated users.</span></span> <span data-ttu-id="7fb89-148">Toutefois, vous pouvez toujours appliquer l’attribut Authorize aux hubs ou aux méthodes pour spécifier des exigences supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="7fb89-148">However, you can still apply the Authorize attribute to hubs or methods to specify additional requirements.</span></span> <span data-ttu-id="7fb89-149">Toute spécification que vous spécifiez dans un attribut est ajoutée à l’exigence de base de l’authentification.</span><span class="sxs-lookup"><span data-stu-id="7fb89-149">Any requirement you specify in an attribute is added to the basic requirement of authentication.</span></span>

<span data-ttu-id="7fb89-150">L’exemple suivant montre un fichier de démarrage qui restreint toutes les méthodes de concentrateur aux utilisateurs authentifiés.</span><span class="sxs-lookup"><span data-stu-id="7fb89-150">The following example shows a Startup file which restricts all hub methods to authenticated users.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

<span data-ttu-id="7fb89-151">Si vous appelez la méthode `RequireAuthentication()` une fois qu’une demande Signalr a été traitée, Signalr lèvera une exception `InvalidOperationException`.</span><span class="sxs-lookup"><span data-stu-id="7fb89-151">If you call the `RequireAuthentication()` method after a SignalR request has been processed, SignalR will throw a `InvalidOperationException` exception.</span></span> <span data-ttu-id="7fb89-152">Signalr lève cette exception, car vous ne pouvez pas ajouter un module au HubPipeline une fois que le pipeline a été appelé.</span><span class="sxs-lookup"><span data-stu-id="7fb89-152">SignalR throws this exception because you cannot add a module to the HubPipeline after the pipeline has been invoked.</span></span> <span data-ttu-id="7fb89-153">L’exemple précédent montre l’appel de la méthode `RequireAuthentication` dans la méthode `Configuration` qui est exécutée une fois avant la première demande.</span><span class="sxs-lookup"><span data-stu-id="7fb89-153">The previous example shows calling the `RequireAuthentication` method in the `Configuration` method which is executed one time prior to handling the first request.</span></span>

<a id="custom"></a>

## <a name="customized-authorization"></a><span data-ttu-id="7fb89-154">Autorisation personnalisée</span><span class="sxs-lookup"><span data-stu-id="7fb89-154">Customized authorization</span></span>

<span data-ttu-id="7fb89-155">Si vous avez besoin de personnaliser la façon dont l’autorisation est déterminée, vous pouvez créer une classe qui dérive de `AuthorizeAttribute` et substituer la méthode [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) .</span><span class="sxs-lookup"><span data-stu-id="7fb89-155">If you need to customize how authorization is determined, you can create a class that derives from `AuthorizeAttribute` and override the [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) method.</span></span> <span data-ttu-id="7fb89-156">Pour chaque demande, Signalr appelle cette méthode pour déterminer si l’utilisateur est autorisé à terminer la demande.</span><span class="sxs-lookup"><span data-stu-id="7fb89-156">For each request, SignalR invokes this method to determine whether the user is authorized to complete the request.</span></span> <span data-ttu-id="7fb89-157">Dans la méthode substituée, vous fournissez la logique nécessaire pour votre scénario d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="7fb89-157">In the overridden method, you provide the necessary logic for your authorization scenario.</span></span> <span data-ttu-id="7fb89-158">L’exemple suivant montre comment appliquer l’autorisation par le biais d’une identité basée sur les revendications.</span><span class="sxs-lookup"><span data-stu-id="7fb89-158">The following example shows how to enforce authorization through claims-based identity.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a><span data-ttu-id="7fb89-159">Transmettre les informations d’authentification aux clients</span><span class="sxs-lookup"><span data-stu-id="7fb89-159">Pass authentication information to clients</span></span>

<span data-ttu-id="7fb89-160">Vous devrez peut-être utiliser les informations d’authentification dans le code qui s’exécute sur le client.</span><span class="sxs-lookup"><span data-stu-id="7fb89-160">You may need to use authentication information in the code that runs on the client.</span></span> <span data-ttu-id="7fb89-161">Vous transmettez les informations requises lors de l’appel des méthodes sur le client.</span><span class="sxs-lookup"><span data-stu-id="7fb89-161">You pass the required information when calling the methods on the client.</span></span> <span data-ttu-id="7fb89-162">Par exemple, une méthode d’application de conversation peut passer en tant que paramètre le nom d’utilisateur de la personne qui publie un message, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="7fb89-162">For example, a chat application method could pass as a parameter the user name of the person posting a message, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

<span data-ttu-id="7fb89-163">Vous pouvez aussi créer un objet pour représenter les informations d’authentification et passer cet objet en tant que paramètre, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="7fb89-163">Or, you can create an object to represent the authentication information and pass that object as a parameter, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

<span data-ttu-id="7fb89-164">Vous ne devez jamais transmettre l’ID de connexion d’un client à d’autres clients, car un utilisateur malveillant pourrait l’utiliser pour imiter une requête de ce client.</span><span class="sxs-lookup"><span data-stu-id="7fb89-164">You should never pass one client's connection id to other clients, as a malicious user could use it to mimic a request from that client.</span></span>

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a><span data-ttu-id="7fb89-165">Options d’authentification pour les clients .NET</span><span class="sxs-lookup"><span data-stu-id="7fb89-165">Authentication options for .NET clients</span></span>

<span data-ttu-id="7fb89-166">Si vous disposez d’un client .NET, tel qu’une application console, qui interagit avec un concentrateur qui est limité aux utilisateurs authentifiés, vous pouvez transmettre les informations d’identification d’authentification dans un cookie, l’en-tête de connexion ou un certificat.</span><span class="sxs-lookup"><span data-stu-id="7fb89-166">When you have a .NET client, such as a console app, which interacts with a hub that is limited to authenticated users, you can pass the authentication credentials in a cookie, the connection header, or a certificate.</span></span> <span data-ttu-id="7fb89-167">Les exemples de cette section montrent comment utiliser ces différentes méthodes pour authentifier un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="7fb89-167">The examples in this section show how to use those different methods for authenticating a user.</span></span> <span data-ttu-id="7fb89-168">Ce ne sont pas des applications Signalr entièrement fonctionnelles.</span><span class="sxs-lookup"><span data-stu-id="7fb89-168">They are not fully-functional SignalR apps.</span></span> <span data-ttu-id="7fb89-169">Pour plus d’informations sur les clients .NET avec Signalr, consultez [Guide de l’API hubs-client .net](../guide-to-the-api/hubs-api-guide-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="7fb89-169">For more information about .NET clients with SignalR, see [Hubs API Guide - .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span></span>

<a id="cookie"></a>

### <a name="cookie"></a><span data-ttu-id="7fb89-170">Cookie</span><span class="sxs-lookup"><span data-stu-id="7fb89-170">Cookie</span></span>

<span data-ttu-id="7fb89-171">Lorsque votre client .NET interagit avec un concentrateur qui utilise l’authentification par formulaire ASP.NET, vous devez définir manuellement le cookie d’authentification sur la connexion.</span><span class="sxs-lookup"><span data-stu-id="7fb89-171">When your .NET client interacts with a hub that uses ASP.NET Forms Authentication, you will need to manually set the authentication cookie on the connection.</span></span> <span data-ttu-id="7fb89-172">Vous ajoutez le cookie à la propriété `CookieContainer` sur l’objet [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) .</span><span class="sxs-lookup"><span data-stu-id="7fb89-172">You add the cookie to the `CookieContainer` property on the [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) object.</span></span> <span data-ttu-id="7fb89-173">L’exemple suivant montre une application console qui récupère un cookie d’authentification à partir d’une page Web et ajoute ce cookie à la connexion.</span><span class="sxs-lookup"><span data-stu-id="7fb89-173">The following example shows a console app that retrieves an authentication cookie from a web page and adds that cookie to the connection.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

<span data-ttu-id="7fb89-174">L’application console publie les informations d’identification dans <strong>www.contoso.com/RemoteLogin</strong> qui peuvent faire référence à une page vide contenant le fichier code-behind suivant.</span><span class="sxs-lookup"><span data-stu-id="7fb89-174">The console app posts the credentials to <strong>www.contoso.com/RemoteLogin</strong> which could refer to an empty page that contains the following code-behind file.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a><span data-ttu-id="7fb89-175">Authentification Windows</span><span class="sxs-lookup"><span data-stu-id="7fb89-175">Windows authentication</span></span>

<span data-ttu-id="7fb89-176">Lorsque vous utilisez l’authentification Windows, vous pouvez passer les informations d’identification de l’utilisateur actuel à l’aide de la propriété [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) .</span><span class="sxs-lookup"><span data-stu-id="7fb89-176">When using Windows authentication, you can pass the current user's credentials by using the [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) property.</span></span> <span data-ttu-id="7fb89-177">Vous définissez les informations d’identification pour la connexion à la valeur de DefaultCredentials.</span><span class="sxs-lookup"><span data-stu-id="7fb89-177">You set the credentials for the connection to the value of the DefaultCredentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a><span data-ttu-id="7fb89-178">En-tête de connexion</span><span class="sxs-lookup"><span data-stu-id="7fb89-178">Connection header</span></span>

<span data-ttu-id="7fb89-179">Si votre application n’utilise pas de cookies, vous pouvez transmettre les informations utilisateur dans l’en-tête de connexion.</span><span class="sxs-lookup"><span data-stu-id="7fb89-179">If your application is not using cookies, you can pass user information in the connection header.</span></span> <span data-ttu-id="7fb89-180">Par exemple, vous pouvez passer un jeton dans l’en-tête de connexion.</span><span class="sxs-lookup"><span data-stu-id="7fb89-180">For example, you can pass a token in the connection header.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

<span data-ttu-id="7fb89-181">Ensuite, dans le Hub, vous devez vérifier le jeton de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="7fb89-181">Then, in the hub, you would verify the user's token.</span></span>

<a id="certificate"></a>

### <a name="certificate"></a><span data-ttu-id="7fb89-182">Certificat</span><span class="sxs-lookup"><span data-stu-id="7fb89-182">Certificate</span></span>

<span data-ttu-id="7fb89-183">Vous pouvez transmettre un certificat client pour vérifier l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="7fb89-183">You can pass a client certificate to verify the user.</span></span> <span data-ttu-id="7fb89-184">Vous ajoutez le certificat lors de la création de la connexion.</span><span class="sxs-lookup"><span data-stu-id="7fb89-184">You add the certificate when creating the connection.</span></span> <span data-ttu-id="7fb89-185">L’exemple suivant montre uniquement comment ajouter un certificat client à la connexion ; elle n’affiche pas l’application console complète.</span><span class="sxs-lookup"><span data-stu-id="7fb89-185">The following example shows only how to add a client certificate to the connection; it does not show the full console app.</span></span> <span data-ttu-id="7fb89-186">Elle utilise la classe [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) qui fournit plusieurs façons différentes de créer le certificat.</span><span class="sxs-lookup"><span data-stu-id="7fb89-186">It uses the [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) class which provides several different ways to create the certificate.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
