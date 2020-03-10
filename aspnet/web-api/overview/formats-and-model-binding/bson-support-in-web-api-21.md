---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: Prise en charge de BSON dans API Web ASP.NET 2,1-ASP.NET 4. x
author: MikeWasson
description: montre comment utiliser BSON dans un contrôleur d’API Web (côté serveur) et dans une application cliente .NET pour ASP.NET 4. x.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: ccbc0372120301b1cd8d4cdc86bd9fba9404d8ae
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622296"
---
# <a name="bson-support-in-aspnet-web-api-21"></a><span data-ttu-id="9c09e-103">Prise en charge de BSON dans API Web ASP.NET 2,1</span><span class="sxs-lookup"><span data-stu-id="9c09e-103">BSON Support in ASP.NET Web API 2.1</span></span>

<span data-ttu-id="9c09e-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="9c09e-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="9c09e-105">Cette rubrique montre comment utiliser BSON dans votre contrôleur d’API Web (côté serveur) et dans une application cliente .NET.</span><span class="sxs-lookup"><span data-stu-id="9c09e-105">This topic shows how to use BSON in your Web API controller (server side) and in a .NET client app.</span></span> <span data-ttu-id="9c09e-106">L’API Web 2,1 introduit la prise en charge de BSON.</span><span class="sxs-lookup"><span data-stu-id="9c09e-106">Web API 2.1 introduces support for BSON.</span></span> 

## <a name="what-is-bson"></a><span data-ttu-id="9c09e-107">Qu’est-ce que BSON ?</span><span class="sxs-lookup"><span data-stu-id="9c09e-107">What is BSON?</span></span>

