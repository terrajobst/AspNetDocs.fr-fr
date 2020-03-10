---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: Héritage de type complexe dans OData v4 avec API Web ASP.NET | Microsoft Docs
author: microsoft
description: Selon la spécification OData v4, un type complexe peut hériter d’un autre type complexe. (Un type complexe est un type structuré sans clé.) API Web...
ms.author: riande
ms.date: 09/16/2014
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 3d90216c8e594055f77577eb6d8b1d978ae4c24d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556307"
---
# <a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="6f979-104">Héritage de type complexe dans OData v4 avec API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6f979-104">Complex Type Inheritance in OData v4 with ASP.NET Web API</span></span>

<span data-ttu-id="6f979-105">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="6f979-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="6f979-106">Selon la [spécification](http://www.odata.org/documentation/odata-version-4-0/)OData v4, un type complexe peut hériter d’un autre type complexe.</span><span class="sxs-lookup"><span data-stu-id="6f979-106">According to the OData v4 [specification](http://www.odata.org/documentation/odata-version-4-0/), a complex type can inherit from another complex type.</span></span> <span data-ttu-id="6f979-107">(Un type *complexe* est un type structuré sans clé.) L’API Web OData 5,3 prend en charge l’héritage de type complexe.</span><span class="sxs-lookup"><span data-stu-id="6f979-107">(A *complex* type is a structured type without a key.) Web API OData 5.3 supports complex type inheritance.</span></span>
> 
> <span data-ttu-id="6f979-108">Cette rubrique montre comment générer un modèle EDM (Entity Data Model) avec des types d’héritage complexes.</span><span class="sxs-lookup"><span data-stu-id="6f979-108">This topic shows how to build an entity data model (EDM) with complex inheritance types.</span></span> <span data-ttu-id="6f979-109">Pour obtenir le code source complet, consultez [exemple d’héritage de type complexe OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="6f979-109">For the complete source code, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6f979-110">Versions logicielles utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="6f979-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="6f979-111">API Web OData 5,3</span><span class="sxs-lookup"><span data-stu-id="6f979-111">Web API OData 5.3</span></span>
> - <span data-ttu-id="6f979-112">OData v4</span><span class="sxs-lookup"><span data-stu-id="6f979-112">OData v4</span></span>

## <a name="model-hierarchy"></a><span data-ttu-id="6f979-113">Hiérarchie du modèle</span><span class="sxs-lookup"><span data-stu-id="6f979-113">Model Hierarchy</span></span>

<span data-ttu-id="6f979-114">Pour illustrer l’héritage de type complexe, nous allons utiliser la hiérarchie de classes suivante.</span><span class="sxs-lookup"><span data-stu-id="6f979-114">To illustrate complex type inheritance, we'll use the following class hierarchy.</span></span>

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

<span data-ttu-id="6f979-115">`Shape` est un type complexe abstrait.</span><span class="sxs-lookup"><span data-stu-id="6f979-115">`Shape` is an abstract complex type.</span></span> <span data-ttu-id="6f979-116">`Rectangle`, `Triangle`et `Circle` sont des types complexes dérivés de `Shape`, et `RoundRectangle` dérive de `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="6f979-116">`Rectangle`, `Triangle`, and `Circle` are complex types derived from `Shape`, and `RoundRectangle` derives from `Rectangle`.</span></span> <span data-ttu-id="6f979-117">`Window` est un type d’entité et contient une instance `Shape`.</span><span class="sxs-lookup"><span data-stu-id="6f979-117">`Window` is an entity type and contains a `Shape` instance.</span></span>

<span data-ttu-id="6f979-118">Voici les classes CLR qui définissent ces types.</span><span class="sxs-lookup"><span data-stu-id="6f979-118">Here are the CLR classes that define these types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a><span data-ttu-id="6f979-119">Générer le modèle EDM</span><span class="sxs-lookup"><span data-stu-id="6f979-119">Build the EDM Model</span></span>

<span data-ttu-id="6f979-120">Pour créer le modèle EDM, vous pouvez utiliser **ODataConventionModelBuilder**, qui déduit les relations d’héritage des types CLR.</span><span class="sxs-lookup"><span data-stu-id="6f979-120">To create the EDM, you can use **ODataConventionModelBuilder**, which infers the inheritance relationships from the CLR types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="6f979-121">Vous pouvez également générer explicitement le modèle EDM à l’aide de **ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="6f979-121">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span> <span data-ttu-id="6f979-122">Cela nécessite davantage de code, mais vous donne davantage de contrôle sur le modèle EDM.</span><span class="sxs-lookup"><span data-stu-id="6f979-122">This takes more code, but gives you more control over the EDM.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="6f979-123">Ces deux exemples créent le même schéma EDM.</span><span class="sxs-lookup"><span data-stu-id="6f979-123">These two examples create the same EDM schema.</span></span>

## <a name="metadata-document"></a><span data-ttu-id="6f979-124">Document de métadonnées</span><span class="sxs-lookup"><span data-stu-id="6f979-124">Metadata Document</span></span>

<span data-ttu-id="6f979-125">Voici le document de métadonnées OData, qui montre l’héritage de type complexe.</span><span class="sxs-lookup"><span data-stu-id="6f979-125">Here is the OData metadata document, showing complex type inheritance.</span></span>

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

<span data-ttu-id="6f979-126">À partir du document de métadonnées, vous pouvez voir que :</span><span class="sxs-lookup"><span data-stu-id="6f979-126">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="6f979-127">Le type complexe `Shape` est abstract.</span><span class="sxs-lookup"><span data-stu-id="6f979-127">The `Shape` complex type is abstract.</span></span>
- <span data-ttu-id="6f979-128">Les `Rectangle`, `Triangle`et `Circle` type complexe ont le type de base `Shape`.</span><span class="sxs-lookup"><span data-stu-id="6f979-128">The `Rectangle`, `Triangle`, and `Circle` complex type have the base type `Shape`.</span></span>
- <span data-ttu-id="6f979-129">Le type de `RoundRectangle` a le type de base `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="6f979-129">The `RoundRectangle` type has the base type `Rectangle`.</span></span>

## <a name="casting-complex-types"></a><span data-ttu-id="6f979-130">Cast de types complexes</span><span class="sxs-lookup"><span data-stu-id="6f979-130">Casting Complex Types</span></span>

<span data-ttu-id="6f979-131">La conversion de types complexes est désormais prise en charge.</span><span class="sxs-lookup"><span data-stu-id="6f979-131">Casting on complex types is now supported.</span></span> <span data-ttu-id="6f979-132">Par exemple, la requête suivante effectue un cast d’un `Shape` en `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="6f979-132">For example, the following query casts a `Shape` to a `Rectangle`.</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

<span data-ttu-id="6f979-133">Voici la charge utile de la réponse :</span><span class="sxs-lookup"><span data-stu-id="6f979-133">Here's the response payload:</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
