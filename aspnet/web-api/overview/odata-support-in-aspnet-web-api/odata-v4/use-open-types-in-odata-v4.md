---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: Types ouverts dans OData v4 avec API Web ASP.NET | Microsoft Docs
author: microsoft
description: Dans OData v4, un type ouvert est un type structuré qui contient des propriétés dynamiques, en plus de toutes les propriétés déclarées dans la définition de type. Ouvrir…
ms.author: riande
ms.date: 09/15/2014
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 950442c071bf50d2c8c1588971f13f85c4891436
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622177"
---
# <a name="open-types-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="cdf2b-104">Types ouverts dans OData v4 avec API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cdf2b-104">Open Types in OData v4 with ASP.NET Web API</span></span>

<span data-ttu-id="cdf2b-105">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="cdf2b-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="cdf2b-106">Dans OData v4, un *type ouvert* est un type structuré qui contient des propriétés dynamiques, en plus de toutes les propriétés déclarées dans la définition de type.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-106">In OData v4, an *open type* is a structured type that contains dynamic properties, in addition to any properties that are declared in the type definition.</span></span> <span data-ttu-id="cdf2b-107">Les types ouverts vous permettent d’ajouter de la flexibilité à vos modèles de données.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-107">Open types let you add flexibility to your data models.</span></span> <span data-ttu-id="cdf2b-108">Ce didacticiel montre comment utiliser des types ouverts dans API Web ASP.NET OData.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-108">This tutorial shows how to use open types in ASP.NET Web API OData.</span></span>
> 
> <span data-ttu-id="cdf2b-109">Ce didacticiel part du principe que vous savez déjà comment créer un point de terminaison OData dans API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-109">This tutorial assumes that you already know how to create an OData endpoint in ASP.NET Web API.</span></span> <span data-ttu-id="cdf2b-110">Si ce n’est pas le cas, commencez par lire [créer un point de terminaison OData v4](create-an-odata-v4-endpoint.md) en premier.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-110">If not, start by reading [Create an OData v4 Endpoint](create-an-odata-v4-endpoint.md) first.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="cdf2b-111">Versions logicielles utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="cdf2b-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="cdf2b-112">API Web OData 5,3</span><span class="sxs-lookup"><span data-stu-id="cdf2b-112">Web API OData 5.3</span></span>
> - <span data-ttu-id="cdf2b-113">OData v4</span><span class="sxs-lookup"><span data-stu-id="cdf2b-113">OData v4</span></span>

<span data-ttu-id="cdf2b-114">Tout d’abord, la terminologie OData :</span><span class="sxs-lookup"><span data-stu-id="cdf2b-114">First, some OData terminology:</span></span>

- <span data-ttu-id="cdf2b-115">Type d’entité : type structuré avec une clé.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-115">Entity type: A structured type with a key.</span></span>
- <span data-ttu-id="cdf2b-116">Type complexe : type structuré sans clé.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-116">Complex type: A structured type without a key.</span></span>
- <span data-ttu-id="cdf2b-117">Open type : type avec des propriétés dynamiques.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-117">Open type: A type with dynamic properties.</span></span> <span data-ttu-id="cdf2b-118">Les types d’entité et les types complexes peuvent être ouverts.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-118">Both entity types and complex types can be open.</span></span>

<span data-ttu-id="cdf2b-119">La valeur d’une propriété dynamique peut être un type primitif, un type complexe ou un type énumération ; ou une collection de l’un de ces types.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-119">The value of a dynamic property can be a primitive type, complex type, or enumeration type; or a collection of any of those types.</span></span> <span data-ttu-id="cdf2b-120">Pour plus d’informations sur les types ouverts, consultez la [spécification OData v4](http://www.odata.org/documentation/odata-version-4-0/).</span><span class="sxs-lookup"><span data-stu-id="cdf2b-120">For more information about open types, see the [OData v4 specification](http://www.odata.org/documentation/odata-version-4-0/).</span></span>

## <a name="install-the-web-odata-libraries"></a><span data-ttu-id="cdf2b-121">Installer les bibliothèques Web OData</span><span class="sxs-lookup"><span data-stu-id="cdf2b-121">Install the Web OData Libraries</span></span>

<span data-ttu-id="cdf2b-122">Utilisez le gestionnaire de package NuGet pour installer les dernières bibliothèques OData de l’API Web.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-122">Use NuGet Package Manager to install the latest Web API OData libraries.</span></span> <span data-ttu-id="cdf2b-123">À partir de la fenêtre de la console du gestionnaire de package :</span><span class="sxs-lookup"><span data-stu-id="cdf2b-123">From the Package Manager Console window:</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a><span data-ttu-id="cdf2b-124">Définir les types CLR</span><span class="sxs-lookup"><span data-stu-id="cdf2b-124">Define the CLR Types</span></span>

