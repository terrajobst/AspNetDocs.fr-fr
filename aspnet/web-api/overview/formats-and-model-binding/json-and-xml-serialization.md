---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: Sérialisation JSON et XML dans API Web ASP.NET-ASP.NET 4. x
author: MikeWasson
description: Décrit les formateurs JSON et XML dans API Web ASP.NET pour ASP.NET 4. x.
ms.author: riande
ms.date: 05/30/2012
ms.custom: seoapril2019
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: 00fa07f00eabf7e6c883c5e9ceaf9a38a8f49605
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557469"
---
# <a name="json-and-xml-serialization-in-aspnet-web-api"></a><span data-ttu-id="cf9da-103">Sérialisation JSON et XML dans API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cf9da-103">JSON and XML Serialization in ASP.NET Web API</span></span>

<span data-ttu-id="cf9da-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="cf9da-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="cf9da-105">Cet article décrit les formateurs JSON et XML dans API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="cf9da-105">This article describes the JSON and XML formatters in ASP.NET Web API.</span></span>

<span data-ttu-id="cf9da-106">Dans API Web ASP.NET, un *formateur de type de média* est un objet qui peut :</span><span class="sxs-lookup"><span data-stu-id="cf9da-106">In ASP.NET Web API, a *media-type formatter* is an object that can:</span></span>

- <span data-ttu-id="cf9da-107">Lire des objets CLR à partir d’un corps de message HTTP</span><span class="sxs-lookup"><span data-stu-id="cf9da-107">Read CLR objects from an HTTP message body</span></span>
- <span data-ttu-id="cf9da-108">Écrire des objets CLR dans le corps d’un message HTTP</span><span class="sxs-lookup"><span data-stu-id="cf9da-108">Write CLR objects into an HTTP message body</span></span>

<span data-ttu-id="cf9da-109">L’API Web fournit des formateurs de type de média pour JSON et XML.</span><span class="sxs-lookup"><span data-stu-id="cf9da-109">Web API provides media-type formatters for both JSON and XML.</span></span> <span data-ttu-id="cf9da-110">L’infrastructure insère ces formateurs dans le pipeline par défaut.</span><span class="sxs-lookup"><span data-stu-id="cf9da-110">The framework inserts these formatters into the pipeline by default.</span></span> <span data-ttu-id="cf9da-111">Les clients peuvent demander JSON ou XML dans l’en-tête Accept de la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="cf9da-111">Clients can request either JSON or XML in the Accept header of the HTTP request.</span></span>

## <a name="contents"></a><span data-ttu-id="cf9da-112">Contenu</span><span class="sxs-lookup"><span data-stu-id="cf9da-112">Contents</span></span>

