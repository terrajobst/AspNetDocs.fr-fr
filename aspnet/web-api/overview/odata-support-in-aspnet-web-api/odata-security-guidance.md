---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: Guide de sécurité pour API Web ASP.NET 2 OData-ASP.NET 4. x
author: MikeWasson
description: Décrit les problèmes de sécurité à prendre en compte lors de l’exposition d’un jeu de données par le biais d’OData pour API Web ASP.NET 2 sur ASP.NET 4. x.
ms.author: riande
ms.date: 02/06/2013
ms.custom: seoapril2019
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 8194a368cb0629c30e32ec05bf4bed150d442ad8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556496"
---
# <a name="security-guidance-for-aspnet-web-api-2-odata"></a><span data-ttu-id="1d9d1-103">Guide de sécurité pour API Web ASP.NET 2 OData</span><span class="sxs-lookup"><span data-stu-id="1d9d1-103">Security Guidance for ASP.NET Web API 2 OData</span></span>

<span data-ttu-id="1d9d1-104">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1d9d1-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="1d9d1-105">Cette rubrique décrit certains des problèmes de sécurité que vous devez prendre en compte lors de l’exposition d’un jeu de données par le biais d’OData pour API Web ASP.NET 2 sur ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="1d9d1-105">This topic describes some of the security issues that you should consider when exposing a dataset through OData for ASP.NET Web API 2 on ASP.NET 4.x.</span></span>

## <a name="edm-security"></a><span data-ttu-id="1d9d1-106">Sécurité EDM</span><span class="sxs-lookup"><span data-stu-id="1d9d1-106">EDM Security</span></span>

<span data-ttu-id="1d9d1-107">La sémantique de requête est basée sur le modèle EDM (Entity Data Model), et non sur les types de modèles sous-jacents.</span><span class="sxs-lookup"><span data-stu-id="1d9d1-107">The query semantics are based on the entity data model (EDM), not the underlying model types.</span></span> <span data-ttu-id="1d9d1-108">Vous pouvez exclure une propriété du modèle EDM et celle-ci ne sera pas visible par la requête.</span><span class="sxs-lookup"><span data-stu-id="1d9d1-108">You can exclude a property from the EDM and it will not be visible to the query.</span></span> <span data-ttu-id="1d9d1-109">Par exemple, supposons que votre modèle comprenne un type d’employé avec une propriété de salaire.</span><span class="sxs-lookup"><span data-stu-id="1d9d1-109">For example, suppose your model includes an Employee type with a Salary property.</span></span> <span data-ttu-id="1d9d1-110">Vous souhaiterez peut-être exclure cette propriété de l’EDM pour la masquer sur les clients.</span><span class="sxs-lookup"><span data-stu-id="1d9d1-110">You might want to exclude this property from the EDM to hide it from clients.</span></span>

<span data-ttu-id="1d9d1-111">Il existe deux façons d’exclure une propriété du modèle EDM.</span><span class="sxs-lookup"><span data-stu-id="1d9d1-111">There are two ways to exclude a property from the EDM.</span></span> <span data-ttu-id="1d9d1-112">Vous pouvez définir l’attribut **[IgnoreDataMember]** sur la propriété dans la classe de modèle :</span><span class="sxs-lookup"><span data-stu-id="1d9d1-112">You can set the **[IgnoreDataMember]** attribute on the property in the model class:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

<span data-ttu-id="1d9d1-113">Vous pouvez également supprimer la propriété de l’EDM par programme :</span><span class="sxs-lookup"><span data-stu-id="1d9d1-113">You can also remove the property from the EDM programmatically:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a><span data-ttu-id="1d9d1-114">Sécurité des requêtes</span><span class="sxs-lookup"><span data-stu-id="1d9d1-114">Query Security</span></span>

<span data-ttu-id="1d9d1-115">Un client malveillant ou naïve peut être en mesure de créer une requête dont l’exécution prend beaucoup de temps.</span><span class="sxs-lookup"><span data-stu-id="1d9d1-115">A malicious or naive client may be able to construct a query that takes a very long time to execute.</span></span> <span data-ttu-id="1d9d1-116">Dans le pire des cas, cela peut perturber l’accès à votre service.</span><span class="sxs-lookup"><span data-stu-id="1d9d1-116">In the worst case this can disrupt access to your service.</span></span>

