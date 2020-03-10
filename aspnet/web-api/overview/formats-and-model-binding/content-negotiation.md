---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: Négociation de contenu dans API Web ASP.NET-ASP.NET 4. x
author: MikeWasson
description: Décrit comment API Web ASP.NET implémente la négociation de contenu HTTP pour ASP.NET 4. x.
ms.author: riande
ms.date: 05/20/2012
ms.custom: seoapril2019
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: cb6668ff6de276d3778ce11f27ce597d8bf1f9c7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622268"
---
# <a name="content-negotiation-in-aspnet-web-api"></a><span data-ttu-id="df66e-103">Négociation de contenu dans API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="df66e-103">Content Negotiation in ASP.NET Web API</span></span>

<span data-ttu-id="df66e-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="df66e-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="df66e-105">Cet article explique comment API Web ASP.NET implémente la négociation de contenu pour ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="df66e-105">This article describes how ASP.NET Web API implements content negotiation for ASP.NET 4.x.</span></span>

<span data-ttu-id="df66e-106">La spécification HTTP (RFC 2616) définit la négociation de contenu comme « le processus de sélection de la meilleure représentation pour une réponse donnée lorsqu’il y a plusieurs représentations disponibles ».</span><span class="sxs-lookup"><span data-stu-id="df66e-106">The HTTP specification (RFC 2616) defines content negotiation as "the process of selecting the best representation for a given response when there are multiple representations available."</span></span> <span data-ttu-id="df66e-107">Le mécanisme principal de négociation de contenu dans HTTP sont les en-têtes de requête suivants :</span><span class="sxs-lookup"><span data-stu-id="df66e-107">The primary mechanism for content negotiation in HTTP are these request headers:</span></span>

- <span data-ttu-id="df66e-108">**Accepter :** Les types de médias qui sont acceptables pour la réponse, tels que « application/JSON », « application/XML » ou un type de support personnalisé tel que &quot;application/vnd. example + XML&quot;</span><span class="sxs-lookup"><span data-stu-id="df66e-108">**Accept:** Which media types are acceptable for the response, such as "application/json," "application/xml," or a custom media type such as &quot;application/vnd.example+xml&quot;</span></span>
- <span data-ttu-id="df66e-109">**Accept-Charset :** Les jeux de caractères admis, par exemple UTF-8 ou ISO 8859-1.</span><span class="sxs-lookup"><span data-stu-id="df66e-109">**Accept-Charset:** Which character sets are acceptable, such as UTF-8 or ISO 8859-1.</span></span>
- <span data-ttu-id="df66e-110">**Acceptation-encodage :** Les encodages de contenu acceptables, tels que gzip.</span><span class="sxs-lookup"><span data-stu-id="df66e-110">**Accept-Encoding:** Which content encodings are acceptable, such as gzip.</span></span>
- <span data-ttu-id="df66e-111">**Accept-Language :** Langage naturel préféré, tel que « en-US ».</span><span class="sxs-lookup"><span data-stu-id="df66e-111">**Accept-Language:** The preferred natural language, such as "en-us".</span></span>

<span data-ttu-id="df66e-112">Le serveur peut également examiner d’autres parties de la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="df66e-112">The server can also look at other portions of the HTTP request.</span></span> <span data-ttu-id="df66e-113">Par exemple, si la requête contient un en-tête X-requested-with, indiquant une requête AJAX, le serveur peut utiliser par défaut JSON en l’absence d’en-tête Accept.</span><span class="sxs-lookup"><span data-stu-id="df66e-113">For example, if the request contains an X-Requested-With header, indicating an AJAX request, the server might default to JSON if there is no Accept header.</span></span>

<span data-ttu-id="df66e-114">Dans cet article, nous allons examiner comment l’API Web utilise les en-têtes Accept et Accept-Charset.</span><span class="sxs-lookup"><span data-stu-id="df66e-114">In this article, we'll look at how Web API uses the Accept and Accept-Charset headers.</span></span> <span data-ttu-id="df66e-115">(À ce stade, il n’existe pas de prise en charge intégrée pour Accept-Encoding ou Accept-Language.)</span><span class="sxs-lookup"><span data-stu-id="df66e-115">(At this time, there is no built-in support for Accept-Encoding or Accept-Language.)</span></span>

## <a name="serialization"></a><span data-ttu-id="df66e-116">Sérialisation</span><span class="sxs-lookup"><span data-stu-id="df66e-116">Serialization</span></span>

