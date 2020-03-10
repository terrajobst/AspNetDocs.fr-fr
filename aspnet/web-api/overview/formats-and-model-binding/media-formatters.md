---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: Formateurs de médias dans API Web ASP.NET 2-ASP.NET 4. x
author: MikeWasson
description: Montre comment prendre en charge des formats de média supplémentaires dans API Web ASP.NET pour ASP.NET 4. x.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: da0c566dad302054d7d0a6435e4c6df178c64772
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557252"
---
# <a name="media-formatters-in-aspnet-web-api-2"></a><span data-ttu-id="6e2cf-103">Formateurs de médias dans API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="6e2cf-103">Media Formatters in ASP.NET Web API 2</span></span>

<span data-ttu-id="6e2cf-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6e2cf-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="6e2cf-105">Ce didacticiel montre comment prendre en charge des formats de média supplémentaires dans API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6e2cf-105">This tutorial shows how to support additional media formats in ASP.NET Web API.</span></span>

## <a name="internet-media-types"></a><span data-ttu-id="6e2cf-106">Types de médias Internet</span><span class="sxs-lookup"><span data-stu-id="6e2cf-106">Internet Media Types</span></span>

<span data-ttu-id="6e2cf-107">Un type de média, également appelé type MIME, identifie le format d’un élément de données.</span><span class="sxs-lookup"><span data-stu-id="6e2cf-107">A media type, also called a MIME type, identifies the format of a piece of data.</span></span> <span data-ttu-id="6e2cf-108">Dans HTTP, les types de média décrivent le format du corps du message.</span><span class="sxs-lookup"><span data-stu-id="6e2cf-108">In HTTP, media types describe the format of the message body.</span></span> <span data-ttu-id="6e2cf-109">Un type de média se compose de deux chaînes, un type et un sous-type.</span><span class="sxs-lookup"><span data-stu-id="6e2cf-109">A media type consists of two strings, a type and a subtype.</span></span> <span data-ttu-id="6e2cf-110">Exemple :</span><span class="sxs-lookup"><span data-stu-id="6e2cf-110">For example:</span></span>

- <span data-ttu-id="6e2cf-111">texte/html</span><span class="sxs-lookup"><span data-stu-id="6e2cf-111">text/html</span></span>
- <span data-ttu-id="6e2cf-112">image/png</span><span class="sxs-lookup"><span data-stu-id="6e2cf-112">image/png</span></span>
- <span data-ttu-id="6e2cf-113">application/json</span><span class="sxs-lookup"><span data-stu-id="6e2cf-113">application/json</span></span>

<span data-ttu-id="6e2cf-114">Lorsqu’un message HTTP contient un corps d’entité, l’en-tête Content-type spécifie le format du corps du message.</span><span class="sxs-lookup"><span data-stu-id="6e2cf-114">When an HTTP message contains an entity-body, the Content-Type header specifies the format of the message body.</span></span> <span data-ttu-id="6e2cf-115">Cela indique au récepteur comment analyser le contenu du corps du message.</span><span class="sxs-lookup"><span data-stu-id="6e2cf-115">This tells the receiver how to parse the contents of the message body.</span></span>

<span data-ttu-id="6e2cf-116">Par exemple, si une réponse HTTP contient une image PNG, la réponse peut contenir les en-têtes suivants.</span><span class="sxs-lookup"><span data-stu-id="6e2cf-116">For example, if an HTTP response contains a PNG image, the response might have the following headers.</span></span>

[!code-console[Main](media-formatters/samples/sample1.cmd)]

<span data-ttu-id="6e2cf-117">Lorsque le client envoie un message de demande, il peut inclure un en-tête Accept.</span><span class="sxs-lookup"><span data-stu-id="6e2cf-117">When the client sends a request message, it can include an Accept header.</span></span> <span data-ttu-id="6e2cf-118">L’en-tête Accept indique au serveur le ou les types de médias que le client souhaite obtenir du serveur.</span><span class="sxs-lookup"><span data-stu-id="6e2cf-118">The Accept header tells the server which media type(s) the client wants from the server.</span></span> <span data-ttu-id="6e2cf-119">Exemple :</span><span class="sxs-lookup"><span data-stu-id="6e2cf-119">For example:</span></span>

[!code-console[Main](media-formatters/samples/sample2.cmd)]

<span data-ttu-id="6e2cf-120">Cet en-tête indique au serveur que le client veut HTML, XHTML ou XML.</span><span class="sxs-lookup"><span data-stu-id="6e2cf-120">This header tells the server that the client wants either HTML, XHTML, or XML.</span></span>