<span data-ttu-id="cdf2b-125">Commencez par définir les modèles EDM en tant que types CLR.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-125">Start by defining the EDM models as CLR types.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="cdf2b-126">Lorsque le Entity Data Model (EDM) est créé,</span><span class="sxs-lookup"><span data-stu-id="cdf2b-126">When the Entity Data Model (EDM) is created,</span></span>

- <span data-ttu-id="cdf2b-127">`Category` est un type d’énumération.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-127">`Category` is an enumeration type.</span></span>
- <span data-ttu-id="cdf2b-128">`Address` est un type complexe.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-128">`Address` is a complex type.</span></span> <span data-ttu-id="cdf2b-129">(Elle n’a pas de clé, donc il ne s’agit pas d’un type d’entité.)</span><span class="sxs-lookup"><span data-stu-id="cdf2b-129">(It does not have a key, so it is not an entity type.)</span></span>
- <span data-ttu-id="cdf2b-130">`Customer` est un type d’entité.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-130">`Customer` is an entity type.</span></span> <span data-ttu-id="cdf2b-131">(Elle a une clé.)</span><span class="sxs-lookup"><span data-stu-id="cdf2b-131">(It has a key.)</span></span>
- <span data-ttu-id="cdf2b-132">`Press` est un type complexe ouvert.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-132">`Press` is an open complex type.</span></span>
- <span data-ttu-id="cdf2b-133">`Book` est un type d’entité ouvert.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-133">`Book` is an open entity type.</span></span>

<span data-ttu-id="cdf2b-134">Pour créer un type ouvert, le type CLR doit avoir une propriété de type `IDictionary<string, object>`, qui contient les propriétés dynamiques.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-134">To create an open type, the CLR type must have a property of type `IDictionary<string, object>`, which holds the dynamic properties.</span></span>

## <a name="build-the-edm-model"></a><span data-ttu-id="cdf2b-135">Générer le modèle EDM</span><span class="sxs-lookup"><span data-stu-id="cdf2b-135">Build the EDM Model</span></span>

<span data-ttu-id="cdf2b-136">Si vous utilisez **ODataConventionModelBuilder** pour créer le modèle EDM, `Press` et `Book` sont automatiquement ajoutés en tant que types ouverts, en fonction de la présence d’une propriété `IDictionary<string, object>`.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-136">If you use **ODataConventionModelBuilder** to create the EDM, `Press` and `Book` are automatically added as open types, based on the presence of a `IDictionary<string, object>` property.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="cdf2b-137">Vous pouvez également générer explicitement le modèle EDM à l’aide de **ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-137">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a><span data-ttu-id="cdf2b-138">Ajouter un contrôleur OData</span><span class="sxs-lookup"><span data-stu-id="cdf2b-138">Add an OData Controller</span></span>

<span data-ttu-id="cdf2b-139">Ensuite, ajoutez un contrôleur OData.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-139">Next, add an OData controller.</span></span> <span data-ttu-id="cdf2b-140">Pour ce didacticiel, nous allons utiliser un contrôleur simplifié qui prend uniquement en charge les demandes d’extraction et de publication, et utilise une liste en mémoire pour stocker les entités.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-140">For this tutorial, we'll use a simplified controller that just supports GET and POST requests, and uses an in-memory list to store entities.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

<span data-ttu-id="cdf2b-141">Notez que la première `Book` instance n’a pas de propriétés dynamiques.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-141">Notice that the first `Book` instance has no dynamic properties.</span></span> <span data-ttu-id="cdf2b-142">La deuxième instance de `Book` a les propriétés dynamiques suivantes :</span><span class="sxs-lookup"><span data-stu-id="cdf2b-142">The second `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="cdf2b-143">« Publié » : type primitif</span><span class="sxs-lookup"><span data-stu-id="cdf2b-143">"Published": Primitive type</span></span>
- <span data-ttu-id="cdf2b-144">"Authors" : collection de types primitifs</span><span class="sxs-lookup"><span data-stu-id="cdf2b-144">"Authors": Collection of primitive types</span></span>
- <span data-ttu-id="cdf2b-145">"OtherCategories" : collection de types énumération.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-145">"OtherCategories": Collection of enumeration types.</span></span>