<span data-ttu-id="df66e-117">Si un contrôleur d’API Web retourne une ressource en tant que type CLR, le pipeline sérialise la valeur de retour et l’écrit dans le corps de la réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="df66e-117">If a Web API controller returns a resource as CLR type, the pipeline serializes the return value and writes it into the HTTP response body.</span></span>

<span data-ttu-id="df66e-118">Par exemple, considérez l’action de contrôleur suivante :</span><span class="sxs-lookup"><span data-stu-id="df66e-118">For example, consider the following controller action:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

<span data-ttu-id="df66e-119">Un client peut envoyer cette requête HTTP :</span><span class="sxs-lookup"><span data-stu-id="df66e-119">A client might send this HTTP request:</span></span>

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

<span data-ttu-id="df66e-120">En réponse, le serveur peut envoyer :</span><span class="sxs-lookup"><span data-stu-id="df66e-120">In response, the server might send:</span></span>

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

<span data-ttu-id="df66e-121">Dans cet exemple, le client a demandé JSON, JavaScript ou « tout » (\*/\*).</span><span class="sxs-lookup"><span data-stu-id="df66e-121">In this example, the client requested either JSON, Javascript, or "anything" (\*/\*).</span></span> <span data-ttu-id="df66e-122">Le serveur a répondu avec une représentation JSON de l’objet `Product`.</span><span class="sxs-lookup"><span data-stu-id="df66e-122">The server responded with a JSON representation of the `Product` object.</span></span> <span data-ttu-id="df66e-123">Notez que l’en-tête Content-type dans la réponse est défini sur &quot;application/JSON&quot;.</span><span class="sxs-lookup"><span data-stu-id="df66e-123">Notice that the Content-Type header in the response is set to &quot;application/json&quot;.</span></span>

<span data-ttu-id="df66e-124">Un contrôleur peut également retourner un objet **HttpResponseMessage** .</span><span class="sxs-lookup"><span data-stu-id="df66e-124">A controller can also return an **HttpResponseMessage** object.</span></span> <span data-ttu-id="df66e-125">Pour spécifier un objet CLR pour le corps de la réponse, appelez la méthode d’extension **CreateResponse** :</span><span class="sxs-lookup"><span data-stu-id="df66e-125">To specify a CLR object for the response body, call the **CreateResponse** extension method:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

<span data-ttu-id="df66e-126">Cette option vous donne davantage de contrôle sur les détails de la réponse.</span><span class="sxs-lookup"><span data-stu-id="df66e-126">This option gives you more control over the details of the response.</span></span> <span data-ttu-id="df66e-127">Vous pouvez définir le code d’État, ajouter des en-têtes HTTP, etc.</span><span class="sxs-lookup"><span data-stu-id="df66e-127">You can set the status code, add HTTP headers, and so forth.</span></span>

