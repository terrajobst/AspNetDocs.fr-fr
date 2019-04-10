---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: Sérialisation JSON et XML dans l’API Web ASP.NET - ASP.NET 4.x
author: MikeWasson
description: Décrit les formateurs JSON et XML dans l’API Web ASP.NET pour ASP.NET 4.x.
ms.author: riande
ms.date: 05/30/2012
ms.custom: seoapril2019
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: a9e7ed63a55c146976e0221214e722f3a2292fee
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59408275"
---
# <a name="json-and-xml-serialization-in-aspnet-web-api"></a><span data-ttu-id="99e59-103">Sérialisation JSON et XML dans l’API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="99e59-103">JSON and XML Serialization in ASP.NET Web API</span></span>

<span data-ttu-id="99e59-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="99e59-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="99e59-105">Cet article décrit les formateurs JSON et XML dans l’API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="99e59-105">This article describes the JSON and XML formatters in ASP.NET Web API.</span></span>

<span data-ttu-id="99e59-106">Dans l’API Web ASP.NET, un *formateur de type de média* est un objet qui peut :</span><span class="sxs-lookup"><span data-stu-id="99e59-106">In ASP.NET Web API, a *media-type formatter* is an object that can:</span></span>

- <span data-ttu-id="99e59-107">Objets CLR de la lecture à partir de HTTP de corps du message</span><span class="sxs-lookup"><span data-stu-id="99e59-107">Read CLR objects from an HTTP message body</span></span>
- <span data-ttu-id="99e59-108">Écrire des objets CLR dans un corps de message HTTP</span><span class="sxs-lookup"><span data-stu-id="99e59-108">Write CLR objects into an HTTP message body</span></span>

<span data-ttu-id="99e59-109">API Web fournit les formateurs de type de média pour JSON et XML.</span><span class="sxs-lookup"><span data-stu-id="99e59-109">Web API provides media-type formatters for both JSON and XML.</span></span> <span data-ttu-id="99e59-110">Le framework insère ces formateurs dans le pipeline par défaut.</span><span class="sxs-lookup"><span data-stu-id="99e59-110">The framework inserts these formatters into the pipeline by default.</span></span> <span data-ttu-id="99e59-111">Les clients peuvent demander au format JSON ou XML dans l’en-tête Accept de la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="99e59-111">Clients can request either JSON or XML in the Accept header of the HTTP request.</span></span>

## <a name="contents"></a><span data-ttu-id="99e59-112">Sommaire</span><span class="sxs-lookup"><span data-stu-id="99e59-112">Contents</span></span>