<span data-ttu-id="6e2cf-121">Le type de média détermine la façon dont l’API Web sérialise et désérialise le corps du message HTTP.</span><span class="sxs-lookup"><span data-stu-id="6e2cf-121">The media type determines how Web API serializes and deserializes the HTTP message body.</span></span> <span data-ttu-id="6e2cf-122">L’API Web offre une prise en charge intégrée des données XML, JSON, BSON et form-urlencoded, et vous pouvez prendre en charge des types de média supplémentaires en écrivant un *formateur multimédia*.</span><span class="sxs-lookup"><span data-stu-id="6e2cf-122">Web API has built-in support for XML, JSON, BSON, and form-urlencoded data, and you can support additional media types by writing a *media formatter*.</span></span>

<span data-ttu-id="6e2cf-123">Pour créer un formateur multimédia, dérivez de l’une des classes suivantes :</span><span class="sxs-lookup"><span data-stu-id="6e2cf-123">To create a media formatter, derive from one of these classes:</span></span>

- <span data-ttu-id="6e2cf-124">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="6e2cf-124">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span></span> <span data-ttu-id="6e2cf-125">Cette classe utilise des méthodes de lecture et d’écriture asynchrones.</span><span class="sxs-lookup"><span data-stu-id="6e2cf-125">This class uses asynchronous read and write methods.</span></span>
- <span data-ttu-id="6e2cf-126">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="6e2cf-126">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span></span> <span data-ttu-id="6e2cf-127">Cette classe dérive de **MediaTypeFormatter** , mais utilise des méthodes de lecture/écriture synchrones.</span><span class="sxs-lookup"><span data-stu-id="6e2cf-127">This class derives from **MediaTypeFormatter** but uses synchronous read/write methods.</span></span>

<span data-ttu-id="6e2cf-128">La dérivation à partir de **BufferedMediaTypeFormatter** est plus simple, car il n’y a pas de code asynchrone, mais cela signifie également que le thread appelant peut se bloquer pendant les e/s.</span><span class="sxs-lookup"><span data-stu-id="6e2cf-128">Deriving from **BufferedMediaTypeFormatter** is simpler, because there is no asynchronous code, but it also means the calling thread can block during I/O.</span></span>

## <a name="example-creating-a-csv-media-formatter"></a><span data-ttu-id="6e2cf-129">Exemple : création d’un formateur de média CSV</span><span class="sxs-lookup"><span data-stu-id="6e2cf-129">Example: Creating a CSV Media Formatter</span></span>

