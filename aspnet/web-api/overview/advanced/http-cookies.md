---
uid: web-api/overview/advanced/http-cookies
title: Cookies HTTP dans API Web ASP.NET-ASP.NET 4. x
author: MikeWasson
description: Décrit comment envoyer et recevoir des cookies HTTP dans l’API Web pour ASP.NET 4. x.
ms.author: riande
ms.date: 09/17/2012
ms.custom: seoapril2019
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 8ca26ff6776daa13bc4f8b06c2eba61afcfefba2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557686"
---
# <a name="http-cookies-in-aspnet-web-api"></a><span data-ttu-id="6d4c5-103">Cookies HTTP dans l’API web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6d4c5-103">HTTP Cookies in ASP.NET Web API</span></span>

<span data-ttu-id="6d4c5-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6d4c5-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="6d4c5-105">Cette rubrique explique comment envoyer et recevoir des cookies HTTP dans l’API Web.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-105">This topic describes how to send and receive HTTP cookies in Web API.</span></span>

## <a name="background-on-http-cookies"></a><span data-ttu-id="6d4c5-106">Arrière-plan sur les cookies HTTP</span><span class="sxs-lookup"><span data-stu-id="6d4c5-106">Background on HTTP Cookies</span></span>

<span data-ttu-id="6d4c5-107">Cette section donne un bref aperçu de la façon dont les cookies sont implémentés au niveau HTTP.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-107">This section gives a brief overview of how cookies are implemented at the HTTP level.</span></span> <span data-ttu-id="6d4c5-108">Pour plus d’informations, consultez la [RFC 6265](http://tools.ietf.org/html/rfc6265).</span><span class="sxs-lookup"><span data-stu-id="6d4c5-108">For details, consult [RFC 6265](http://tools.ietf.org/html/rfc6265).</span></span>

<span data-ttu-id="6d4c5-109">Un cookie est un élément de données qu’un serveur envoie dans la réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-109">A cookie is a piece of data that a server sends in the HTTP response.</span></span> <span data-ttu-id="6d4c5-110">Le client stocke (éventuellement) le cookie et le retourne aux demandes suivantes.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-110">The client (optionally) stores the cookie and returns it on subsequent requests.</span></span> <span data-ttu-id="6d4c5-111">Cela permet au client et au serveur de partager l’État.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-111">This allows the client and server to share state.</span></span> <span data-ttu-id="6d4c5-112">Pour définir un cookie, le serveur comprend un en-tête Set-Cookie dans la réponse.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-112">To set a cookie, the server includes a Set-Cookie header in the response.</span></span> <span data-ttu-id="6d4c5-113">Le format d’un cookie est une paire nom-valeur, avec des attributs facultatifs.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-113">The format of a cookie is a name-value pair, with optional attributes.</span></span> <span data-ttu-id="6d4c5-114">Exemple :</span><span class="sxs-lookup"><span data-stu-id="6d4c5-114">For example:</span></span>

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

<span data-ttu-id="6d4c5-115">Voici un exemple avec des attributs :</span><span class="sxs-lookup"><span data-stu-id="6d4c5-115">Here is an example with attributes:</span></span>

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

<span data-ttu-id="6d4c5-116">Pour retourner un cookie au serveur, le client comprend un en-tête de cookie dans les demandes ultérieures.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-116">To return a cookie to the server, the client includes a Cookie header in later requests.</span></span>

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

<span data-ttu-id="6d4c5-117">Une réponse HTTP peut inclure plusieurs en-têtes set-cookie.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-117">An HTTP response can include multiple Set-Cookie headers.</span></span>

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

<span data-ttu-id="6d4c5-118">Le client retourne plusieurs cookies à l’aide d’un seul en-tête de cookie.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-118">The client returns multiple cookies using a single Cookie header.</span></span>

[!code-console[Main](http-cookies/samples/sample5.cmd)]