- [<span data-ttu-id="99e59-113">Formateur de Type de média JSON</span><span class="sxs-lookup"><span data-stu-id="99e59-113">JSON Media-Type Formatter</span></span>](#json_media_type_formatter)

    - [<span data-ttu-id="99e59-114">Propriétés en lecture seule</span><span class="sxs-lookup"><span data-stu-id="99e59-114">Read-Only Properties</span></span>](#json_readonly)
    - [<span data-ttu-id="99e59-115">Dates</span><span class="sxs-lookup"><span data-stu-id="99e59-115">Dates</span></span>](#json_dates)
    - [<span data-ttu-id="99e59-116">Mise en retrait</span><span class="sxs-lookup"><span data-stu-id="99e59-116">Indenting</span></span>](#json_indenting)
    - [<span data-ttu-id="99e59-117">Casse mixte</span><span class="sxs-lookup"><span data-stu-id="99e59-117">Camel Casing</span></span>](#json_camelcasing)
    - [<span data-ttu-id="99e59-118">Objets anonymes et faiblement typé</span><span class="sxs-lookup"><span data-stu-id="99e59-118">Anonymous and Weakly-Typed Objects</span></span>](#json_anon)
- [<span data-ttu-id="99e59-119">Formateur de Type de média XML</span><span class="sxs-lookup"><span data-stu-id="99e59-119">XML Media-Type Formatter</span></span>](#xml_media_type_formatter)

    - [<span data-ttu-id="99e59-120">Propriétés en lecture seule</span><span class="sxs-lookup"><span data-stu-id="99e59-120">Read-Only Properties</span></span>](#xml_readonly)
    - [<span data-ttu-id="99e59-121">Dates</span><span class="sxs-lookup"><span data-stu-id="99e59-121">Dates</span></span>](#xml_dates)
    - [<span data-ttu-id="99e59-122">Mise en retrait</span><span class="sxs-lookup"><span data-stu-id="99e59-122">Indenting</span></span>](#xml_indenting)
    - [<span data-ttu-id="99e59-123">Sérialiseurs XML par Type de paramètre</span><span class="sxs-lookup"><span data-stu-id="99e59-123">Setting Per-Type XML Serializers</span></span>](#xml_pertype)
- [<span data-ttu-id="99e59-124">Supprimer le JSON ou un formateur XML</span><span class="sxs-lookup"><span data-stu-id="99e59-124">Removing the JSON or XML Formatter</span></span>](#removing_the_json_or_xml_formatter)
- [<span data-ttu-id="99e59-125">Gestion des références d’objet circulaires</span><span class="sxs-lookup"><span data-stu-id="99e59-125">Handling Circular Object References</span></span>](#handling_circular_object_references)
- [<span data-ttu-id="99e59-126">Sérialisation des objets de test</span><span class="sxs-lookup"><span data-stu-id="99e59-126">Testing Object Serialization</span></span>](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a><span data-ttu-id="99e59-127">Formateur de Type de média JSON</span><span class="sxs-lookup"><span data-stu-id="99e59-127">JSON Media-Type Formatter</span></span>

<span data-ttu-id="99e59-128">Mise en forme de JSON est fournie par le **JsonMediaTypeFormatter** classe.</span><span class="sxs-lookup"><span data-stu-id="99e59-128">JSON formatting is provided by the **JsonMediaTypeFormatter** class.</span></span> <span data-ttu-id="99e59-129">Par défaut, **JsonMediaTypeFormatter** utilise le [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) bibliothèque pour effectuer la sérialisation.</span><span class="sxs-lookup"><span data-stu-id="99e59-129">By default, **JsonMediaTypeFormatter** uses the [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) library to perform serialization.</span></span> <span data-ttu-id="99e59-130">Json.NET est un projet open source de tiers.</span><span class="sxs-lookup"><span data-stu-id="99e59-130">Json.NET is a third-party open source project.</span></span>

<span data-ttu-id="99e59-131">Si vous préférez, vous pouvez configurer le **JsonMediaTypeFormatter** classe à utiliser le **DataContractJsonSerializer** au lieu de Json.NET.</span><span class="sxs-lookup"><span data-stu-id="99e59-131">If you prefer, you can configure the **JsonMediaTypeFormatter** class to use the **DataContractJsonSerializer** instead of Json.NET.</span></span> <span data-ttu-id="99e59-132">Pour ce faire, définissez la **UseDataContractJsonSerializer** propriété **true**:</span><span class="sxs-lookup"><span data-stu-id="99e59-132">To do so, set the **UseDataContractJsonSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a><span data-ttu-id="99e59-133">Sérialisation JSON</span><span class="sxs-lookup"><span data-stu-id="99e59-133">JSON Serialization</span></span>

<span data-ttu-id="99e59-134">Cette section décrit certains des comportements spécifiques du module de formatage JSON, à l’aide de la valeur par défaut [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) sérialiseur.</span><span class="sxs-lookup"><span data-stu-id="99e59-134">This section describes some specific behaviors of the JSON formatter, using the default [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializer.</span></span> <span data-ttu-id="99e59-135">Cela n’est pas censée être une documentation complète de la bibliothèque de Json.NET ; Pour plus d’informations, consultez le [Documentation Json.NET](http://james.newtonking.com/projects/json/help/).</span><span class="sxs-lookup"><span data-stu-id="99e59-135">This is not meant to be comprehensive documentation of the Json.NET library; for more information, see the [Json.NET Documentation](http://james.newtonking.com/projects/json/help/).</span></span>

#### <a name="what-gets-serialized"></a><span data-ttu-id="99e59-136">Éléments sérialisés ?</span><span class="sxs-lookup"><span data-stu-id="99e59-136">What Gets Serialized?</span></span>

<span data-ttu-id="99e59-137">Par défaut, toutes les propriétés et champs publics sont inclus dans le JSON sérialisé.</span><span class="sxs-lookup"><span data-stu-id="99e59-137">By default, all public properties and fields are included in the serialized JSON.</span></span> <span data-ttu-id="99e59-138">Pour omettre une propriété ou un champ, la décorer avec le **JsonIgnore** attribut.</span><span class="sxs-lookup"><span data-stu-id="99e59-138">To omit a property or field, decorate it with the **JsonIgnore** attribute.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

<span data-ttu-id="99e59-139">Si vous préférez un &quot;participer&quot; approche, décorer la classe avec le **DataContract** attribut.</span><span class="sxs-lookup"><span data-stu-id="99e59-139">If you prefer an &quot;opt-in&quot; approach, decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="99e59-140">Si cet attribut est présent, les membres sont ignorés sauf s’ils ont le **DataMember**.</span><span class="sxs-lookup"><span data-stu-id="99e59-140">If this attribute is present, members are ignored unless they have the **DataMember**.</span></span> <span data-ttu-id="99e59-141">Vous pouvez également utiliser **DataMember** pour sérialiser des membres privés.</span><span class="sxs-lookup"><span data-stu-id="99e59-141">You can also use **DataMember** to serialize private members.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="99e59-142">Propriétés en lecture seule</span><span class="sxs-lookup"><span data-stu-id="99e59-142">Read-Only Properties</span></span>

<span data-ttu-id="99e59-143">Propriétés en lecture seule sont sérialisées par défaut.</span><span class="sxs-lookup"><span data-stu-id="99e59-143">Read-only properties are serialized by default.</span></span>

<a id="json_dates"></a>
### <a name="dates"></a><span data-ttu-id="99e59-144">Dates</span><span class="sxs-lookup"><span data-stu-id="99e59-144">Dates</span></span>

<span data-ttu-id="99e59-145">Par défaut, Json.NET écrit des dates [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format.</span><span class="sxs-lookup"><span data-stu-id="99e59-145">By default, Json.NET writes dates in [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format.</span></span> <span data-ttu-id="99e59-146">Les dates au format UTC (Coordinated Universal Time) sont écrites avec un suffixe « Z ».</span><span class="sxs-lookup"><span data-stu-id="99e59-146">Dates in UTC (Coordinated Universal Time) are written with a "Z" suffix.</span></span> <span data-ttu-id="99e59-147">Dates en heure locale incluent un décalage de fuseau horaire.</span><span class="sxs-lookup"><span data-stu-id="99e59-147">Dates in local time include a time-zone offset.</span></span> <span data-ttu-id="99e59-148">Exemple :</span><span class="sxs-lookup"><span data-stu-id="99e59-148">For example:</span></span>

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

<span data-ttu-id="99e59-149">Par défaut, Json.NET conserve le fuseau horaire.</span><span class="sxs-lookup"><span data-stu-id="99e59-149">By default, Json.NET preserves the time zone.</span></span> <span data-ttu-id="99e59-150">Vous pouvez le remplacer en définissant la propriété DateTimeZoneHandling :</span><span class="sxs-lookup"><span data-stu-id="99e59-150">You can override this by setting the DateTimeZoneHandling property:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

<span data-ttu-id="99e59-151">Si vous préférez utiliser [format de date Microsoft JSON](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) au lieu de la norme ISO 8601, définir le **DateFormatHandling** propriété sur les paramètres du sérialiseur :</span><span class="sxs-lookup"><span data-stu-id="99e59-151">If you prefer to use [Microsoft JSON date format](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) instead of ISO 8601, set the **DateFormatHandling** property on the serializer settings:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="99e59-152">Mise en retrait</span><span class="sxs-lookup"><span data-stu-id="99e59-152">Indenting</span></span>

<span data-ttu-id="99e59-153">Pour écrire le JSON mis en retrait, définissez le **mise en forme** à **Formatting.Indented**:</span><span class="sxs-lookup"><span data-stu-id="99e59-153">To write indented JSON, set the **Formatting** setting to **Formatting.Indented**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a><span data-ttu-id="99e59-154">Casse mixte</span><span class="sxs-lookup"><span data-stu-id="99e59-154">Camel Casing</span></span>

<span data-ttu-id="99e59-155">Pour écrire des noms de propriété JSON avec une casse mixte, sans modifier votre modèle de données, définissez la **CamelCasePropertyNamesContractResolver** sur le sérialiseur :</span><span class="sxs-lookup"><span data-stu-id="99e59-155">To write JSON property names with camel casing, without changing your data model, set the **CamelCasePropertyNamesContractResolver** on the serializer:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a><span data-ttu-id="99e59-156">Objets anonymes et faiblement typé</span><span class="sxs-lookup"><span data-stu-id="99e59-156">Anonymous and Weakly-Typed Objects</span></span>

<span data-ttu-id="99e59-157">Une méthode d’action peut retourner un objet anonyme et sa sérialisation au format JSON.</span><span class="sxs-lookup"><span data-stu-id="99e59-157">An action method can return an anonymous object and serialize it to JSON.</span></span> <span data-ttu-id="99e59-158">Exemple :</span><span class="sxs-lookup"><span data-stu-id="99e59-158">For example:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

<span data-ttu-id="99e59-159">Le corps du message de réponse contient le code JSON suivant :</span><span class="sxs-lookup"><span data-stu-id="99e59-159">The response message body will contain the following JSON:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

<span data-ttu-id="99e59-160">Si votre site web API reçoit faiblement structurées objets JSON à partir de clients, vous pouvez désérialiser le corps de la demande à un **Newtonsoft.Json.Linq.JObject** type.</span><span class="sxs-lookup"><span data-stu-id="99e59-160">If your web API receives loosely structured JSON objects from clients, you can deserialize the request body to a **Newtonsoft.Json.Linq.JObject** type.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

<span data-ttu-id="99e59-161">Toutefois, il est généralement préférable d’utiliser des objets de données fortement typées.</span><span class="sxs-lookup"><span data-stu-id="99e59-161">However, it is usually better to use strongly typed data objects.</span></span> <span data-ttu-id="99e59-162">Puis vous n’avez pas besoin analyser les données vous-même, et vous offre les avantages de la validation du modèle.</span><span class="sxs-lookup"><span data-stu-id="99e59-162">Then you don't need to parse the data yourself, and you get the benefits of model validation.</span></span>

<span data-ttu-id="99e59-163">Le sérialiseur XML ne prend pas en charge les types anonymes ou **JObject** instances.</span><span class="sxs-lookup"><span data-stu-id="99e59-163">The XML serializer does not support anonymous types or **JObject** instances.</span></span> <span data-ttu-id="99e59-164">Si vous utilisez ces fonctionnalités pour vos données JSON, vous devez supprimer le formateur XML provenant du pipeline, comme décrit plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="99e59-164">If you use these features for your JSON data, you should remove the XML formatter from the pipeline, as described later in this article.</span></span>

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a><span data-ttu-id="99e59-165">Formateur de Type de média XML</span><span class="sxs-lookup"><span data-stu-id="99e59-165">XML Media-Type Formatter</span></span>

<span data-ttu-id="99e59-166">Mise en forme XML est fournie par le **XmlMediaTypeFormatter** classe.</span><span class="sxs-lookup"><span data-stu-id="99e59-166">XML formatting is provided by the **XmlMediaTypeFormatter** class.</span></span> <span data-ttu-id="99e59-167">Par défaut, **XmlMediaTypeFormatter** utilise le **DataContractSerializer** classe pour effectuer la sérialisation.</span><span class="sxs-lookup"><span data-stu-id="99e59-167">By default, **XmlMediaTypeFormatter** uses the **DataContractSerializer** class to perform serialization.</span></span>

<span data-ttu-id="99e59-168">Si vous préférez, vous pouvez configurer le **XmlMediaTypeFormatter** à utiliser le **XmlSerializer** au lieu du **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="99e59-168">If you prefer, you can configure the **XmlMediaTypeFormatter** to use the **XmlSerializer** instead of the **DataContractSerializer**.</span></span> <span data-ttu-id="99e59-169">Pour ce faire, définissez la **/usexmlserializer** propriété **true**:</span><span class="sxs-lookup"><span data-stu-id="99e59-169">To do so, set the **UseXmlSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

<span data-ttu-id="99e59-170">Le **XmlSerializer** classe prend en charge un ensemble plus restreint de types que **DataContractSerializer**, mais offre davantage de contrôle sur le document XML obtenu.</span><span class="sxs-lookup"><span data-stu-id="99e59-170">The **XmlSerializer** class supports a narrower set of types than **DataContractSerializer**, but gives more control over the resulting XML.</span></span> <span data-ttu-id="99e59-171">Envisagez d’utiliser **XmlSerializer** si vous avez besoin faire correspondre un schéma XML existant.</span><span class="sxs-lookup"><span data-stu-id="99e59-171">Consider using **XmlSerializer** if you need to match an existing XML schema.</span></span>

### <a name="xml-serialization"></a><span data-ttu-id="99e59-172">Sérialisation XML</span><span class="sxs-lookup"><span data-stu-id="99e59-172">XML Serialization</span></span>

<span data-ttu-id="99e59-173">Cette section décrit certains des comportements spécifiques du formateur XML, à l’aide de la valeur par défaut **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="99e59-173">This section describes some specific behaviors of the XML formatter, using the default **DataContractSerializer**.</span></span>

<span data-ttu-id="99e59-174">Par défaut, le DataContractSerializer se comporte comme suit :</span><span class="sxs-lookup"><span data-stu-id="99e59-174">By default, the DataContractSerializer behaves as follows:</span></span>

- <span data-ttu-id="99e59-175">Tous les champs et propriétés de lecture/écriture publique sont sérialisés.</span><span class="sxs-lookup"><span data-stu-id="99e59-175">All public read/write properties and fields are serialized.</span></span> <span data-ttu-id="99e59-176">Pour omettre une propriété ou un champ, la décorer avec le **IgnoreDataMember** attribut.</span><span class="sxs-lookup"><span data-stu-id="99e59-176">To omit a property or field, decorate it with the **IgnoreDataMember** attribute.</span></span>
- <span data-ttu-id="99e59-177">Membres privés et protégés ne sont pas sérialisées.</span><span class="sxs-lookup"><span data-stu-id="99e59-177">Private and protected members are not serialized.</span></span>
- <span data-ttu-id="99e59-178">Propriétés en lecture seule ne sont pas sérialisées.</span><span class="sxs-lookup"><span data-stu-id="99e59-178">Read-only properties are not serialized.</span></span> <span data-ttu-id="99e59-179">(Toutefois, le contenu d’une propriété de collection en lecture seule est sérialisé.)</span><span class="sxs-lookup"><span data-stu-id="99e59-179">(However, the contents of a read-only collection property are serialized.)</span></span>
- <span data-ttu-id="99e59-180">Noms de membre et de classe sont écrits dans le code XML exactement comme elles apparaissent dans la déclaration de classe.</span><span class="sxs-lookup"><span data-stu-id="99e59-180">Class and member names are written in the XML exactly as they appear in the class declaration.</span></span>
- <span data-ttu-id="99e59-181">Un espace de noms XML par défaut est utilisé.</span><span class="sxs-lookup"><span data-stu-id="99e59-181">A default XML namespace is used.</span></span>

<span data-ttu-id="99e59-182">Si vous avez besoin de mieux contrôler la sérialisation, vous pouvez décorer la classe avec le **DataContract** attribut.</span><span class="sxs-lookup"><span data-stu-id="99e59-182">If you need more control over the serialization, you can decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="99e59-183">Lorsque cet attribut est présent, la classe est sérialisée comme suit :</span><span class="sxs-lookup"><span data-stu-id="99e59-183">When this attribute is present, the class is serialized as follows:</span></span>

- <span data-ttu-id="99e59-184">&quot;Participer&quot; approche : Propriétés et champs ne sont pas sérialisés par défaut.</span><span class="sxs-lookup"><span data-stu-id="99e59-184">&quot;Opt in&quot; approach: Properties and fields are not serialized by default.</span></span> <span data-ttu-id="99e59-185">Pour sérialiser une propriété ou un champ, la décorer avec le **DataMember** attribut.</span><span class="sxs-lookup"><span data-stu-id="99e59-185">To serialize a property or field, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="99e59-186">Pour sérialiser un membre privé ou protégé, la décorer avec le **DataMember** attribut.</span><span class="sxs-lookup"><span data-stu-id="99e59-186">To serialize a private or protected member, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="99e59-187">Propriétés en lecture seule ne sont pas sérialisées.</span><span class="sxs-lookup"><span data-stu-id="99e59-187">Read-only properties are not serialized.</span></span>
- <span data-ttu-id="99e59-188">Pour modifier la façon dont le nom de classe s’affiche dans le code XML, définissez le *nom* paramètre dans le **DataContract** attribut.</span><span class="sxs-lookup"><span data-stu-id="99e59-188">To change how the class name appears in the XML, set the *Name* parameter in the **DataContract** attribute.</span></span>
- <span data-ttu-id="99e59-189">Pour modifier la façon dont un nom de membre s’affiche dans le code XML, définissez le *nom* paramètre dans le **DataMember** attribut.</span><span class="sxs-lookup"><span data-stu-id="99e59-189">To change how a member name appears in the XML, set the *Name* parameter in the **DataMember** attribute.</span></span>
- <span data-ttu-id="99e59-190">Pour modifier l’espace de noms XML, définissez le *Namespace* paramètre dans le **DataContract** classe.</span><span class="sxs-lookup"><span data-stu-id="99e59-190">To change the XML namespace, set the *Namespace* parameter in the **DataContract** class.</span></span>

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="99e59-191">Propriétés en lecture seule</span><span class="sxs-lookup"><span data-stu-id="99e59-191">Read-Only Properties</span></span>

<span data-ttu-id="99e59-192">Propriétés en lecture seule ne sont pas sérialisées.</span><span class="sxs-lookup"><span data-stu-id="99e59-192">Read-only properties are not serialized.</span></span> <span data-ttu-id="99e59-193">Si une propriété en lecture seule a un champ de stockage privé, vous pouvez marquer le champ privé avec les **DataMember** attribut.</span><span class="sxs-lookup"><span data-stu-id="99e59-193">If a read-only property has a backing private field, you can mark the private field with the **DataMember** attribute.</span></span> <span data-ttu-id="99e59-194">Cette approche nécessite la **DataContract** attribut sur la classe.</span><span class="sxs-lookup"><span data-stu-id="99e59-194">This approach requires the **DataContract** attribute on the class.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a><span data-ttu-id="99e59-195">Dates</span><span class="sxs-lookup"><span data-stu-id="99e59-195">Dates</span></span>

<span data-ttu-id="99e59-196">Les dates sont écrites au format ISO 8601.</span><span class="sxs-lookup"><span data-stu-id="99e59-196">Dates are written in ISO 8601 format.</span></span> <span data-ttu-id="99e59-197">Par exemple, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span><span class="sxs-lookup"><span data-stu-id="99e59-197">For example, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span></span>

<a id="xml_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="99e59-198">Mise en retrait</span><span class="sxs-lookup"><span data-stu-id="99e59-198">Indenting</span></span>

<span data-ttu-id="99e59-199">Pour écrire du code XML mis en retrait, définir le **mettre en retrait** propriété **true**:</span><span class="sxs-lookup"><span data-stu-id="99e59-199">To write indented XML, set the **Indent** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a><span data-ttu-id="99e59-200">Sérialiseurs XML par Type de paramètre</span><span class="sxs-lookup"><span data-stu-id="99e59-200">Setting Per-Type XML Serializers</span></span>

<span data-ttu-id="99e59-201">Vous pouvez définir différents sérialiseurs XML pour les différents types CLR.</span><span class="sxs-lookup"><span data-stu-id="99e59-201">You can set different XML serializers for different CLR types.</span></span> <span data-ttu-id="99e59-202">Par exemple, vous pouvez avoir un objet de données particulière nécessite **XmlSerializer** pour la compatibilité descendante.</span><span class="sxs-lookup"><span data-stu-id="99e59-202">For example, you might have a particular data object that requires **XmlSerializer** for backward compatibility.</span></span> <span data-ttu-id="99e59-203">Vous pouvez utiliser **XmlSerializer** pour cet objet et continuer à utiliser **DataContractSerializer** pour d’autres types.</span><span class="sxs-lookup"><span data-stu-id="99e59-203">You can use **XmlSerializer** for this object and continue to use **DataContractSerializer** for other types.</span></span>

<span data-ttu-id="99e59-204">Pour définir un sérialiseur XML pour un type particulier, appelez **SetSerializer**.</span><span class="sxs-lookup"><span data-stu-id="99e59-204">To set an XML serializer for a particular type, call **SetSerializer**.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

<span data-ttu-id="99e59-205">Vous pouvez spécifier un **XmlSerializer** ou n’importe quel objet qui dérive de **XmlObjectSerializer**.</span><span class="sxs-lookup"><span data-stu-id="99e59-205">You can specify an **XmlSerializer** or any object that derives from **XmlObjectSerializer**.</span></span>

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a><span data-ttu-id="99e59-206">Supprimer le JSON ou un formateur XML</span><span class="sxs-lookup"><span data-stu-id="99e59-206">Removing the JSON or XML Formatter</span></span>

<span data-ttu-id="99e59-207">Vous pouvez supprimer le formateur JSON ou le formateur XML à partir de la liste des formateurs, si vous ne souhaitez pas les utiliser.</span><span class="sxs-lookup"><span data-stu-id="99e59-207">You can remove the JSON formatter or the XML formatter from the list of formatters, if you do not want to use them.</span></span> <span data-ttu-id="99e59-208">Les principales raisons à cela sont :</span><span class="sxs-lookup"><span data-stu-id="99e59-208">The main reasons to do this are:</span></span>

- <span data-ttu-id="99e59-209">Pour restreindre les réponses de l’API web à un type de média spécifique.</span><span class="sxs-lookup"><span data-stu-id="99e59-209">To restrict your web API responses to a particular media type.</span></span> <span data-ttu-id="99e59-210">Par exemple, vous pouvez décider prendre en charge uniquement les réponses JSON et supprimer le formateur XML.</span><span class="sxs-lookup"><span data-stu-id="99e59-210">For example, you might decide to support only JSON responses, and remove the XML formatter.</span></span>
- <span data-ttu-id="99e59-211">Pour remplacer le formateur par défaut par un formateur personnalisé.</span><span class="sxs-lookup"><span data-stu-id="99e59-211">To replace the default formatter with a custom formatter.</span></span> <span data-ttu-id="99e59-212">Par exemple, vous pouvez remplacer le formateur JSON avec votre propre implémentation personnalisée d’un module de formatage JSON.</span><span class="sxs-lookup"><span data-stu-id="99e59-212">For example, you could replace the JSON formatter with your own custom implementation of a JSON formatter.</span></span>

<span data-ttu-id="99e59-213">Le code suivant montre comment supprimer les formateurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="99e59-213">The following code shows how to remove the default formatters.</span></span> <span data-ttu-id="99e59-214">Appeler à partir votre **Application\_Démarrer** méthode, définie dans Global.asax.</span><span class="sxs-lookup"><span data-stu-id="99e59-214">Call this from your **Application\_Start** method, defined in Global.asax.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a><span data-ttu-id="99e59-215">Gestion des références d’objet circulaires</span><span class="sxs-lookup"><span data-stu-id="99e59-215">Handling Circular Object References</span></span>

<span data-ttu-id="99e59-216">Par défaut, les formateurs JSON et XML écrivent tous les objets en tant que valeurs.</span><span class="sxs-lookup"><span data-stu-id="99e59-216">By default, the JSON and XML formatters write all objects as values.</span></span> <span data-ttu-id="99e59-217">Si les deux propriétés font référence au même objet, ou si le même objet apparaît deux fois dans une collection, le formateur sérialise l’objet à deux reprises.</span><span class="sxs-lookup"><span data-stu-id="99e59-217">If two properties refer to the same object, or if the same object appears twice in a collection, the formatter will serialize the object twice.</span></span> <span data-ttu-id="99e59-218">Il s’agit une problématique en particulier si votre graphique d’objet contient des cycles, étant donné que le sérialiseur lève une exception lorsqu’il détecte une boucle dans le graphique.</span><span class="sxs-lookup"><span data-stu-id="99e59-218">This is a particular problem if your object graph contains cycles, because the serializer will throw an exception when it detects a loop in the graph.</span></span>

<span data-ttu-id="99e59-219">Tenez compte des modèles d’objet suivants et contrôleur.</span><span class="sxs-lookup"><span data-stu-id="99e59-219">Consider the following object models and controller.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

<span data-ttu-id="99e59-220">Appel de cette action provoque le formateur à levé une exception, ce qui se traduit par un état code 500 (erreur interne du serveur) réponse au client.</span><span class="sxs-lookup"><span data-stu-id="99e59-220">Invoking this action will cause the formatter to thrown an exception, which translates to a status code 500 (Internal Server Error) response to the client.</span></span>

<span data-ttu-id="99e59-221">Pour conserver les références d’objet dans JSON, ajoutez le code suivant à **Application\_Démarrer** méthode dans le fichier Global.asax :</span><span class="sxs-lookup"><span data-stu-id="99e59-221">To preserve object references in JSON, add the following code to **Application\_Start** method in the Global.asax file:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

<span data-ttu-id="99e59-222">L’action du contrôleur retournent désormais JSON ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="99e59-222">Now the controller action will return JSON that looks like this:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

<span data-ttu-id="99e59-223">Notez que le sérialiseur ajoute un &quot;$id&quot; propriété pour les deux objets.</span><span class="sxs-lookup"><span data-stu-id="99e59-223">Notice that the serializer adds an &quot;$id&quot; property to both objects.</span></span> <span data-ttu-id="99e59-224">En outre, il détecte que la propriété Employee.Department crée une boucle, elle remplace la valeur par une référence d’objet : {&quot;$ref&quot;:&quot;1&quot;}.</span><span class="sxs-lookup"><span data-stu-id="99e59-224">Also, it detects that the Employee.Department property creates a loop, so it replaces the value with an object reference: {&quot;$ref&quot;:&quot;1&quot;}.</span></span>

> [!NOTE]
> <span data-ttu-id="99e59-225">Références d’objet ne sont pas standard au format JSON.</span><span class="sxs-lookup"><span data-stu-id="99e59-225">Object references are not standard in JSON.</span></span> <span data-ttu-id="99e59-226">Avant d’utiliser cette fonctionnalité, réfléchissez aux si vos clients seront en mesure d’analyser les résultats.</span><span class="sxs-lookup"><span data-stu-id="99e59-226">Before using this feature, consider whether your clients will be able to parse the results.</span></span> <span data-ttu-id="99e59-227">Il peut être préférable de ne supprimer cycles du graphique.</span><span class="sxs-lookup"><span data-stu-id="99e59-227">It might be better simply to remove cycles from the graph.</span></span> <span data-ttu-id="99e59-228">Par exemple, le lien d’employé au département n’est pas vraiment nécessaire dans cet exemple.</span><span class="sxs-lookup"><span data-stu-id="99e59-228">For example, the link from Employee back to Department is not really needed in this example.</span></span>


<span data-ttu-id="99e59-229">Pour conserver les références d’objet dans XML, vous avez deux options.</span><span class="sxs-lookup"><span data-stu-id="99e59-229">To preserve object references in XML, you have two options.</span></span> <span data-ttu-id="99e59-230">L’option la plus simple consiste à ajouter `[DataContract(IsReference=true)]` à votre classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="99e59-230">The simpler option is to add `[DataContract(IsReference=true)]` to your model class.</span></span> <span data-ttu-id="99e59-231">Le *IsReference* paramètre permet de références d’objet.</span><span class="sxs-lookup"><span data-stu-id="99e59-231">The *IsReference* parameter enables object references.</span></span> <span data-ttu-id="99e59-232">N’oubliez pas que **DataContract** rend sérialisation participer, donc vous devrez également ajouter **DataMember** attributs aux propriétés :</span><span class="sxs-lookup"><span data-stu-id="99e59-232">Remember that **DataContract** makes serialization opt-in, so you will also need to add **DataMember** attributes to the properties:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

<span data-ttu-id="99e59-233">Maintenant le formateur produira XML semblable au suivant :</span><span class="sxs-lookup"><span data-stu-id="99e59-233">Now the formatter will produce XML similar to following:</span></span>

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

<span data-ttu-id="99e59-234">Si vous souhaitez éviter les attributs sur votre classe de modèle, il existe une autre option : Créer un nouveau spécifiques au type **DataContractSerializer** d’instance et définissez *preserveObjectReferences* à **true** dans le constructeur.</span><span class="sxs-lookup"><span data-stu-id="99e59-234">If you want to avoid attributes on your model class, there is another option: Create a new type-specific **DataContractSerializer** instance and set *preserveObjectReferences* to **true** in the constructor.</span></span> <span data-ttu-id="99e59-235">Puis définissez cette instance comme un sérialiseur par type sur le formateur de type de média XML.</span><span class="sxs-lookup"><span data-stu-id="99e59-235">Then set this instance as a per-type serializer on the XML media-type formatter.</span></span> <span data-ttu-id="99e59-236">Le code suivant montre comment procéder :</span><span class="sxs-lookup"><span data-stu-id="99e59-236">The following code show how to do this:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a><span data-ttu-id="99e59-237">Sérialisation des objets de test</span><span class="sxs-lookup"><span data-stu-id="99e59-237">Testing Object Serialization</span></span>

<span data-ttu-id="99e59-238">Lorsque vous concevez votre API web, il est utile tester comment vos objets de données seront sérialisés.</span><span class="sxs-lookup"><span data-stu-id="99e59-238">As you design your web API, it is useful to test how your data objects will be serialized.</span></span> <span data-ttu-id="99e59-239">Vous pouvez faire cela sans création d’un contrôleur ou d’appel d’une action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="99e59-239">You can do this without creating a controller or invoking a controller action.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