<span data-ttu-id="1d9d1-117">L’attribut **[interrogeable]** est un filtre d’action qui analyse, valide et applique la requête.</span><span class="sxs-lookup"><span data-stu-id="1d9d1-117">The **[Queryable]** attribute is an action filter that parses, validates, and applies the query.</span></span> <span data-ttu-id="1d9d1-118">Le filtre convertit les options de requête en une expression LINQ.</span><span class="sxs-lookup"><span data-stu-id="1d9d1-118">The filter converts the query options into a LINQ expression.</span></span> <span data-ttu-id="1d9d1-119">Lorsque le contrôleur OData retourne un type **IQueryable** , le fournisseur LINQ **IQueryable** convertit l’expression LINQ en une requête.</span><span class="sxs-lookup"><span data-stu-id="1d9d1-119">When the OData controller returns an **IQueryable** type, the **IQueryable** LINQ provider converts the LINQ expression into a query.</span></span> <span data-ttu-id="1d9d1-120">Par conséquent, les performances dépendent du fournisseur LINQ utilisé, ainsi que des caractéristiques particulières de votre schéma de base de données ou de base de données.</span><span class="sxs-lookup"><span data-stu-id="1d9d1-120">Therefore, performance depends on the LINQ provider that is used, and also on the particular characteristics of your dataset or database schema.</span></span>