<span data-ttu-id="6d4c5-119">L’étendue et la durée d’un cookie sont contrôlées par les attributs suivants dans l’en-tête Set-Cookie :</span><span class="sxs-lookup"><span data-stu-id="6d4c5-119">The scope and duration of a cookie are controlled by following attributes in the Set-Cookie header:</span></span>

- <span data-ttu-id="6d4c5-120">**Domain**: indique au client le domaine qui doit recevoir le cookie.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-120">**Domain**: Tells the client which domain should receive the cookie.</span></span> <span data-ttu-id="6d4c5-121">Par exemple, si le domaine est « example.com », le client retourne le cookie à chaque sous-domaine de example.com.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-121">For example, if the domain is "example.com", the client returns the cookie to every subdomain of example.com.</span></span> <span data-ttu-id="6d4c5-122">S’il n’est pas spécifié, le domaine est le serveur d’origine.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-122">If not specified, the domain is the origin server.</span></span>
- <span data-ttu-id="6d4c5-123">**Path**: limite le cookie au chemin d’accès spécifié dans le domaine.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-123">**Path**: Restricts the cookie to the specified path within the domain.</span></span> <span data-ttu-id="6d4c5-124">S’il n’est pas spécifié, le chemin d’accès de l’URI de la demande est utilisé.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-124">If not specified, the path of the request URI is used.</span></span>
- <span data-ttu-id="6d4c5-125">**Expires**: définit une date d’expiration pour le cookie.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-125">**Expires**: Sets an expiration date for the cookie.</span></span> <span data-ttu-id="6d4c5-126">Le client supprime le cookie lorsqu’il expire.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-126">The client deletes the cookie when it expires.</span></span>
- <span data-ttu-id="6d4c5-127">**Max-age**: définit l’âge maximal du cookie.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-127">**Max-Age**: Sets the maximum age for the cookie.</span></span> <span data-ttu-id="6d4c5-128">Le client supprime le cookie lorsqu’il atteint l’âge maximal.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-128">The client deletes the cookie when it reaches the maximum age.</span></span>

<span data-ttu-id="6d4c5-129">Si `Expires` et `Max-Age` sont définis, `Max-Age` est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-129">If both `Expires` and `Max-Age` are set, `Max-Age` takes precedence.</span></span> <span data-ttu-id="6d4c5-130">Si aucun n’est défini, le client supprime le cookie lorsque la session active se termine.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-130">If neither is set, the client deletes the cookie when the current session ends.</span></span> <span data-ttu-id="6d4c5-131">(La signification exacte de « session » est déterminée par l’agent utilisateur.)</span><span class="sxs-lookup"><span data-stu-id="6d4c5-131">(The exact meaning of "session" is determined by the user-agent.)</span></span>

<span data-ttu-id="6d4c5-132">Toutefois, sachez que les clients peuvent ignorer les cookies.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-132">However, be aware that clients may ignore cookies.</span></span> <span data-ttu-id="6d4c5-133">Par exemple, un utilisateur peut désactiver les cookies pour des raisons de confidentialité.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-133">For example, a user might disable cookies for privacy reasons.</span></span> <span data-ttu-id="6d4c5-134">Les clients peuvent supprimer des cookies avant leur expiration ou limiter le nombre de cookies stockés.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-134">Clients may delete cookies before they expire, or limit the number of cookies stored.</span></span> <span data-ttu-id="6d4c5-135">Pour des raisons de confidentialité, les clients rejettent souvent des cookies « tiers », où le domaine ne correspond pas au serveur d’origine.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-135">For privacy reasons, clients often reject "third party" cookies, where the domain does not match the origin server.</span></span> <span data-ttu-id="6d4c5-136">En bref, le serveur ne doit pas s’appuyer sur la récupération des cookies qu’il définit.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-136">In short, the server should not rely on getting back the cookies that it sets.</span></span>

## <a name="cookies-in-web-api"></a><span data-ttu-id="6d4c5-137">Cookies dans l’API Web</span><span class="sxs-lookup"><span data-stu-id="6d4c5-137">Cookies in Web API</span></span>