<span data-ttu-id="6e2cf-130">L’exemple suivant montre un formateur de type de média qui peut sérialiser un objet Product dans un format CSV (valeurs séparées par des virgules).</span><span class="sxs-lookup"><span data-stu-id="6e2cf-130">The following example shows a media type formatter that can serialize a Product object to a comma-separated values (CSV) format.</span></span> <span data-ttu-id="6e2cf-131">Cet exemple utilise le type de produit défini dans le didacticiel [création d’une API Web qui prend en charge les opérations CRUD](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span><span class="sxs-lookup"><span data-stu-id="6e2cf-131">This example uses the Product type defined in the tutorial [Creating a Web API that Supports CRUD Operations](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span></span> <span data-ttu-id="6e2cf-132">Voici la définition de l’objet Product :</span><span class="sxs-lookup"><span data-stu-id="6e2cf-132">Here is the definition of the Product object:</span></span>

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

<span data-ttu-id="6e2cf-133">Pour implémenter un formateur CSV, définissez une classe qui dérive de **BufferedMediaTypeFormatter**:</span><span class="sxs-lookup"><span data-stu-id="6e2cf-133">To implement a CSV formatter, define a class that derives from **BufferedMediaTypeFormatter**:</span></span>

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

<span data-ttu-id="6e2cf-134">Dans le constructeur, ajoutez les types de médias pris en charge par le formateur.</span><span class="sxs-lookup"><span data-stu-id="6e2cf-134">In the constructor, add the media types that the formatter supports.</span></span> <span data-ttu-id="6e2cf-135">Dans cet exemple, le formateur prend en charge un type de média unique, &quot;texte/CSV&quot;:</span><span class="sxs-lookup"><span data-stu-id="6e2cf-135">In this example, the formatter supports a single media type, &quot;text/csv&quot;:</span></span>

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

<span data-ttu-id="6e2cf-136">Substituez la méthode **CanWriteType** pour indiquer les types que le formateur peut sérialiser :</span><span class="sxs-lookup"><span data-stu-id="6e2cf-136">Override the **CanWriteType** method to indicate which types the formatter can serialize:</span></span>

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

<span data-ttu-id="6e2cf-137">Dans cet exemple, le formateur peut sérialiser des objets `Product` uniques ainsi que des collections d’objets `Product`.</span><span class="sxs-lookup"><span data-stu-id="6e2cf-137">In this example, the formatter can serialize single `Product` objects as well as collections of `Product` objects.</span></span>

<span data-ttu-id="6e2cf-138">De même, substituez la méthode **CanReadType** pour indiquer les types que le formateur peut désérialiser.</span><span class="sxs-lookup"><span data-stu-id="6e2cf-138">Similarly, override the **CanReadType** method to indicate which types the formatter can deserialize.</span></span> <span data-ttu-id="6e2cf-139">Dans cet exemple, le formateur ne prend pas en charge la désérialisation, donc la méthode retourne simplement **false**.</span><span class="sxs-lookup"><span data-stu-id="6e2cf-139">In this example, the formatter does not support deserialization, so the method simply returns **false**.</span></span>

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

<span data-ttu-id="6e2cf-140">Enfin, remplacez la méthode **WriteToStream** .</span><span class="sxs-lookup"><span data-stu-id="6e2cf-140">Finally, override the **WriteToStream** method.</span></span> <span data-ttu-id="6e2cf-141">Cette méthode sérialise un type en l’écrivant dans un flux.</span><span class="sxs-lookup"><span data-stu-id="6e2cf-141">This method serializes a type by writing it to a stream.</span></span> <span data-ttu-id="6e2cf-142">Si votre formateur prend en charge la désérialisation, substituez également la méthode **readFromStream** .</span><span class="sxs-lookup"><span data-stu-id="6e2cf-142">If your formatter supports deserialization, also override the **ReadFromStream** method.</span></span>

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a><span data-ttu-id="6e2cf-143">Ajout d’un formateur multimédia au pipeline de l’API Web</span><span class="sxs-lookup"><span data-stu-id="6e2cf-143">Adding a Media Formatter to the Web API Pipeline</span></span>

<span data-ttu-id="6e2cf-144">Pour ajouter un formateur de type de média au pipeline de l’API Web, utilisez la propriété **Formatters** sur l’objet **HttpConfiguration** .</span><span class="sxs-lookup"><span data-stu-id="6e2cf-144">To add a media type formatter to the Web API pipeline, use the **Formatters** property on the **HttpConfiguration** object.</span></span>

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a><span data-ttu-id="6e2cf-145">Encodages de caractères</span><span class="sxs-lookup"><span data-stu-id="6e2cf-145">Character Encodings</span></span>

<span data-ttu-id="6e2cf-146">Un formateur de média peut éventuellement prendre en charge plusieurs encodages de caractères, comme UTF-8 ou ISO 8859-1.</span><span class="sxs-lookup"><span data-stu-id="6e2cf-146">Optionally, a media formatter can support multiple character encodings, such as UTF-8 or ISO 8859-1.</span></span>

<span data-ttu-id="6e2cf-147">Dans le constructeur, ajoutez un ou plusieurs types [System. Text. Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) à la collection **SupportedEncodings** .</span><span class="sxs-lookup"><span data-stu-id="6e2cf-147">In the constructor, add one or more [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) types to the **SupportedEncodings** collection.</span></span> <span data-ttu-id="6e2cf-148">Placez d’abord l’encodage par défaut.</span><span class="sxs-lookup"><span data-stu-id="6e2cf-148">Put the default encoding first.</span></span>

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

<span data-ttu-id="6e2cf-149">Dans les méthodes **WriteToStream** et **readFromStream** , appelez [MediaTypeFormatter. SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) pour sélectionner l’encodage de caractères par défaut.</span><span class="sxs-lookup"><span data-stu-id="6e2cf-149">In the **WriteToStream** and **ReadFromStream** methods, call [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) to select the preferred character encoding.</span></span> <span data-ttu-id="6e2cf-150">Cette méthode correspond aux en-têtes de requête par rapport à la liste des encodages pris en charge.</span><span class="sxs-lookup"><span data-stu-id="6e2cf-150">This method matches the request headers against the list of supported encodings.</span></span> <span data-ttu-id="6e2cf-151">Utilisez l' **encodage** retourné lors de la lecture ou de l’écriture à partir du flux :</span><span class="sxs-lookup"><span data-stu-id="6e2cf-151">Use the returned **Encoding** when you read or write from the stream:</span></span>

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