<span data-ttu-id="1d9d1-121">Pour plus d’informations sur l’utilisation des options de requête OData dans API Web ASP.NET, consultez [prise en charge des options de requête OData](supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="1d9d1-121">For more information about using OData query options in ASP.NET Web API, see [Supporting OData Query Options](supporting-odata-query-options.md).</span></span>

<span data-ttu-id="1d9d1-122">Si vous savez que tous les clients sont approuvés (par exemple, dans un environnement d’entreprise), ou si votre jeu de données est petit, les performances des requêtes peuvent ne pas être un problème.</span><span class="sxs-lookup"><span data-stu-id="1d9d1-122">If you know that all clients are trusted (for example, in an enterprise environment), or if your dataset is small, query performance might not be an issue.</span></span> <span data-ttu-id="1d9d1-123">Dans le cas contraire, vous devez prendre en compte les recommandations suivantes.</span><span class="sxs-lookup"><span data-stu-id="1d9d1-123">Otherwise, you should consider the following recommendations.</span></span>

- <span data-ttu-id="1d9d1-124">Testez votre service avec diverses requêtes et profilez la base de la base de</span><span class="sxs-lookup"><span data-stu-id="1d9d1-124">Test your service with various queries and profile the DB.</span></span>
- <span data-ttu-id="1d9d1-125">Activez la pagination pilotée par le serveur pour éviter de retourner un jeu de données volumineux dans une requête.</span><span class="sxs-lookup"><span data-stu-id="1d9d1-125">Enable server-driven paging, to avoid returning a large data set in one query.</span></span> <span data-ttu-id="1d9d1-126">Pour plus d’informations, consultez [pagination pilotée par le serveur](supporting-odata-query-options.md#server-paging).</span><span class="sxs-lookup"><span data-stu-id="1d9d1-126">For more information, see [Server-Driven Paging](supporting-odata-query-options.md#server-paging).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- <span data-ttu-id="1d9d1-127">Avez-vous besoin d' $filter et de $orderby ?</span><span class="sxs-lookup"><span data-stu-id="1d9d1-127">Do you need $filter and $orderby?</span></span> <span data-ttu-id="1d9d1-128">Certaines applications peuvent autoriser la pagination du client, en utilisant $top et $skip, mais désactiver les autres options de requête.</span><span class="sxs-lookup"><span data-stu-id="1d9d1-128">Some applications might allow client paging, using $top and $skip, but disable the other query options.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- <span data-ttu-id="1d9d1-129">Envisagez de limiter les $orderby aux propriétés dans un index cluster.</span><span class="sxs-lookup"><span data-stu-id="1d9d1-129">Consider restricting $orderby to properties in a clustered index.</span></span> <span data-ttu-id="1d9d1-130">Le tri des données volumineuses sans index cluster est lent.</span><span class="sxs-lookup"><span data-stu-id="1d9d1-130">Sorting large data without a clustered index is slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- <span data-ttu-id="1d9d1-131">Nombre maximal de nœuds : la propriété **maxNodeCount** sur **[Queryable]** définit le nombre maximal de nœuds autorisés dans l’arborescence de syntaxe $Filter.</span><span class="sxs-lookup"><span data-stu-id="1d9d1-131">Maximum node count: The **MaxNodeCount** property on **[Queryable]** sets the maximum number nodes allowed in the $filter syntax tree.</span></span> <span data-ttu-id="1d9d1-132">La valeur par défaut est 100, mais vous pouvez définir une valeur inférieure, car un grand nombre de nœuds peut être lent à compiler.</span><span class="sxs-lookup"><span data-stu-id="1d9d1-132">The default value is 100, but you may want to set a lower value, because a large number of nodes can be slow to compile.</span></span> <span data-ttu-id="1d9d1-133">Cela est particulièrement vrai si vous utilisez des LINQ to Objects (par exemple, des requêtes LINQ sur une collection en mémoire, sans utiliser de fournisseur LINQ intermédiaire).</span><span class="sxs-lookup"><span data-stu-id="1d9d1-133">This is particularly true if you are using LINQ to Objects (i.e., LINQ queries on a collection in memory, without the use of an intermediate LINQ provider).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- <span data-ttu-id="1d9d1-134">Envisagez de désactiver les fonctions any () et All (), car elles peuvent être lentes.</span><span class="sxs-lookup"><span data-stu-id="1d9d1-134">Consider disabling the any() and all() functions, as these can be slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- <span data-ttu-id="1d9d1-135">Si des propriétés de chaîne contiennent des&#8212;chaînes volumineuses, par exemple, une description de&#8212;produit ou une entrée de blog, envisagez de désactiver les fonctions de chaîne.</span><span class="sxs-lookup"><span data-stu-id="1d9d1-135">If any string properties contain large strings&#8212;for example, a product description or a blog entry&#8212;consider disabling the string functions.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- <span data-ttu-id="1d9d1-136">Envisagez de ne pas autoriser le filtrage sur les propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="1d9d1-136">Consider disallowing filtering on navigation properties.</span></span> <span data-ttu-id="1d9d1-137">Le filtrage sur les propriétés de navigation peut entraîner une jointure, qui peut être lente, en fonction de votre schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="1d9d1-137">Filtering on navigation properties can result in a join, which might be slow, depending on your database schema.</span></span> <span data-ttu-id="1d9d1-138">Le code suivant illustre un validateur de requête qui empêche le filtrage sur les propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="1d9d1-138">The following code shows a query validator that prevents filtering on navigation properties.</span></span> <span data-ttu-id="1d9d1-139">Pour plus d’informations sur les validateurs de requête, consultez [validation de requête](supporting-odata-query-options.md#query-validation).</span><span class="sxs-lookup"><span data-stu-id="1d9d1-139">For more information about query validators, see [Query Validation](supporting-odata-query-options.md#query-validation).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- <span data-ttu-id="1d9d1-140">Envisagez de limiter $filter requêtes en écrivant un validateur personnalisé pour votre base de données.</span><span class="sxs-lookup"><span data-stu-id="1d9d1-140">Consider restricting $filter queries by writing a validator that is customized for your database.</span></span> <span data-ttu-id="1d9d1-141">Par exemple, considérez ces deux requêtes :</span><span class="sxs-lookup"><span data-stu-id="1d9d1-141">For example, consider these two queries:</span></span> 

  - <span data-ttu-id="1d9d1-142">Tous les films avec des acteurs dont le nom commence par « A ».</span><span class="sxs-lookup"><span data-stu-id="1d9d1-142">All movies with actors whose last name starts with ‘A'.</span></span>
  - <span data-ttu-id="1d9d1-143">Tous les films publiés dans 1994.</span><span class="sxs-lookup"><span data-stu-id="1d9d1-143">All movies released in 1994.</span></span>

    <span data-ttu-id="1d9d1-144">À moins que les films ne soient indexés par des acteurs, la première requête peut nécessiter que le moteur de base de base de base de l’analyse la liste complète des films.</span><span class="sxs-lookup"><span data-stu-id="1d9d1-144">Unless movies are indexed by actors, the first query might require the DB engine to scan the entire list of movies.</span></span> <span data-ttu-id="1d9d1-145">Tandis que la deuxième requête peut être acceptable, en supposant que les films sont indexés par année de sortie.</span><span class="sxs-lookup"><span data-stu-id="1d9d1-145">Whereas the second query might be acceptable, assuming movies are indexed by release year.</span></span>

    <span data-ttu-id="1d9d1-146">Le code suivant illustre un validateur qui autorise le filtrage sur les propriétés « ReleaseYear » et « title », mais pas sur d’autres propriétés.</span><span class="sxs-lookup"><span data-stu-id="1d9d1-146">The following code shows a validator that allows filtering on the "ReleaseYear" and "Title" properties but no other properties.</span></span>

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- <span data-ttu-id="1d9d1-147">En général, réfléchissez aux fonctions de $filter dont vous avez besoin.</span><span class="sxs-lookup"><span data-stu-id="1d9d1-147">In general, consider which $filter functions you need.</span></span> <span data-ttu-id="1d9d1-148">Si vos clients n’ont pas besoin de l’expressivité complète de $filter, vous pouvez limiter les fonctions autorisées.</span><span class="sxs-lookup"><span data-stu-id="1d9d1-148">If your clients do not need the full expressiveness of $filter, you can limit the allowed functions.</span></span>