<span data-ttu-id="6d4c5-138">Pour ajouter un cookie à une réponse HTTP, créez une instance **CookieHeaderValue** qui représente le cookie.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-138">To add a cookie to an HTTP response, create a **CookieHeaderValue** instance that represents the cookie.</span></span> <span data-ttu-id="6d4c5-139">Appelez ensuite la méthode d’extension **AddCookies** , qui est définie dans le **System .net. http. Classe HttpResponseHeadersExtensions** , pour ajouter le cookie.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-139">Then call the **AddCookies** extension method, which is defined in the **System.Net.Http. HttpResponseHeadersExtensions** class, to add the cookie.</span></span>

<span data-ttu-id="6d4c5-140">Par exemple, le code suivant ajoute un cookie au sein d’une action de contrôleur :</span><span class="sxs-lookup"><span data-stu-id="6d4c5-140">For example, the following code adds a cookie within a controller action:</span></span>

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

<span data-ttu-id="6d4c5-141">Notez que **AddCookies** prend un tableau d’instances **CookieHeaderValue** .</span><span class="sxs-lookup"><span data-stu-id="6d4c5-141">Notice that **AddCookies** takes an array of **CookieHeaderValue** instances.</span></span>

<span data-ttu-id="6d4c5-142">Pour extraire les cookies d’une demande du client, appelez la méthode **GetCookies** :</span><span class="sxs-lookup"><span data-stu-id="6d4c5-142">To extract the cookies from a client request, call the **GetCookies** method:</span></span>

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

<span data-ttu-id="6d4c5-143">Un **CookieHeaderValue** contient une collection d’instances **CookieState** .</span><span class="sxs-lookup"><span data-stu-id="6d4c5-143">A **CookieHeaderValue** contains a collection of **CookieState** instances.</span></span> <span data-ttu-id="6d4c5-144">Chaque **CookieState** représente un cookie.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-144">Each **CookieState** represents one cookie.</span></span> <span data-ttu-id="6d4c5-145">Utilisez la méthode d’indexeur pour obtenir un **CookieState** par nom, comme indiqué.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-145">Use the indexer method to get a **CookieState** by name, as shown.</span></span>

## <a name="structured-cookie-data"></a><span data-ttu-id="6d4c5-146">Données de cookie structurées</span><span class="sxs-lookup"><span data-stu-id="6d4c5-146">Structured Cookie Data</span></span>

<span data-ttu-id="6d4c5-147">De nombreux navigateurs limitent le nombre de cookies&#8212;qu’ils stockent à la fois le nombre total et le nombre par domaine.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-147">Many browsers limit how many cookies they will store&#8212;both the total number, and the number per domain.</span></span> <span data-ttu-id="6d4c5-148">Par conséquent, il peut être utile de placer des données structurées dans un cookie unique, au lieu de définir plusieurs cookies.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-148">Therefore, it can be useful to put structured data into a single cookie, instead of setting multiple cookies.</span></span>

> [!NOTE]
> <span data-ttu-id="6d4c5-149">La RFC 6265 ne définit pas la structure des données de cookie.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-149">RFC 6265 does not define the structure of cookie data.</span></span>

<span data-ttu-id="6d4c5-150">À l’aide de la classe **CookieHeaderValue** , vous pouvez passer une liste de paires nom-valeur pour les données de cookie.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-150">Using the **CookieHeaderValue** class, you can pass a list of name-value pairs for the cookie data.</span></span> <span data-ttu-id="6d4c5-151">Ces paires nom-valeur sont encodées en tant que données de formulaire encodées URL dans l’en-tête Set-Cookie :</span><span class="sxs-lookup"><span data-stu-id="6d4c5-151">These name-value pairs are encoded as URL-encoded form data in the Set-Cookie header:</span></span>

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

<span data-ttu-id="6d4c5-152">Le code précédent génère l’en-tête Set-Cookie suivant :</span><span class="sxs-lookup"><span data-stu-id="6d4c5-152">The previous code produces the following Set-Cookie header:</span></span>

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