- [<span data-ttu-id="cf9da-113">Formateur de type de média JSON</span><span class="sxs-lookup"><span data-stu-id="cf9da-113">JSON Media-Type Formatter</span></span>](#json_media_type_formatter)

    - [<span data-ttu-id="cf9da-114">Propriétés en lecture seule</span><span class="sxs-lookup"><span data-stu-id="cf9da-114">Read-Only Properties</span></span>](#json_readonly)
    - [<span data-ttu-id="cf9da-115">Indiquée</span><span class="sxs-lookup"><span data-stu-id="cf9da-115">Dates</span></span>](#json_dates)
    - [<span data-ttu-id="cf9da-116">Mise en retrait</span><span class="sxs-lookup"><span data-stu-id="cf9da-116">Indenting</span></span>](#json_indenting)
    - [<span data-ttu-id="cf9da-117">Casse mixte</span><span class="sxs-lookup"><span data-stu-id="cf9da-117">Camel Casing</span></span>](#json_camelcasing)
    - [<span data-ttu-id="cf9da-118">Objets anonymes et faiblement typés</span><span class="sxs-lookup"><span data-stu-id="cf9da-118">Anonymous and Weakly-Typed Objects</span></span>](#json_anon)
- [<span data-ttu-id="cf9da-119">Formateur de type de média XML</span><span class="sxs-lookup"><span data-stu-id="cf9da-119">XML Media-Type Formatter</span></span>](#xml_media_type_formatter)

    - [<span data-ttu-id="cf9da-120">Propriétés en lecture seule</span><span class="sxs-lookup"><span data-stu-id="cf9da-120">Read-Only Properties</span></span>](#xml_readonly)
    - [<span data-ttu-id="cf9da-121">Indiquée</span><span class="sxs-lookup"><span data-stu-id="cf9da-121">Dates</span></span>](#xml_dates)
    - [<span data-ttu-id="cf9da-122">Mise en retrait</span><span class="sxs-lookup"><span data-stu-id="cf9da-122">Indenting</span></span>](#xml_indenting)
    - [<span data-ttu-id="cf9da-123">Définition de sérialiseurs XML par type</span><span class="sxs-lookup"><span data-stu-id="cf9da-123">Setting Per-Type XML Serializers</span></span>](#xml_pertype)
- [<span data-ttu-id="cf9da-124">Suppression du formateur JSON ou XML</span><span class="sxs-lookup"><span data-stu-id="cf9da-124">Removing the JSON or XML Formatter</span></span>](#removing_the_json_or_xml_formatter)
- [<span data-ttu-id="cf9da-125">Gestion des références d’objets circulaires</span><span class="sxs-lookup"><span data-stu-id="cf9da-125">Handling Circular Object References</span></span>](#handling_circular_object_references)
- [<span data-ttu-id="cf9da-126">Test de la sérialisation d’un objet</span><span class="sxs-lookup"><span data-stu-id="cf9da-126">Testing Object Serialization</span></span>](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a><span data-ttu-id="cf9da-127">Formateur de type de média JSON</span><span class="sxs-lookup"><span data-stu-id="cf9da-127">JSON Media-Type Formatter</span></span>

<span data-ttu-id="cf9da-128">La mise en forme JSON est fournie par la classe **JsonMediaTypeFormatter** .</span><span class="sxs-lookup"><span data-stu-id="cf9da-128">JSON formatting is provided by the **JsonMediaTypeFormatter** class.</span></span> <span data-ttu-id="cf9da-129">Par défaut, **JsonMediaTypeFormatter** utilise la bibliothèque [JSON.net](https://github.com/JamesNK/Newtonsoft.Json) pour effectuer la sérialisation.</span><span class="sxs-lookup"><span data-stu-id="cf9da-129">By default, **JsonMediaTypeFormatter** uses the [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) library to perform serialization.</span></span> <span data-ttu-id="cf9da-130">Json.NET est un projet open source tiers.</span><span class="sxs-lookup"><span data-stu-id="cf9da-130">Json.NET is a third-party open source project.</span></span>

<span data-ttu-id="cf9da-131">Si vous préférez, vous pouvez configurer la classe **JsonMediaTypeFormatter** pour utiliser **DataContractJsonSerializer** au lieu de JSON.net.</span><span class="sxs-lookup"><span data-stu-id="cf9da-131">If you prefer, you can configure the **JsonMediaTypeFormatter** class to use the **DataContractJsonSerializer** instead of Json.NET.</span></span> <span data-ttu-id="cf9da-132">Pour ce faire, affectez la valeur **true**à la propriété **UseDataContractJsonSerializer** :</span><span class="sxs-lookup"><span data-stu-id="cf9da-132">To do so, set the **UseDataContractJsonSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a><span data-ttu-id="cf9da-133">Sérialisation JSON</span><span class="sxs-lookup"><span data-stu-id="cf9da-133">JSON Serialization</span></span>

<span data-ttu-id="cf9da-134">Cette section décrit certains comportements spécifiques du formateur JSON, à l’aide du sérialiseur [JSON.net](https://github.com/JamesNK/Newtonsoft.Json) par défaut.</span><span class="sxs-lookup"><span data-stu-id="cf9da-134">This section describes some specific behaviors of the JSON formatter, using the default [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializer.</span></span> <span data-ttu-id="cf9da-135">Il ne s’agit pas d’une documentation complète de la bibliothèque Json.NET. Pour plus d’informations, consultez la [Documentation JSON.net](http://james.newtonking.com/projects/json/help/).</span><span class="sxs-lookup"><span data-stu-id="cf9da-135">This is not meant to be comprehensive documentation of the Json.NET library; for more information, see the [Json.NET Documentation](http://james.newtonking.com/projects/json/help/).</span></span>

#### <a name="what-gets-serialized"></a><span data-ttu-id="cf9da-136">Que est-ce qui est sérialisé ?</span><span class="sxs-lookup"><span data-stu-id="cf9da-136">What Gets Serialized?</span></span>

<span data-ttu-id="cf9da-137">Par défaut, tous les champs et propriétés publics sont inclus dans le JSON sérialisé.</span><span class="sxs-lookup"><span data-stu-id="cf9da-137">By default, all public properties and fields are included in the serialized JSON.</span></span> <span data-ttu-id="cf9da-138">Pour omettre une propriété ou un champ, décorez-le avec l’attribut **JsonIgnore** .</span><span class="sxs-lookup"><span data-stu-id="cf9da-138">To omit a property or field, decorate it with the **JsonIgnore** attribute.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

<span data-ttu-id="cf9da-139">Si vous préférez une approche de&quot; d’abonnement &quot;, Décorez la classe avec l’attribut **DataContract** .</span><span class="sxs-lookup"><span data-stu-id="cf9da-139">If you prefer an &quot;opt-in&quot; approach, decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="cf9da-140">Si cet attribut est présent, les membres sont ignorés, sauf s’ils ont le **DataMember**.</span><span class="sxs-lookup"><span data-stu-id="cf9da-140">If this attribute is present, members are ignored unless they have the **DataMember**.</span></span> <span data-ttu-id="cf9da-141">Vous pouvez également utiliser **DataMember** pour sérialiser des membres privés.</span><span class="sxs-lookup"><span data-stu-id="cf9da-141">You can also use **DataMember** to serialize private members.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="cf9da-142">Propriétés en lecture seule</span><span class="sxs-lookup"><span data-stu-id="cf9da-142">Read-Only Properties</span></span>

<span data-ttu-id="cf9da-143">Les propriétés en lecture seule sont sérialisées par défaut.</span><span class="sxs-lookup"><span data-stu-id="cf9da-143">Read-only properties are serialized by default.</span></span>

<a id="json_dates"></a>
### <a name="dates"></a><span data-ttu-id="cf9da-144">Dates</span><span class="sxs-lookup"><span data-stu-id="cf9da-144">Dates</span></span>

<span data-ttu-id="cf9da-145">Par défaut, Json.NET écrit les dates au format [ISO 8601](http://www.w3.org/TR/NOTE-datetime) .</span><span class="sxs-lookup"><span data-stu-id="cf9da-145">By default, Json.NET writes dates in [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format.</span></span> <span data-ttu-id="cf9da-146">Les dates au format UTC (temps universel coordonné) sont écrites avec un suffixe « Z ».</span><span class="sxs-lookup"><span data-stu-id="cf9da-146">Dates in UTC (Coordinated Universal Time) are written with a "Z" suffix.</span></span> <span data-ttu-id="cf9da-147">Les dates en heure locale incluent un décalage de fuseau horaire.</span><span class="sxs-lookup"><span data-stu-id="cf9da-147">Dates in local time include a time-zone offset.</span></span> <span data-ttu-id="cf9da-148">Exemple :</span><span class="sxs-lookup"><span data-stu-id="cf9da-148">For example:</span></span>

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

<span data-ttu-id="cf9da-149">Par défaut, Json.NET conserve le fuseau horaire.</span><span class="sxs-lookup"><span data-stu-id="cf9da-149">By default, Json.NET preserves the time zone.</span></span> <span data-ttu-id="cf9da-150">Vous pouvez le remplacer en définissant la propriété DateTimeZoneHandling :</span><span class="sxs-lookup"><span data-stu-id="cf9da-150">You can override this by setting the DateTimeZoneHandling property:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

<span data-ttu-id="cf9da-151">Si vous préférez utiliser le [format de date Microsoft JSON](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) au lieu de la norme ISO 8601, définissez la propriété **DateFormatHandling** sur les paramètres du sérialiseur :</span><span class="sxs-lookup"><span data-stu-id="cf9da-151">If you prefer to use [Microsoft JSON date format](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) instead of ISO 8601, set the **DateFormatHandling** property on the serializer settings:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="cf9da-152">Mise en retrait</span><span class="sxs-lookup"><span data-stu-id="cf9da-152">Indenting</span></span>

<span data-ttu-id="cf9da-153">Pour écrire du code JSON mis en retrait, définissez le paramètre de **mise** en forme sur **mise en forme. en retrait**:</span><span class="sxs-lookup"><span data-stu-id="cf9da-153">To write indented JSON, set the **Formatting** setting to **Formatting.Indented**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a><span data-ttu-id="cf9da-154">Casse mixte</span><span class="sxs-lookup"><span data-stu-id="cf9da-154">Camel Casing</span></span>

<span data-ttu-id="cf9da-155">Pour écrire des noms de propriété JSON avec la casse mixte, sans modifier votre modèle de données, définissez **CamelCasePropertyNamesContractResolver** sur le sérialiseur :</span><span class="sxs-lookup"><span data-stu-id="cf9da-155">To write JSON property names with camel casing, without changing your data model, set the **CamelCasePropertyNamesContractResolver** on the serializer:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a><span data-ttu-id="cf9da-156">Objets anonymes et faiblement typés</span><span class="sxs-lookup"><span data-stu-id="cf9da-156">Anonymous and Weakly-Typed Objects</span></span>

<span data-ttu-id="cf9da-157">Une méthode d’action peut retourner un objet anonyme et la sérialiser au format JSON.</span><span class="sxs-lookup"><span data-stu-id="cf9da-157">An action method can return an anonymous object and serialize it to JSON.</span></span> <span data-ttu-id="cf9da-158">Exemple :</span><span class="sxs-lookup"><span data-stu-id="cf9da-158">For example:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

<span data-ttu-id="cf9da-159">Le corps du message de réponse contient le code JSON suivant :</span><span class="sxs-lookup"><span data-stu-id="cf9da-159">The response message body will contain the following JSON:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

<span data-ttu-id="cf9da-160">Si votre API Web reçoit des objets JSON faiblement structurés des clients, vous pouvez désérialiser le corps de la requête en un type **Newtonsoft. JSON. Linq. JObject** .</span><span class="sxs-lookup"><span data-stu-id="cf9da-160">If your web API receives loosely structured JSON objects from clients, you can deserialize the request body to a **Newtonsoft.Json.Linq.JObject** type.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

<span data-ttu-id="cf9da-161">Toutefois, il est généralement préférable d’utiliser des objets de données fortement typés.</span><span class="sxs-lookup"><span data-stu-id="cf9da-161">However, it is usually better to use strongly typed data objects.</span></span> <span data-ttu-id="cf9da-162">Vous n’avez pas besoin d’analyser les données vous-même et vous bénéficiez des avantages de la validation du modèle.</span><span class="sxs-lookup"><span data-stu-id="cf9da-162">Then you don't need to parse the data yourself, and you get the benefits of model validation.</span></span>

<span data-ttu-id="cf9da-163">Le sérialiseur XML ne prend pas en charge les types anonymes ou les instances **JObject** .</span><span class="sxs-lookup"><span data-stu-id="cf9da-163">The XML serializer does not support anonymous types or **JObject** instances.</span></span> <span data-ttu-id="cf9da-164">Si vous utilisez ces fonctionnalités pour vos données JSON, vous devez supprimer le formateur XML du pipeline, comme décrit plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="cf9da-164">If you use these features for your JSON data, you should remove the XML formatter from the pipeline, as described later in this article.</span></span>

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a><span data-ttu-id="cf9da-165">Formateur de type de média XML</span><span class="sxs-lookup"><span data-stu-id="cf9da-165">XML Media-Type Formatter</span></span>

<span data-ttu-id="cf9da-166">La mise en forme XML est fournie par la classe **XmlMediaTypeFormatter** .</span><span class="sxs-lookup"><span data-stu-id="cf9da-166">XML formatting is provided by the **XmlMediaTypeFormatter** class.</span></span> <span data-ttu-id="cf9da-167">Par défaut, **XmlMediaTypeFormatter** utilise la classe **DataContractSerializer** pour effectuer la sérialisation.</span><span class="sxs-lookup"><span data-stu-id="cf9da-167">By default, **XmlMediaTypeFormatter** uses the **DataContractSerializer** class to perform serialization.</span></span>

<span data-ttu-id="cf9da-168">Si vous préférez, vous pouvez configurer **XmlMediaTypeFormatter** pour utiliser le **XmlSerializer** au lieu de **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="cf9da-168">If you prefer, you can configure the **XmlMediaTypeFormatter** to use the **XmlSerializer** instead of the **DataContractSerializer**.</span></span> <span data-ttu-id="cf9da-169">Pour ce faire, affectez la valeur **true**à la propriété **UseXmlSerializer** :</span><span class="sxs-lookup"><span data-stu-id="cf9da-169">To do so, set the **UseXmlSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

<span data-ttu-id="cf9da-170">La classe **XmlSerializer** prend en charge un ensemble plus étroit de types que **DataContractSerializer**, mais offre davantage de contrôle sur le XML résultant.</span><span class="sxs-lookup"><span data-stu-id="cf9da-170">The **XmlSerializer** class supports a narrower set of types than **DataContractSerializer**, but gives more control over the resulting XML.</span></span> <span data-ttu-id="cf9da-171">Pensez à utiliser **XmlSerializer** si vous devez faire correspondre un schéma XML existant.</span><span class="sxs-lookup"><span data-stu-id="cf9da-171">Consider using **XmlSerializer** if you need to match an existing XML schema.</span></span>

### <a name="xml-serialization"></a><span data-ttu-id="cf9da-172">Sérialisation XML</span><span class="sxs-lookup"><span data-stu-id="cf9da-172">XML Serialization</span></span>

<span data-ttu-id="cf9da-173">Cette section décrit certains comportements spécifiques du formateur XML, à l’aide de **DataContractSerializer**par défaut.</span><span class="sxs-lookup"><span data-stu-id="cf9da-173">This section describes some specific behaviors of the XML formatter, using the default **DataContractSerializer**.</span></span>

<span data-ttu-id="cf9da-174">Par défaut, DataContractSerializer se comporte comme suit :</span><span class="sxs-lookup"><span data-stu-id="cf9da-174">By default, the DataContractSerializer behaves as follows:</span></span>

- <span data-ttu-id="cf9da-175">Toutes les propriétés et tous les champs publics en lecture/écriture sont sérialisés.</span><span class="sxs-lookup"><span data-stu-id="cf9da-175">All public read/write properties and fields are serialized.</span></span> <span data-ttu-id="cf9da-176">Pour omettre une propriété ou un champ, décorez-le avec l’attribut **IgnoreDataMember** .</span><span class="sxs-lookup"><span data-stu-id="cf9da-176">To omit a property or field, decorate it with the **IgnoreDataMember** attribute.</span></span>
- <span data-ttu-id="cf9da-177">Les membres privés et protégés ne sont pas sérialisés.</span><span class="sxs-lookup"><span data-stu-id="cf9da-177">Private and protected members are not serialized.</span></span>
- <span data-ttu-id="cf9da-178">Les propriétés en lecture seule ne sont pas sérialisées.</span><span class="sxs-lookup"><span data-stu-id="cf9da-178">Read-only properties are not serialized.</span></span> <span data-ttu-id="cf9da-179">(Toutefois, le contenu d’une propriété de collection en lecture seule est sérialisé.)</span><span class="sxs-lookup"><span data-stu-id="cf9da-179">(However, the contents of a read-only collection property are serialized.)</span></span>
- <span data-ttu-id="cf9da-180">Les noms de classes et de membres sont écrits dans le XML exactement tels qu’ils apparaissent dans la déclaration de classe.</span><span class="sxs-lookup"><span data-stu-id="cf9da-180">Class and member names are written in the XML exactly as they appear in the class declaration.</span></span>
- <span data-ttu-id="cf9da-181">Un espace de noms XML par défaut est utilisé.</span><span class="sxs-lookup"><span data-stu-id="cf9da-181">A default XML namespace is used.</span></span>

<span data-ttu-id="cf9da-182">Si vous avez besoin de davantage de contrôle sur la sérialisation, vous pouvez décorer la classe avec l’attribut **DataContract** .</span><span class="sxs-lookup"><span data-stu-id="cf9da-182">If you need more control over the serialization, you can decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="cf9da-183">Lorsque cet attribut est présent, la classe est sérialisée comme suit :</span><span class="sxs-lookup"><span data-stu-id="cf9da-183">When this attribute is present, the class is serialized as follows:</span></span>

- <span data-ttu-id="cf9da-184">&quot;adopter&quot; approche : les propriétés et les champs ne sont pas sérialisés par défaut.</span><span class="sxs-lookup"><span data-stu-id="cf9da-184">&quot;Opt in&quot; approach: Properties and fields are not serialized by default.</span></span> <span data-ttu-id="cf9da-185">Pour sérialiser une propriété ou un champ, décorez-le avec l’attribut **DataMember** .</span><span class="sxs-lookup"><span data-stu-id="cf9da-185">To serialize a property or field, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="cf9da-186">Pour sérialiser un membre privé ou protégé, décorez-le avec l’attribut **DataMember** .</span><span class="sxs-lookup"><span data-stu-id="cf9da-186">To serialize a private or protected member, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="cf9da-187">Les propriétés en lecture seule ne sont pas sérialisées.</span><span class="sxs-lookup"><span data-stu-id="cf9da-187">Read-only properties are not serialized.</span></span>
- <span data-ttu-id="cf9da-188">Pour modifier le mode d’affichage du nom de la classe dans le code XML, définissez le paramètre *Name* dans l’attribut **DataContract** .</span><span class="sxs-lookup"><span data-stu-id="cf9da-188">To change how the class name appears in the XML, set the *Name* parameter in the **DataContract** attribute.</span></span>
- <span data-ttu-id="cf9da-189">Pour modifier le mode d’affichage d’un nom de membre dans le XML, définissez le paramètre *Name* dans l’attribut **DataMember** .</span><span class="sxs-lookup"><span data-stu-id="cf9da-189">To change how a member name appears in the XML, set the *Name* parameter in the **DataMember** attribute.</span></span>
- <span data-ttu-id="cf9da-190">Pour modifier l’espace de noms XML, définissez le paramètre d' *espace de noms* dans la classe **DataContract** .</span><span class="sxs-lookup"><span data-stu-id="cf9da-190">To change the XML namespace, set the *Namespace* parameter in the **DataContract** class.</span></span>

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="cf9da-191">Propriétés en lecture seule</span><span class="sxs-lookup"><span data-stu-id="cf9da-191">Read-Only Properties</span></span>

<span data-ttu-id="cf9da-192">Les propriétés en lecture seule ne sont pas sérialisées.</span><span class="sxs-lookup"><span data-stu-id="cf9da-192">Read-only properties are not serialized.</span></span> <span data-ttu-id="cf9da-193">Si une propriété en lecture seule contient un champ privé de sauvegarde, vous pouvez marquer le champ privé avec l’attribut **DataMember** .</span><span class="sxs-lookup"><span data-stu-id="cf9da-193">If a read-only property has a backing private field, you can mark the private field with the **DataMember** attribute.</span></span> <span data-ttu-id="cf9da-194">Cette approche nécessite l’attribut **DataContract** sur la classe.</span><span class="sxs-lookup"><span data-stu-id="cf9da-194">This approach requires the **DataContract** attribute on the class.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a><span data-ttu-id="cf9da-195">Dates</span><span class="sxs-lookup"><span data-stu-id="cf9da-195">Dates</span></span>

<span data-ttu-id="cf9da-196">Les dates sont écrites au format ISO 8601.</span><span class="sxs-lookup"><span data-stu-id="cf9da-196">Dates are written in ISO 8601 format.</span></span> <span data-ttu-id="cf9da-197">Par exemple, &quot;2012-05-23T20:21:37.9116538 Z&quot;.</span><span class="sxs-lookup"><span data-stu-id="cf9da-197">For example, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span></span>

<a id="xml_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="cf9da-198">Mise en retrait</span><span class="sxs-lookup"><span data-stu-id="cf9da-198">Indenting</span></span>

<span data-ttu-id="cf9da-199">Pour écrire du code XML mis en retrait, affectez la valeur **true**à la propriété **indent** :</span><span class="sxs-lookup"><span data-stu-id="cf9da-199">To write indented XML, set the **Indent** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a><span data-ttu-id="cf9da-200">Définition de sérialiseurs XML par type</span><span class="sxs-lookup"><span data-stu-id="cf9da-200">Setting Per-Type XML Serializers</span></span>

<span data-ttu-id="cf9da-201">Vous pouvez définir différents sérialiseurs XML pour différents types CLR.</span><span class="sxs-lookup"><span data-stu-id="cf9da-201">You can set different XML serializers for different CLR types.</span></span> <span data-ttu-id="cf9da-202">Par exemple, vous pouvez avoir un objet de données particulier qui requiert **XmlSerializer** pour des raisons de compatibilité descendante.</span><span class="sxs-lookup"><span data-stu-id="cf9da-202">For example, you might have a particular data object that requires **XmlSerializer** for backward compatibility.</span></span> <span data-ttu-id="cf9da-203">Vous pouvez utiliser **XmlSerializer** pour cet objet et continuer à utiliser **DataContractSerializer** pour d’autres types.</span><span class="sxs-lookup"><span data-stu-id="cf9da-203">You can use **XmlSerializer** for this object and continue to use **DataContractSerializer** for other types.</span></span>

<span data-ttu-id="cf9da-204">Pour définir un sérialiseur XML pour un type particulier, appelez **SetSerializer**.</span><span class="sxs-lookup"><span data-stu-id="cf9da-204">To set an XML serializer for a particular type, call **SetSerializer**.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

<span data-ttu-id="cf9da-205">Vous pouvez spécifier un **XmlSerializer** ou tout objet qui dérive de **XmlObjectSerializer**.</span><span class="sxs-lookup"><span data-stu-id="cf9da-205">You can specify an **XmlSerializer** or any object that derives from **XmlObjectSerializer**.</span></span>

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a><span data-ttu-id="cf9da-206">Suppression du formateur JSON ou XML</span><span class="sxs-lookup"><span data-stu-id="cf9da-206">Removing the JSON or XML Formatter</span></span>

<span data-ttu-id="cf9da-207">Vous pouvez supprimer le module de formatage JSON ou XML de la liste des formateurs, si vous ne souhaitez pas les utiliser.</span><span class="sxs-lookup"><span data-stu-id="cf9da-207">You can remove the JSON formatter or the XML formatter from the list of formatters, if you do not want to use them.</span></span> <span data-ttu-id="cf9da-208">Les principales raisons sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="cf9da-208">The main reasons to do this are:</span></span>

- <span data-ttu-id="cf9da-209">Pour limiter les réponses de votre API Web à un type de média particulier.</span><span class="sxs-lookup"><span data-stu-id="cf9da-209">To restrict your web API responses to a particular media type.</span></span> <span data-ttu-id="cf9da-210">Par exemple, vous pouvez décider de prendre en charge uniquement les réponses JSON et supprimer le formateur XML.</span><span class="sxs-lookup"><span data-stu-id="cf9da-210">For example, you might decide to support only JSON responses, and remove the XML formatter.</span></span>
- <span data-ttu-id="cf9da-211">Pour remplacer le formateur par défaut par un formateur personnalisé.</span><span class="sxs-lookup"><span data-stu-id="cf9da-211">To replace the default formatter with a custom formatter.</span></span> <span data-ttu-id="cf9da-212">Par exemple, vous pouvez remplacer le formateur JSON par votre propre implémentation personnalisée d’un formateur JSON.</span><span class="sxs-lookup"><span data-stu-id="cf9da-212">For example, you could replace the JSON formatter with your own custom implementation of a JSON formatter.</span></span>

<span data-ttu-id="cf9da-213">Le code suivant montre comment supprimer les formateurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="cf9da-213">The following code shows how to remove the default formatters.</span></span> <span data-ttu-id="cf9da-214">Appelez-le à partir de votre **Application\_méthode Start** , définie dans global. asax.</span><span class="sxs-lookup"><span data-stu-id="cf9da-214">Call this from your **Application\_Start** method, defined in Global.asax.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a><span data-ttu-id="cf9da-215">Gestion des références d’objets circulaires</span><span class="sxs-lookup"><span data-stu-id="cf9da-215">Handling Circular Object References</span></span>

<span data-ttu-id="cf9da-216">Par défaut, les formateurs JSON et XML écrivent tous les objets en tant que valeurs.</span><span class="sxs-lookup"><span data-stu-id="cf9da-216">By default, the JSON and XML formatters write all objects as values.</span></span> <span data-ttu-id="cf9da-217">Si deux propriétés font référence au même objet, ou si le même objet apparaît deux fois dans une collection, le formateur sérialise l’objet deux fois.</span><span class="sxs-lookup"><span data-stu-id="cf9da-217">If two properties refer to the same object, or if the same object appears twice in a collection, the formatter will serialize the object twice.</span></span> <span data-ttu-id="cf9da-218">Il s’agit d’un problème particulier si votre graphique d’objets contient des cycles, car le sérialiseur lèvera une exception lorsqu’il détecte une boucle dans le graphique.</span><span class="sxs-lookup"><span data-stu-id="cf9da-218">This is a particular problem if your object graph contains cycles, because the serializer will throw an exception when it detects a loop in the graph.</span></span>

<span data-ttu-id="cf9da-219">Tenez compte des modèles objet et du contrôleur suivants.</span><span class="sxs-lookup"><span data-stu-id="cf9da-219">Consider the following object models and controller.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

<span data-ttu-id="cf9da-220">L’appel de cette action entraîne la levée d’une exception par le formateur, qui se traduit par une réponse de code d’état 500 (erreur de serveur interne) au client.</span><span class="sxs-lookup"><span data-stu-id="cf9da-220">Invoking this action will cause the formatter to thrown an exception, which translates to a status code 500 (Internal Server Error) response to the client.</span></span>

<span data-ttu-id="cf9da-221">Pour conserver les références d’objet dans JSON, ajoutez le code suivant à l' **Application\_méthode Start** dans le fichier global. asax :</span><span class="sxs-lookup"><span data-stu-id="cf9da-221">To preserve object references in JSON, add the following code to **Application\_Start** method in the Global.asax file:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

<span data-ttu-id="cf9da-222">À présent, l’action du contrôleur renverra un JSON ressemblant à ceci :</span><span class="sxs-lookup"><span data-stu-id="cf9da-222">Now the controller action will return JSON that looks like this:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

<span data-ttu-id="cf9da-223">Notez que le sérialiseur ajoute une &quot;$id&quot; propriété aux deux objets.</span><span class="sxs-lookup"><span data-stu-id="cf9da-223">Notice that the serializer adds an &quot;$id&quot; property to both objects.</span></span> <span data-ttu-id="cf9da-224">En outre, il détecte que la propriété Employee. Department crée une boucle, de sorte qu’elle remplace la valeur par une référence d’objet : {&quot;$ref&quot;:&quot;1&quot;}.</span><span class="sxs-lookup"><span data-stu-id="cf9da-224">Also, it detects that the Employee.Department property creates a loop, so it replaces the value with an object reference: {&quot;$ref&quot;:&quot;1&quot;}.</span></span>

> [!NOTE]
> <span data-ttu-id="cf9da-225">Les références d’objet ne sont pas standard dans JSON.</span><span class="sxs-lookup"><span data-stu-id="cf9da-225">Object references are not standard in JSON.</span></span> <span data-ttu-id="cf9da-226">Avant d’utiliser cette fonctionnalité, déterminez si vos clients peuvent analyser les résultats.</span><span class="sxs-lookup"><span data-stu-id="cf9da-226">Before using this feature, consider whether your clients will be able to parse the results.</span></span> <span data-ttu-id="cf9da-227">Il peut être préférable de supprimer simplement des cycles du graphique.</span><span class="sxs-lookup"><span data-stu-id="cf9da-227">It might be better simply to remove cycles from the graph.</span></span> <span data-ttu-id="cf9da-228">Par exemple, le lien de retour de l’employé au service n’est pas vraiment nécessaire dans cet exemple.</span><span class="sxs-lookup"><span data-stu-id="cf9da-228">For example, the link from Employee back to Department is not really needed in this example.</span></span>

<span data-ttu-id="cf9da-229">Pour conserver les références d’objet dans XML, vous avez deux options.</span><span class="sxs-lookup"><span data-stu-id="cf9da-229">To preserve object references in XML, you have two options.</span></span> <span data-ttu-id="cf9da-230">L’option la plus simple consiste à ajouter `[DataContract(IsReference=true)]` à votre classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="cf9da-230">The simpler option is to add `[DataContract(IsReference=true)]` to your model class.</span></span> <span data-ttu-id="cf9da-231">Le paramètre *IsReference* active les références d’objet.</span><span class="sxs-lookup"><span data-stu-id="cf9da-231">The *IsReference* parameter enables object references.</span></span> <span data-ttu-id="cf9da-232">Rappelez-vous que **DataContract** fait un abonnement de sérialisation. vous devrez donc également ajouter des attributs **DataMember** aux propriétés :</span><span class="sxs-lookup"><span data-stu-id="cf9da-232">Remember that **DataContract** makes serialization opt-in, so you will also need to add **DataMember** attributes to the properties:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

<span data-ttu-id="cf9da-233">À présent, le formateur produira un XML similaire à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="cf9da-233">Now the formatter will produce XML similar to following:</span></span>

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

<span data-ttu-id="cf9da-234">Si vous souhaitez éviter des attributs sur votre classe de modèle, il existe une autre option : créer une instance **DataContractSerializer** spécifique au type et affecter à *preserveObjectReferences* la **valeur true** dans le constructeur.</span><span class="sxs-lookup"><span data-stu-id="cf9da-234">If you want to avoid attributes on your model class, there is another option: Create a new type-specific **DataContractSerializer** instance and set *preserveObjectReferences* to **true** in the constructor.</span></span> <span data-ttu-id="cf9da-235">Ensuite, définissez cette instance en tant que sérialiseur par type sur le formateur de type de média XML.</span><span class="sxs-lookup"><span data-stu-id="cf9da-235">Then set this instance as a per-type serializer on the XML media-type formatter.</span></span> <span data-ttu-id="cf9da-236">Le code suivant montre comment procéder :</span><span class="sxs-lookup"><span data-stu-id="cf9da-236">The following code show how to do this:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a><span data-ttu-id="cf9da-237">Test de la sérialisation d’un objet</span><span class="sxs-lookup"><span data-stu-id="cf9da-237">Testing Object Serialization</span></span>

<span data-ttu-id="cf9da-238">Au fur et à mesure que vous concevez votre API Web, il est utile de tester la manière dont vos objets de données seront sérialisés.</span><span class="sxs-lookup"><span data-stu-id="cf9da-238">As you design your web API, it is useful to test how your data objects will be serialized.</span></span> <span data-ttu-id="cf9da-239">Vous pouvez le faire sans créer de contrôleur ni appeler une action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="cf9da-239">You can do this without creating a controller or invoking a controller action.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