<span data-ttu-id="cdf2b-146">En outre, la propriété `Press` de cette `Book` instance a les propriétés dynamiques suivantes :</span><span class="sxs-lookup"><span data-stu-id="cdf2b-146">Also, the `Press` property of that `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="cdf2b-147">« Blog » : type primitif</span><span class="sxs-lookup"><span data-stu-id="cdf2b-147">"Blog": Primitive type</span></span>
- <span data-ttu-id="cdf2b-148">« Address » : type complexe</span><span class="sxs-lookup"><span data-stu-id="cdf2b-148">"Address": Complex type</span></span>

## <a name="query-the-metadata"></a><span data-ttu-id="cdf2b-149">Interroger les métadonnées</span><span class="sxs-lookup"><span data-stu-id="cdf2b-149">Query the Metadata</span></span>

<span data-ttu-id="cdf2b-150">Pour obtenir le document de métadonnées OData, envoyez une demande de récupération à `~/$metadata`.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-150">To get the OData metadata document, send a GET request to `~/$metadata`.</span></span> <span data-ttu-id="cdf2b-151">Le corps de la réponse doit ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="cdf2b-151">The response body should look similar to this:</span></span>

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

<span data-ttu-id="cdf2b-152">À partir du document de métadonnées, vous pouvez voir que :</span><span class="sxs-lookup"><span data-stu-id="cdf2b-152">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="cdf2b-153">Pour les types `Book` et `Press`, la valeur de l’attribut `OpenType` est true.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-153">For the `Book` and `Press` types, the value of the `OpenType` attribute is true.</span></span> <span data-ttu-id="cdf2b-154">Les types `Customer` et `Address` n’ont pas cet attribut.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-154">The `Customer` and `Address` types don't have this attribute.</span></span>
- <span data-ttu-id="cdf2b-155">Le type d’entité `Book` possède trois propriétés déclarées : ISBN, title et Press.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-155">The `Book` entity type has three declared properties: ISBN, Title, and Press.</span></span> <span data-ttu-id="cdf2b-156">Les métadonnées OData n’incluent pas la propriété `Book.Properties` de la classe CLR.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-156">The OData metadata does not include the `Book.Properties` property from the CLR class.</span></span>
- <span data-ttu-id="cdf2b-157">De même, le `Press` type complexe n’a que deux propriétés déclarées : Name et Category.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-157">Similarly, the `Press` complex type has only two declared properties: Name and Category.</span></span> <span data-ttu-id="cdf2b-158">Les métadonnées n’incluent pas la propriété `Press.DynamicProperties` de la classe CLR.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-158">The metadata does not include the `Press.DynamicProperties` property from the CLR class.</span></span>

## <a name="query-an-entity"></a><span data-ttu-id="cdf2b-159">Interroger une entité</span><span class="sxs-lookup"><span data-stu-id="cdf2b-159">Query an Entity</span></span>

<span data-ttu-id="cdf2b-160">Pour obtenir le livre avec ISBN égal à « 978-0-7356-7942-9 », envoyez une demande de récupération à `~/Books('978-0-7356-7942-9')`.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-160">To get the book with ISBN equal to "978-0-7356-7942-9", send a GET request to `~/Books('978-0-7356-7942-9')`.</span></span> <span data-ttu-id="cdf2b-161">Le corps de la réponse doit ressembler à ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-161">The response body should look similar to the following.</span></span> <span data-ttu-id="cdf2b-162">(En retrait pour le rendre plus lisible.)</span><span class="sxs-lookup"><span data-stu-id="cdf2b-162">(Indented to make it more readable.)</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

<span data-ttu-id="cdf2b-163">Notez que les propriétés dynamiques sont incluses inline avec les propriétés déclarées.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-163">Notice that the dynamic properties are included inline with the declared properties.</span></span>

## <a name="post-an-entity"></a><span data-ttu-id="cdf2b-164">PUBLICATION d’une entité</span><span class="sxs-lookup"><span data-stu-id="cdf2b-164">POST an Entity</span></span>

<span data-ttu-id="cdf2b-165">Pour ajouter une entité de livre, envoyez une demande de publication à `~/Books`.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-165">To add a Book entity, send a POST request to `~/Books`.</span></span> <span data-ttu-id="cdf2b-166">Le client peut définir des propriétés dynamiques dans la charge utile de la demande.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-166">The client can set dynamic properties in the request payload.</span></span>

<span data-ttu-id="cdf2b-167">Voici un exemple de demande.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-167">Here is an example request.</span></span> <span data-ttu-id="cdf2b-168">Notez les propriétés « Price » et « published ».</span><span class="sxs-lookup"><span data-stu-id="cdf2b-168">Note the "Price" and "Published" properties.</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

<span data-ttu-id="cdf2b-169">Si vous définissez un point d’arrêt dans la méthode du contrôleur, vous pouvez voir que l’API Web a ajouté ces propriétés au dictionnaire `Properties`.</span><span class="sxs-lookup"><span data-stu-id="cdf2b-169">If you set a breakpoint in the controller method, you can see that Web API added these properties to the `Properties` dictionary.</span></span>

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a><span data-ttu-id="cdf2b-170">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="cdf2b-170">Additional Resources</span></span>

[<span data-ttu-id="cdf2b-171">Exemple d’ouverture de type OData</span><span class="sxs-lookup"><span data-stu-id="cdf2b-171">OData Open Type Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