<span data-ttu-id="6d4c5-153">La classe **CookieState** fournit une méthode d’indexeur pour lire les sous-valeurs d’un cookie dans le message de demande :</span><span class="sxs-lookup"><span data-stu-id="6d4c5-153">The **CookieState** class provides an indexer method to read the sub-values from a cookie in the request message:</span></span>

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a><span data-ttu-id="6d4c5-154">Exemple : définir et récupérer des cookies dans un gestionnaire de messages</span><span class="sxs-lookup"><span data-stu-id="6d4c5-154">Example: Set and Retrieve Cookies in a Message Handler</span></span>

<span data-ttu-id="6d4c5-155">Les exemples précédents ont montré comment utiliser des cookies à partir d’un contrôleur d’API Web.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-155">The previous examples showed how to use cookies from within a Web API controller.</span></span> <span data-ttu-id="6d4c5-156">Une autre option consiste à utiliser des [gestionnaires de messages](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="6d4c5-156">Another option is to use [message handlers](http-message-handlers.md).</span></span> <span data-ttu-id="6d4c5-157">Les gestionnaires de messages sont appelés plus tôt dans le pipeline que les contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-157">Message handlers are invoked earlier in the pipeline than controllers.</span></span> <span data-ttu-id="6d4c5-158">Un gestionnaire de messages peut lire les cookies de la demande avant que la demande n’atteigne le contrôleur, ou ajouter des cookies à la réponse après que le contrôleur a généré la réponse.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-158">A message handler can read cookies from the request before the request reaches the controller, or add cookies to the response after the controller generates the response.</span></span>

![](http-cookies/_static/image2.png)

<span data-ttu-id="6d4c5-159">Le code suivant illustre un gestionnaire de messages pour créer des ID de session.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-159">The following code shows a message handler for creating session IDs.</span></span> <span data-ttu-id="6d4c5-160">L’ID de session est stocké dans un cookie.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-160">The session ID is stored in a cookie.</span></span> <span data-ttu-id="6d4c5-161">Le gestionnaire vérifie la requête pour le cookie de session.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-161">The handler checks the request for the session cookie.</span></span> <span data-ttu-id="6d4c5-162">Si la demande n’inclut pas le cookie, le gestionnaire génère un nouvel ID de session.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-162">If the request does not include the cookie, the handler generates a new session ID.</span></span> <span data-ttu-id="6d4c5-163">Dans les deux cas, le gestionnaire stocke l’ID de session dans le conteneur de propriétés **HttpRequestMessage. Properties** .</span><span class="sxs-lookup"><span data-stu-id="6d4c5-163">In either case, the handler stores the session ID in the **HttpRequestMessage.Properties** property bag.</span></span> <span data-ttu-id="6d4c5-164">Il ajoute également le cookie de session à la réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-164">It also adds the session cookie to the HTTP response.</span></span>

<span data-ttu-id="6d4c5-165">Cette implémentation ne valide pas le fait que l’ID de session du client a été émis par le serveur.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-165">This implementation does not validate that the session ID from the client was actually issued by the server.</span></span> <span data-ttu-id="6d4c5-166">Ne l’utilisez pas comme une forme d’authentification !</span><span class="sxs-lookup"><span data-stu-id="6d4c5-166">Don't use it as a form of authentication!</span></span> <span data-ttu-id="6d4c5-167">Le point de l’exemple est de montrer la gestion des cookies HTTP.</span><span class="sxs-lookup"><span data-stu-id="6d4c5-167">The point of the example is to show HTTP cookie management.</span></span>

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

<span data-ttu-id="6d4c5-168">Un contrôleur peut obtenir l’ID de session à partir du jeu de propriétés **HttpRequestMessage. Properties** .</span><span class="sxs-lookup"><span data-stu-id="6d4c5-168">A controller can get the session ID from the **HttpRequestMessage.Properties** property bag.</span></span>

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
