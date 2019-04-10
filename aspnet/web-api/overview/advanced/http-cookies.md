---
uid: web-api/overview/advanced/http-cookies
title: Les Cookies HTTP dans l’API Web ASP.NET - ASP.NET 4.x
author: MikeWasson
description: Décrit comment envoyer et recevoir des cookies HTTP dans l’API Web pour ASP.NET 4.x.
ms.author: riande
ms.date: 09/17/2012
ms.custom: seoapril2019
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: cd6391582f05ab80c4bd45a455a2ce488d1186c1
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59418324"
---
# <a name="http-cookies-in-aspnet-web-api"></a><span data-ttu-id="ee164-103">Cookies HTTP dans l’API web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ee164-103">HTTP Cookies in ASP.NET Web API</span></span>

<span data-ttu-id="ee164-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ee164-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="ee164-105">Cette rubrique décrit comment envoyer et recevoir des cookies HTTP dans l’API Web.</span><span class="sxs-lookup"><span data-stu-id="ee164-105">This topic describes how to send and receive HTTP cookies in Web API.</span></span>

## <a name="background-on-http-cookies"></a><span data-ttu-id="ee164-106">En arrière-plan sur les Cookies HTTP</span><span class="sxs-lookup"><span data-stu-id="ee164-106">Background on HTTP Cookies</span></span>