<span data-ttu-id="9c09e-108">[BSON](http://bsonspec.org/) est un format de sérialisation binaire.</span><span class="sxs-lookup"><span data-stu-id="9c09e-108">[BSON](http://bsonspec.org/) is a binary serialization format.</span></span> <span data-ttu-id="9c09e-109">« BSON » signifie « JSON binaire », mais BSON et JSON sont sérialisés très différemment.</span><span class="sxs-lookup"><span data-stu-id="9c09e-109">"BSON" stands for "Binary JSON", but BSON and JSON are serialized very differently.</span></span> <span data-ttu-id="9c09e-110">BSON est de type JSON, car les objets sont représentés en tant que paires nom-valeur, similaire à JSON.</span><span class="sxs-lookup"><span data-stu-id="9c09e-110">BSON is "JSON-like", because objects are represented as name-value pairs, similar to JSON.</span></span> <span data-ttu-id="9c09e-111">Contrairement à JSON, les types de données numériques sont stockés sous la forme d’octets et non de chaînes</span><span class="sxs-lookup"><span data-stu-id="9c09e-111">Unlike JSON, numeric data types are stored as bytes, not strings</span></span>

<span data-ttu-id="9c09e-112">BSON a été conçu pour être léger, facile à analyser et rapide à encoder/décoder.</span><span class="sxs-lookup"><span data-stu-id="9c09e-112">BSON was designed to be lightweight, easy to scan, and fast to encode/decode.</span></span>

- <span data-ttu-id="9c09e-113">BSON a une taille comparable à JSON.</span><span class="sxs-lookup"><span data-stu-id="9c09e-113">BSON is comparable in size to JSON.</span></span> <span data-ttu-id="9c09e-114">Selon les données, une charge utile BSON peut être plus modeste ou plus volumineuse qu’une charge utile JSON.</span><span class="sxs-lookup"><span data-stu-id="9c09e-114">Depending on the data, a BSON payload may be smaller or larger than a JSON payload.</span></span> <span data-ttu-id="9c09e-115">Pour sérialiser des données binaires, telles qu’un fichier image, BSON est plus petit que JSON, car les données binaires ne sont pas encodées en base64.</span><span class="sxs-lookup"><span data-stu-id="9c09e-115">For serializing binary data, such as an image file, BSON is smaller than JSON, because the binary data is not base64-encoded.</span></span>
- <span data-ttu-id="9c09e-116">Les documents BSON sont faciles à analyser, car les éléments sont précédés d’un champ de longueur, de sorte qu’un analyseur peut ignorer les éléments sans les décoder.</span><span class="sxs-lookup"><span data-stu-id="9c09e-116">BSON documents are easy to scan because elements are prefixed with a length field, so a parser can skip elements without decoding them.</span></span>
- <span data-ttu-id="9c09e-117">L’encodage et le décodage sont efficaces, car les types de données numériques sont stockés sous la forme de nombres, et non de chaînes.</span><span class="sxs-lookup"><span data-stu-id="9c09e-117">Encoding and decoding are efficient, because numeric data types are stored as numbers, not strings.</span></span>

<span data-ttu-id="9c09e-118">Les clients natifs, tels que les applications clientes .NET, peuvent tirer parti de l’utilisation de BSON à la place de formats textuels tels que JSON ou XML.</span><span class="sxs-lookup"><span data-stu-id="9c09e-118">Native clients, such as .NET client apps, can benefit from using BSON in place of text-based formats such as JSON or XML.</span></span> <span data-ttu-id="9c09e-119">Pour les clients de navigateur, vous souhaiterez sans doute utiliser JSON, car JavaScript peut directement convertir la charge utile JSON.</span><span class="sxs-lookup"><span data-stu-id="9c09e-119">For browser clients, you will probably want to stick with JSON, because JavaScript can directly convert the JSON payload.</span></span>

<span data-ttu-id="9c09e-120">Heureusement, l’API Web utilise la [négociation de contenu](content-negotiation.md). par conséquent, votre API peut prendre en charge les deux formats et laisser le client choisir.</span><span class="sxs-lookup"><span data-stu-id="9c09e-120">Fortunately, Web API uses [content negotiation](content-negotiation.md), so your API can support both formats and let the client choose.</span></span>

## <a name="enabling-bson-on-the-server"></a><span data-ttu-id="9c09e-121">Activation de BSON sur le serveur</span><span class="sxs-lookup"><span data-stu-id="9c09e-121">Enabling BSON on the Server</span></span>

<span data-ttu-id="9c09e-122">Dans la configuration de votre API Web, ajoutez **BsonMediaTypeFormatter** à la collection Formatters.</span><span class="sxs-lookup"><span data-stu-id="9c09e-122">In your Web API configuration, add the **BsonMediaTypeFormatter** to the formatters collection.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

<span data-ttu-id="9c09e-123">Désormais, si le client demande « application/BSON », l’API Web utilise le formateur BSON.</span><span class="sxs-lookup"><span data-stu-id="9c09e-123">Now if the client requests "application/bson", Web API will use the BSON formatter.</span></span>

<span data-ttu-id="9c09e-124">Pour associer des BSON à d’autres types de médias, ajoutez-les à la collection SupportedMediaTypes.</span><span class="sxs-lookup"><span data-stu-id="9c09e-124">To associate BSON with other media types, add them to the SupportedMediaTypes collection.</span></span> <span data-ttu-id="9c09e-125">Le code suivant ajoute « application/vnd. contoso » aux types de média pris en charge :</span><span class="sxs-lookup"><span data-stu-id="9c09e-125">The following code adds "application/vnd.contoso" to the supported media types:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a><span data-ttu-id="9c09e-126">Exemple de session HTTP</span><span class="sxs-lookup"><span data-stu-id="9c09e-126">Example HTTP Session</span></span>

<span data-ttu-id="9c09e-127">Pour cet exemple, nous allons utiliser la classe de modèle suivante et un contrôleur d’API Web simple :</span><span class="sxs-lookup"><span data-stu-id="9c09e-127">For this example, we'll use the following model class plus a simple Web API controller:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

<span data-ttu-id="9c09e-128">Un client peut envoyer la requête HTTP suivante :</span><span class="sxs-lookup"><span data-stu-id="9c09e-128">A client might send the following HTTP request:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

<span data-ttu-id="9c09e-129">Voici la réponse :</span><span class="sxs-lookup"><span data-stu-id="9c09e-129">Here is the response:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

<span data-ttu-id="9c09e-130">Ici, j’ai remplacé les données binaires par &quot;.&quot; caractères.</span><span class="sxs-lookup"><span data-stu-id="9c09e-130">Here I've replaced the binary data with &quot;.&quot; characters.</span></span> <span data-ttu-id="9c09e-131">La capture d’écran suivante de Fiddler montre les valeurs hex brutes.</span><span class="sxs-lookup"><span data-stu-id="9c09e-131">The following screen shot from Fiddler shows the raw hex values.</span></span>

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a><span data-ttu-id="9c09e-132">Utilisation de BSON avec HttpClient</span><span class="sxs-lookup"><span data-stu-id="9c09e-132">Using BSON with HttpClient</span></span>

<span data-ttu-id="9c09e-133">Les applications clientes .NET peuvent utiliser le formateur BSON avec **httpclient**.</span><span class="sxs-lookup"><span data-stu-id="9c09e-133">.NET clients apps can use the BSON formatter with **HttpClient**.</span></span> <span data-ttu-id="9c09e-134">Pour plus d’informations sur **httpclient**, consultez [appel d’une API Web à partir d’un client .net](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="9c09e-134">For more information about **HttpClient**, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="9c09e-135">Le code suivant envoie une requête d’extraction qui accepte BSON, puis désérialise la charge utile BSON dans la réponse.</span><span class="sxs-lookup"><span data-stu-id="9c09e-135">The following code sends a GET request that accepts BSON, and then deserializes the BSON payload in the response.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

<span data-ttu-id="9c09e-136">Pour demander BSON à partir du serveur, définissez l’en-tête Accept sur « application/BSON » :</span><span class="sxs-lookup"><span data-stu-id="9c09e-136">To request BSON from the server, set the Accept header to "application/bson":</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

<span data-ttu-id="9c09e-137">Pour désérialiser le corps de la réponse, utilisez **BsonMediaTypeFormatter**.</span><span class="sxs-lookup"><span data-stu-id="9c09e-137">To deserialize the response body, use the **BsonMediaTypeFormatter**.</span></span> <span data-ttu-id="9c09e-138">Ce formateur n’est pas dans la collection de formateurs par défaut. vous devez donc le spécifier lorsque vous lisez le corps de la réponse :</span><span class="sxs-lookup"><span data-stu-id="9c09e-138">This formatter is not in the default formatters collection, so you have to specify it when you read the response body:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

<span data-ttu-id="9c09e-139">L’exemple suivant montre comment envoyer une demande de publication contenant BSON.</span><span class="sxs-lookup"><span data-stu-id="9c09e-139">The next example shows how to send a POST request that contains BSON.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

<span data-ttu-id="9c09e-140">Une grande partie de ce code est identique à l’exemple précédent.</span><span class="sxs-lookup"><span data-stu-id="9c09e-140">Much of this code is the same as the previous example.</span></span> <span data-ttu-id="9c09e-141">Mais dans la méthode **PostAsync** , spécifiez **BsonMediaTypeFormatter** comme formateur :</span><span class="sxs-lookup"><span data-stu-id="9c09e-141">But in the **PostAsync** method, specify **BsonMediaTypeFormatter** as the formatter:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a><span data-ttu-id="9c09e-142">Sérialisation des types primitifs de niveau supérieur</span><span class="sxs-lookup"><span data-stu-id="9c09e-142">Serializing Top-Level Primitive Types</span></span>

<span data-ttu-id="9c09e-143">Chaque document BSON est une liste de paires clé/valeur. La spécification BSON ne définit pas de syntaxe pour la sérialisation d’une seule valeur brute, telle qu’un entier ou une chaîne.</span><span class="sxs-lookup"><span data-stu-id="9c09e-143">Every BSON document is a list of key/value pairs.The BSON specification does not define a syntax for serializing a single raw value, such as an integer or string.</span></span>

<span data-ttu-id="9c09e-144">Pour contourner cette limitation, **BsonMediaTypeFormatter** traite les types primitifs comme un cas spécial.</span><span class="sxs-lookup"><span data-stu-id="9c09e-144">To work around this limitation, the **BsonMediaTypeFormatter** treats primitive types as a special case.</span></span> <span data-ttu-id="9c09e-145">Avant de sérialiser, elle convertit la valeur en une paire clé/valeur avec la clé « value ».</span><span class="sxs-lookup"><span data-stu-id="9c09e-145">Before serializing, it converts the value into a key/value pair with the key "Value".</span></span> <span data-ttu-id="9c09e-146">Par exemple, supposons que votre contrôleur d’API retourne un entier :</span><span class="sxs-lookup"><span data-stu-id="9c09e-146">For example, suppose your API controller returns an integer:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

<span data-ttu-id="9c09e-147">Avant de sérialiser, le formateur BSON le convertit en paire clé/valeur suivante :</span><span class="sxs-lookup"><span data-stu-id="9c09e-147">Before serializing, the BSON formatter converts this to the following key/value pair:</span></span>

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

<span data-ttu-id="9c09e-148">Lorsque vous désérialisez, le formateur rétablit la valeur d’origine des données.</span><span class="sxs-lookup"><span data-stu-id="9c09e-148">When you deserialize, the formatter converts the data back to the original value.</span></span> <span data-ttu-id="9c09e-149">Toutefois, les clients qui utilisent un autre analyseur BSON doivent gérer ce cas, si votre API Web retourne des valeurs brutes.</span><span class="sxs-lookup"><span data-stu-id="9c09e-149">However, clients using a different BSON parser will need to handle this case, if your web API returns raw values.</span></span> <span data-ttu-id="9c09e-150">En général, vous devez envisager de retourner des données structurées plutôt que des valeurs brutes.</span><span class="sxs-lookup"><span data-stu-id="9c09e-150">In general, you should consider returning structured data, rather than raw values.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9c09e-151">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="9c09e-151">Additional Resources</span></span>

[<span data-ttu-id="9c09e-152">API Web BSON, exemple</span><span class="sxs-lookup"><span data-stu-id="9c09e-152">Web API BSON Sample</span></span>](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/BSONSample/)

[<span data-ttu-id="9c09e-153">Formateurs de médias</span><span class="sxs-lookup"><span data-stu-id="9c09e-153">Media Formatters</span></span>](media-formatters.md)