<span data-ttu-id="df66e-128">L’objet qui sérialise la ressource est appelé *formateur multimédia*.</span><span class="sxs-lookup"><span data-stu-id="df66e-128">The object that serializes the resource is called a *media formatter*.</span></span> <span data-ttu-id="df66e-129">Les formateurs multimédias dérivent de la classe **MediaTypeFormatter** .</span><span class="sxs-lookup"><span data-stu-id="df66e-129">Media formatters derive from the **MediaTypeFormatter** class.</span></span> <span data-ttu-id="df66e-130">L’API Web fournit des formateurs multimédias pour XML et JSON, et vous pouvez créer des formateurs personnalisés pour prendre en charge d’autres types de média.</span><span class="sxs-lookup"><span data-stu-id="df66e-130">Web API provides media formatters for XML and JSON, and you can create custom formatters to support other media types.</span></span> <span data-ttu-id="df66e-131">Pour plus d’informations sur l’écriture d’un formateur personnalisé, consultez [formateurs de médias](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="df66e-131">For information about writing a custom formatter, see [Media Formatters](media-formatters.md).</span></span>

## <a name="how-content-negotiation-works"></a><span data-ttu-id="df66e-132">Fonctionnement de la négociation de contenu</span><span class="sxs-lookup"><span data-stu-id="df66e-132">How Content Negotiation Works</span></span>

<span data-ttu-id="df66e-133">Tout d’abord, le pipeline obtient le service **IContentNegotiator** à partir de l’objet **HttpConfiguration** .</span><span class="sxs-lookup"><span data-stu-id="df66e-133">First, the pipeline gets the **IContentNegotiator** service from the **HttpConfiguration** object.</span></span> <span data-ttu-id="df66e-134">Elle obtient également la liste des formateurs de média à partir de la collection **HttpConfiguration. Formatters** .</span><span class="sxs-lookup"><span data-stu-id="df66e-134">It also gets the list of media formatters from the **HttpConfiguration.Formatters** collection.</span></span>

<span data-ttu-id="df66e-135">Ensuite, le pipeline appelle **IContentNegotiator. Negotiate**, en passant :</span><span class="sxs-lookup"><span data-stu-id="df66e-135">Next, the pipeline calls **IContentNegotiator.Negotiate**, passing in:</span></span>

- <span data-ttu-id="df66e-136">Type d’objet à sérialiser.</span><span class="sxs-lookup"><span data-stu-id="df66e-136">The type of object to serialize</span></span>
- <span data-ttu-id="df66e-137">Collection de formateurs de médias</span><span class="sxs-lookup"><span data-stu-id="df66e-137">The collection of media formatters</span></span>
- <span data-ttu-id="df66e-138">La requête HTTP</span><span class="sxs-lookup"><span data-stu-id="df66e-138">The HTTP request</span></span>

<span data-ttu-id="df66e-139">La méthode **Negotiate** retourne deux informations :</span><span class="sxs-lookup"><span data-stu-id="df66e-139">The **Negotiate** method returns two pieces of information:</span></span>

- <span data-ttu-id="df66e-140">Formateur à utiliser</span><span class="sxs-lookup"><span data-stu-id="df66e-140">Which formatter to use</span></span>
- <span data-ttu-id="df66e-141">Type de média pour la réponse</span><span class="sxs-lookup"><span data-stu-id="df66e-141">The media type for the response</span></span>

<span data-ttu-id="df66e-142">Si aucun formateur n’est trouvé, la méthode **Negotiate** retourne la **valeur null**et le client reçoit l’erreur http 406 (non acceptable).</span><span class="sxs-lookup"><span data-stu-id="df66e-142">If no formatter is found, the **Negotiate** method returns **null**, and the client receives HTTP error 406 (Not Acceptable).</span></span>

<span data-ttu-id="df66e-143">Le code suivant montre comment un contrôleur peut appeler directement la négociation de contenu :</span><span class="sxs-lookup"><span data-stu-id="df66e-143">The following code shows how a controller can directly invoke content negotiation:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

<span data-ttu-id="df66e-144">Ce code équivaut à ce que le pipeline effectue automatiquement.</span><span class="sxs-lookup"><span data-stu-id="df66e-144">This code is equivalent to the what the pipeline does automatically.</span></span>

## <a name="default-content-negotiator"></a><span data-ttu-id="df66e-145">Négociateur de contenu par défaut</span><span class="sxs-lookup"><span data-stu-id="df66e-145">Default Content Negotiator</span></span>

<span data-ttu-id="df66e-146">La classe **DefaultContentNegotiator** fournit l’implémentation par défaut de **IContentNegotiator**.</span><span class="sxs-lookup"><span data-stu-id="df66e-146">The **DefaultContentNegotiator** class provides the default implementation of **IContentNegotiator**.</span></span> <span data-ttu-id="df66e-147">Il utilise plusieurs critères pour sélectionner un formateur.</span><span class="sxs-lookup"><span data-stu-id="df66e-147">It uses several criteria to select a formatter.</span></span>

<span data-ttu-id="df66e-148">Tout d’abord, le formateur doit être en mesure de sérialiser le type.</span><span class="sxs-lookup"><span data-stu-id="df66e-148">First, the formatter must be able to serialize the type.</span></span> <span data-ttu-id="df66e-149">Cette vérification est effectuée en appelant **MediaTypeFormatter. CanWriteType**.</span><span class="sxs-lookup"><span data-stu-id="df66e-149">This is verified by calling **MediaTypeFormatter.CanWriteType**.</span></span>

<span data-ttu-id="df66e-150">Ensuite, le négociateur de contenu examine chaque formateur et évalue la manière dont il correspond à la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="df66e-150">Next, the content negotiator looks at each formatter and evaluates how well it matches the HTTP request.</span></span> <span data-ttu-id="df66e-151">Pour évaluer la correspondance, le négociateur de contenu examine deux choses sur le formateur :</span><span class="sxs-lookup"><span data-stu-id="df66e-151">To evaluate the match, the content negotiator looks at two things on the formatter:</span></span>

- <span data-ttu-id="df66e-152">Collection **SupportedMediaTypes** , qui contient la liste des types de média pris en charge.</span><span class="sxs-lookup"><span data-stu-id="df66e-152">The **SupportedMediaTypes** collection, which contains a list of supported media types.</span></span> <span data-ttu-id="df66e-153">Le négociateur de contenu tente de faire correspondre cette liste avec l’en-tête d’acceptation de la demande.</span><span class="sxs-lookup"><span data-stu-id="df66e-153">The content negotiator tries to match this list against the request Accept header.</span></span> <span data-ttu-id="df66e-154">Notez que l’en-tête Accept peut inclure des plages.</span><span class="sxs-lookup"><span data-stu-id="df66e-154">Note that the Accept header can include ranges.</span></span> <span data-ttu-id="df66e-155">Par exemple, « texte/brut » est une correspondance pour le texte/la\* ou \*/\*.</span><span class="sxs-lookup"><span data-stu-id="df66e-155">For example, "text/plain" is a match for text/\* or \*/\*.</span></span>
- <span data-ttu-id="df66e-156">Collection **MediaTypeMappings** , qui contient une liste d’objets **MediaTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="df66e-156">The **MediaTypeMappings** collection, which contains a list of **MediaTypeMapping** objects.</span></span> <span data-ttu-id="df66e-157">La classe **MediaTypeMapping** offre un moyen générique de faire correspondre les requêtes http avec les types de média.</span><span class="sxs-lookup"><span data-stu-id="df66e-157">The **MediaTypeMapping** class provides a generic way to match HTTP requests with media types.</span></span> <span data-ttu-id="df66e-158">Par exemple, il peut mapper un en-tête HTTP personnalisé à un type de média particulier.</span><span class="sxs-lookup"><span data-stu-id="df66e-158">For example, it could map a custom HTTP header to a particular media type.</span></span>

<span data-ttu-id="df66e-159">S’il existe plusieurs correspondances, la correspondance avec le facteur de qualité le plus élevé gagne.</span><span class="sxs-lookup"><span data-stu-id="df66e-159">If there are multiple matches, the match with the highest quality factor wins.</span></span> <span data-ttu-id="df66e-160">Exemple :</span><span class="sxs-lookup"><span data-stu-id="df66e-160">For example:</span></span>

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

<span data-ttu-id="df66e-161">Dans cet exemple, application/JSON a un facteur de qualité implicite de 1,0, donc il est préférable à application/XML.</span><span class="sxs-lookup"><span data-stu-id="df66e-161">In this example, application/json has an implied quality factor of 1.0, so it is preferred over application/xml.</span></span>

<span data-ttu-id="df66e-162">Si aucune correspondance n’est trouvée, le négociateur de contenu tente de faire correspondre le type de média du corps de la demande, le cas échéant.</span><span class="sxs-lookup"><span data-stu-id="df66e-162">If no matches are found, the content negotiator tries to match on the media type of the request body, if any.</span></span> <span data-ttu-id="df66e-163">Par exemple, si la requête contient des données JSON, le négociateur de contenu recherche un formateur JSON.</span><span class="sxs-lookup"><span data-stu-id="df66e-163">For example, if the request contains JSON data, the content negotiator looks for a JSON formatter.</span></span>

<span data-ttu-id="df66e-164">S’il n’y a toujours aucune correspondance, le négociateur de contenu sélectionne simplement le premier formateur qui peut sérialiser le type.</span><span class="sxs-lookup"><span data-stu-id="df66e-164">If there are still no matches, the content negotiator simply picks the first formatter that can serialize the type.</span></span>

## <a name="selecting-a-character-encoding"></a><span data-ttu-id="df66e-165">Sélection d’un encodage de caractères</span><span class="sxs-lookup"><span data-stu-id="df66e-165">Selecting a Character Encoding</span></span>

<span data-ttu-id="df66e-166">Une fois qu’un formateur est sélectionné, le négociateur de contenu choisit le meilleur encodage de caractères en examinant la propriété **SupportedEncodings** du formateur et en la faisant correspondre à l’en-tête Accept-Charset dans la requête (le cas échéant).</span><span class="sxs-lookup"><span data-stu-id="df66e-166">After a formatter is selected, the content negotiator chooses the best character encoding by looking at the **SupportedEncodings** property on the formatter, and matching it against the Accept-Charset header in the request (if any).</span></span>