<span data-ttu-id="ee164-107">Cette section donne un bref aperçu de la façon dont les cookies sont implémentés au niveau HTTP.</span><span class="sxs-lookup"><span data-stu-id="ee164-107">This section gives a brief overview of how cookies are implemented at the HTTP level.</span></span> <span data-ttu-id="ee164-108">Pour plus d’informations, consultez [RFC 6265](http://tools.ietf.org/html/rfc6265).</span><span class="sxs-lookup"><span data-stu-id="ee164-108">For details, consult [RFC 6265](http://tools.ietf.org/html/rfc6265).</span></span>

<span data-ttu-id="ee164-109">Un cookie est un élément de données qui envoie par un serveur dans la réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="ee164-109">A cookie is a piece of data that a server sends in the HTTP response.</span></span> <span data-ttu-id="ee164-110">Le client stocke le cookie (facultatif) et le retourne pour les demandes suivantes.</span><span class="sxs-lookup"><span data-stu-id="ee164-110">The client (optionally) stores the cookie and returns it on subsequent requests.</span></span> <span data-ttu-id="ee164-111">Ainsi, le client et le serveur partager l’état.</span><span class="sxs-lookup"><span data-stu-id="ee164-111">This allows the client and server to share state.</span></span> <span data-ttu-id="ee164-112">Pour définir un cookie, le serveur inclut un en-tête Set-Cookie dans la réponse.</span><span class="sxs-lookup"><span data-stu-id="ee164-112">To set a cookie, the server includes a Set-Cookie header in the response.</span></span> <span data-ttu-id="ee164-113">Le format d’un cookie est une paire nom-valeur, avec des attributs facultatifs.</span><span class="sxs-lookup"><span data-stu-id="ee164-113">The format of a cookie is a name-value pair, with optional attributes.</span></span> <span data-ttu-id="ee164-114">Exemple :</span><span class="sxs-lookup"><span data-stu-id="ee164-114">For example:</span></span>

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

<span data-ttu-id="ee164-115">Voici un exemple avec des attributs :</span><span class="sxs-lookup"><span data-stu-id="ee164-115">Here is an example with attributes:</span></span>

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

<span data-ttu-id="ee164-116">Pour retourner un cookie au serveur, le client inclut un en-tête de Cookie dans les demandes ultérieures.</span><span class="sxs-lookup"><span data-stu-id="ee164-116">To return a cookie to the server, the client includes a Cookie header in later requests.</span></span>

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

<span data-ttu-id="ee164-117">Une réponse HTTP peut inclure plusieurs en-têtes Set-Cookie.</span><span class="sxs-lookup"><span data-stu-id="ee164-117">An HTTP response can include multiple Set-Cookie headers.</span></span>

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

<span data-ttu-id="ee164-118">Le client retourne plusieurs cookies à l’aide d’un en-tête de Cookie unique.</span><span class="sxs-lookup"><span data-stu-id="ee164-118">The client returns multiple cookies using a single Cookie header.</span></span>

[!code-console[Main](http-cookies/samples/sample5.cmd)]

<span data-ttu-id="ee164-119">L’étendue et la durée d’un cookie sont contrôlées par les attributs suivants dans l’en-tête Set-Cookie :</span><span class="sxs-lookup"><span data-stu-id="ee164-119">The scope and duration of a cookie are controlled by following attributes in the Set-Cookie header:</span></span>

- <span data-ttu-id="ee164-120">**Domaine**: Indique au client de domaine qui doit recevoir le cookie.</span><span class="sxs-lookup"><span data-stu-id="ee164-120">**Domain**: Tells the client which domain should receive the cookie.</span></span> <span data-ttu-id="ee164-121">Par exemple, si le domaine est « example.com », le client retourne le cookie à chaque sous-domaine de example.com.</span><span class="sxs-lookup"><span data-stu-id="ee164-121">For example, if the domain is "example.com", the client returns the cookie to every subdomain of example.com.</span></span> <span data-ttu-id="ee164-122">Si non spécifié, le domaine est le serveur d’origine.</span><span class="sxs-lookup"><span data-stu-id="ee164-122">If not specified, the domain is the origin server.</span></span>
- <span data-ttu-id="ee164-123">**Chemin d’accès**: Restreint le cookie pour le chemin d’accès spécifié au sein du domaine.</span><span class="sxs-lookup"><span data-stu-id="ee164-123">**Path**: Restricts the cookie to the specified path within the domain.</span></span> <span data-ttu-id="ee164-124">Si non spécifié, le chemin d’accès de l’URI de demande est utilisé.</span><span class="sxs-lookup"><span data-stu-id="ee164-124">If not specified, the path of the request URI is used.</span></span>
- <span data-ttu-id="ee164-125">**Expiration**: Définit une date d’expiration du cookie.</span><span class="sxs-lookup"><span data-stu-id="ee164-125">**Expires**: Sets an expiration date for the cookie.</span></span> <span data-ttu-id="ee164-126">Le client supprime le cookie expiré.</span><span class="sxs-lookup"><span data-stu-id="ee164-126">The client deletes the cookie when it expires.</span></span>
- <span data-ttu-id="ee164-127">**Max-Age**: Définit l’âge maximal pour le cookie.</span><span class="sxs-lookup"><span data-stu-id="ee164-127">**Max-Age**: Sets the maximum age for the cookie.</span></span> <span data-ttu-id="ee164-128">Le client supprime le cookie lorsqu’il atteint l’âge maximal.</span><span class="sxs-lookup"><span data-stu-id="ee164-128">The client deletes the cookie when it reaches the maximum age.</span></span>

<span data-ttu-id="ee164-129">Si les deux `Expires` et `Max-Age` sont définies, `Max-Age` est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="ee164-129">If both `Expires` and `Max-Age` are set, `Max-Age` takes precedence.</span></span> <span data-ttu-id="ee164-130">Si aucun n’est défini, le client supprime le cookie lorsque la session active se termine.</span><span class="sxs-lookup"><span data-stu-id="ee164-130">If neither is set, the client deletes the cookie when the current session ends.</span></span> <span data-ttu-id="ee164-131">(La signification exacte de « session » est déterminée par l’agent utilisateur).</span><span class="sxs-lookup"><span data-stu-id="ee164-131">(The exact meaning of "session" is determined by the user-agent.)</span></span>

<span data-ttu-id="ee164-132">Toutefois, n’oubliez pas que les clients peuvent ignorer les cookies.</span><span class="sxs-lookup"><span data-stu-id="ee164-132">However, be aware that clients may ignore cookies.</span></span> <span data-ttu-id="ee164-133">Par exemple, un utilisateur peut désactiver les cookies pour des raisons de confidentialité.</span><span class="sxs-lookup"><span data-stu-id="ee164-133">For example, a user might disable cookies for privacy reasons.</span></span> <span data-ttu-id="ee164-134">Les clients peuvent supprimer les cookies avant leur expiration ou limitent le nombre de cookies stockés.</span><span class="sxs-lookup"><span data-stu-id="ee164-134">Clients may delete cookies before they expire, or limit the number of cookies stored.</span></span> <span data-ttu-id="ee164-135">Pour des raisons de confidentialité, les clients rejettent souvent des cookies de « tiers », où le domaine ne correspond pas au serveur d’origine.</span><span class="sxs-lookup"><span data-stu-id="ee164-135">For privacy reasons, clients often reject "third party" cookies, where the domain does not match the origin server.</span></span> <span data-ttu-id="ee164-136">En bref, le serveur ne doit pas s’appuient sur retour des cookies qu’il définit.</span><span class="sxs-lookup"><span data-stu-id="ee164-136">In short, the server should not rely on getting back the cookies that it sets.</span></span>

## <a name="cookies-in-web-api"></a><span data-ttu-id="ee164-137">Cookies dans l’API Web</span><span class="sxs-lookup"><span data-stu-id="ee164-137">Cookies in Web API</span></span>

<span data-ttu-id="ee164-138">Pour ajouter un cookie à une réponse HTTP, créez un **CookieHeaderValue** instance qui représente le cookie.</span><span class="sxs-lookup"><span data-stu-id="ee164-138">To add a cookie to an HTTP response, create a **CookieHeaderValue** instance that represents the cookie.</span></span> <span data-ttu-id="ee164-139">Appelez ensuite la **AddCookies** méthode d’extension, qui est défini dans le **System.Net.Http. HttpResponseHeadersExtensions** (classe), pour ajouter le cookie.</span><span class="sxs-lookup"><span data-stu-id="ee164-139">Then call the **AddCookies** extension method, which is defined in the **System.Net.Http. HttpResponseHeadersExtensions** class, to add the cookie.</span></span>

<span data-ttu-id="ee164-140">Par exemple, le code suivant ajoute un cookie au sein d’une action de contrôleur :</span><span class="sxs-lookup"><span data-stu-id="ee164-140">For example, the following code adds a cookie within a controller action:</span></span>

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

<span data-ttu-id="ee164-141">Notez que **AddCookies** prend un tableau de **CookieHeaderValue** instances.</span><span class="sxs-lookup"><span data-stu-id="ee164-141">Notice that **AddCookies** takes an array of **CookieHeaderValue** instances.</span></span>

<span data-ttu-id="ee164-142">Pour extraire les cookies à partir d’une demande du client, appelez le **GetCookies** méthode :</span><span class="sxs-lookup"><span data-stu-id="ee164-142">To extract the cookies from a client request, call the **GetCookies** method:</span></span>

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

<span data-ttu-id="ee164-143">Un **CookieHeaderValue** contient une collection de **CookieState** instances.</span><span class="sxs-lookup"><span data-stu-id="ee164-143">A **CookieHeaderValue** contains a collection of **CookieState** instances.</span></span> <span data-ttu-id="ee164-144">Chaque **CookieState** représente un seul petit gâteau.</span><span class="sxs-lookup"><span data-stu-id="ee164-144">Each **CookieState** represents one cookie.</span></span> <span data-ttu-id="ee164-145">Utilisez la méthode de l’indexeur pour obtenir un **CookieState** par nom, comme indiqué.</span><span class="sxs-lookup"><span data-stu-id="ee164-145">Use the indexer method to get a **CookieState** by name, as shown.</span></span>

## <a name="structured-cookie-data"></a><span data-ttu-id="ee164-146">Données de Cookie structurée</span><span class="sxs-lookup"><span data-stu-id="ee164-146">Structured Cookie Data</span></span>

<span data-ttu-id="ee164-147">De nombreux navigateurs limitent le nombre de cookies qu’ils stockera&#8212;à la fois le nombre total et le nombre par domaine.</span><span class="sxs-lookup"><span data-stu-id="ee164-147">Many browsers limit how many cookies they will store&#8212;both the total number, and the number per domain.</span></span> <span data-ttu-id="ee164-148">Par conséquent, il peut être utile placer des données structurées dans un cookie unique, au lieu de définir plusieurs cookies.</span><span class="sxs-lookup"><span data-stu-id="ee164-148">Therefore, it can be useful to put structured data into a single cookie, instead of setting multiple cookies.</span></span>

> [!NOTE]
> <span data-ttu-id="ee164-149">RFC 6265 ne définit pas la structure des données de cookie.</span><span class="sxs-lookup"><span data-stu-id="ee164-149">RFC 6265 does not define the structure of cookie data.</span></span>


<span data-ttu-id="ee164-150">À l’aide de la **CookieHeaderValue** (classe), vous pouvez passer une liste de paires nom-valeur pour les données de cookie.</span><span class="sxs-lookup"><span data-stu-id="ee164-150">Using the **CookieHeaderValue** class, you can pass a list of name-value pairs for the cookie data.</span></span> <span data-ttu-id="ee164-151">Ces paires nom-valeur sont encodés en tant que données de forme codée URL dans l’en-tête Set-Cookie :</span><span class="sxs-lookup"><span data-stu-id="ee164-151">These name-value pairs are encoded as URL-encoded form data in the Set-Cookie header:</span></span>

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

<span data-ttu-id="ee164-152">Le code précédent génère l’en-tête Set-Cookie suivant :</span><span class="sxs-lookup"><span data-stu-id="ee164-152">The previous code produces the following Set-Cookie header:</span></span>

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

<span data-ttu-id="ee164-153">Le **CookieState** classe fournit une méthode de l’indexeur pour lire les valeurs de sous-élément à partir d’un cookie dans le message de demande :</span><span class="sxs-lookup"><span data-stu-id="ee164-153">The **CookieState** class provides an indexer method to read the sub-values from a cookie in the request message:</span></span>

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a><span data-ttu-id="ee164-154">Exemple : Définir et récupérer des Cookies dans un gestionnaire de messages</span><span class="sxs-lookup"><span data-stu-id="ee164-154">Example: Set and Retrieve Cookies in a Message Handler</span></span>

<span data-ttu-id="ee164-155">Les exemples précédents ont montré comment utiliser des cookies à partir d’un contrôleur d’API Web.</span><span class="sxs-lookup"><span data-stu-id="ee164-155">The previous examples showed how to use cookies from within a Web API controller.</span></span> <span data-ttu-id="ee164-156">Une autre option consiste à utiliser [gestionnaires de messages](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="ee164-156">Another option is to use [message handlers](http-message-handlers.md).</span></span> <span data-ttu-id="ee164-157">Gestionnaires de messages sont appelées plus tôt dans le pipeline de contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="ee164-157">Message handlers are invoked earlier in the pipeline than controllers.</span></span> <span data-ttu-id="ee164-158">Un gestionnaire de messages pour lire les cookies à partir de la demande avant que la demande n’atteigne le contrôleur, ou ajouter des cookies à la réponse après que le contrôleur a généré la réponse.</span><span class="sxs-lookup"><span data-stu-id="ee164-158">A message handler can read cookies from the request before the request reaches the controller, or add cookies to the response after the controller generates the response.</span></span>

![](http-cookies/_static/image2.png)

<span data-ttu-id="ee164-159">Le code suivant montre un gestionnaire de messages pour la création d’ID de session.</span><span class="sxs-lookup"><span data-stu-id="ee164-159">The following code shows a message handler for creating session IDs.</span></span> <span data-ttu-id="ee164-160">L’ID de session est stocké dans un cookie.</span><span class="sxs-lookup"><span data-stu-id="ee164-160">The session ID is stored in a cookie.</span></span> <span data-ttu-id="ee164-161">Le gestionnaire vérifie la demande pour le cookie de session.</span><span class="sxs-lookup"><span data-stu-id="ee164-161">The handler checks the request for the session cookie.</span></span> <span data-ttu-id="ee164-162">Si la demande n’inclut pas le cookie, le gestionnaire génère un ID de la nouvelle session.</span><span class="sxs-lookup"><span data-stu-id="ee164-162">If the request does not include the cookie, the handler generates a new session ID.</span></span> <span data-ttu-id="ee164-163">Dans les deux cas, le gestionnaire stocke l’ID de session dans le **HttpRequestMessage.Properties** sac de propriétés.</span><span class="sxs-lookup"><span data-stu-id="ee164-163">In either case, the handler stores the session ID in the **HttpRequestMessage.Properties** property bag.</span></span> <span data-ttu-id="ee164-164">Il ajoute également le cookie de session à la réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="ee164-164">It also adds the session cookie to the HTTP response.</span></span>

<span data-ttu-id="ee164-165">Cette implémentation ne valide pas que l’ID de session à partir du client a été effectivement émis par le serveur.</span><span class="sxs-lookup"><span data-stu-id="ee164-165">This implementation does not validate that the session ID from the client was actually issued by the server.</span></span> <span data-ttu-id="ee164-166">Ne l’utilisez comme un formulaire d’authentification !</span><span class="sxs-lookup"><span data-stu-id="ee164-166">Don't use it as a form of authentication!</span></span> <span data-ttu-id="ee164-167">Le but de cet exemple est de montrer de gestion des cookies HTTP.</span><span class="sxs-lookup"><span data-stu-id="ee164-167">The point of the example is to show HTTP cookie management.</span></span>

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

<span data-ttu-id="ee164-168">Un contrôleur peut obtenir l’ID de session à partir de la **HttpRequestMessage.Properties** sac de propriétés.</span><span class="sxs-lookup"><span data-stu-id="ee164-168">A controller can get the session ID from the **HttpRequestMessage.Properties** property bag.</span></span>

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
