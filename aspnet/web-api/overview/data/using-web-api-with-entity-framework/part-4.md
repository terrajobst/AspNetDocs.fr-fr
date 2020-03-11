---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: Gestion des relations d’entité | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: be4948e5443a5eb4e1824c63dd0c445a7ee1928e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557462"
---
# <a name="handling-entity-relations"></a><span data-ttu-id="87783-102">Gestion des relations d’entité</span><span class="sxs-lookup"><span data-stu-id="87783-102">Handling Entity Relations</span></span>

<span data-ttu-id="87783-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="87783-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="87783-104">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="87783-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="87783-105">Cette section décrit en détail comment EF charge les entités associées et comment gérer les propriétés de navigation circulaires dans vos classes de modèle.</span><span class="sxs-lookup"><span data-stu-id="87783-105">This section describes some details of how EF loads related entities, and how to handle circular navigation properties in your model classes.</span></span> <span data-ttu-id="87783-106">(Cette section fournit des informations générales et n’est pas nécessaire pour suivre le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="87783-106">(This section provides background knowledge, and is not required to complete the tutorial.</span></span> <span data-ttu-id="87783-107">Si vous préférez, passez à la [partie 5.](part-5.md).)</span><span class="sxs-lookup"><span data-stu-id="87783-107">If you prefer, skip to [Part 5.](part-5.md).)</span></span>

## <a name="eager-loading-versus-lazy-loading"></a><span data-ttu-id="87783-108">Chargement hâtif et chargement différé</span><span class="sxs-lookup"><span data-stu-id="87783-108">Eager Loading versus Lazy Loading</span></span>

<span data-ttu-id="87783-109">Lorsque EF est utilisé avec une base de données relationnelle, il est important de comprendre comment EF charge les données associées.</span><span class="sxs-lookup"><span data-stu-id="87783-109">When using EF with a relational database, it's important to understand how EF loads related data.</span></span>

<span data-ttu-id="87783-110">Il est également utile de voir les requêtes SQL générées par EF.</span><span class="sxs-lookup"><span data-stu-id="87783-110">It's also useful to see the SQL queries that EF generates.</span></span> <span data-ttu-id="87783-111">Pour tracer le SQL, ajoutez la ligne de code suivante au constructeur `BookServiceContext` :</span><span class="sxs-lookup"><span data-stu-id="87783-111">To trace the SQL, add the following line of code to the `BookServiceContext` constructor:</span></span>

[!code-csharp[Main](part-4/samples/sample1.cs)]

<span data-ttu-id="87783-112">Si vous envoyez une requête d’extraction à/API/Books, elle retourne JSON comme suit :</span><span class="sxs-lookup"><span data-stu-id="87783-112">If you send a GET request to /api/books, it returns JSON like the following:</span></span>

[!code-console[Main](part-4/samples/sample2.cmd)]

<span data-ttu-id="87783-113">Vous pouvez voir que la propriété auteur a la valeur null, même si le livre contient un type d’auteur valide.</span><span class="sxs-lookup"><span data-stu-id="87783-113">You can see that the Author property is null, even though the book contains a valid AuthorId.</span></span> <span data-ttu-id="87783-114">Cela est dû au fait que EF ne charge pas les entités d’auteur associées.</span><span class="sxs-lookup"><span data-stu-id="87783-114">That's because EF is not loading the related Author entities.</span></span> <span data-ttu-id="87783-115">Le journal des traces de la requête SQL confirme ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="87783-115">The trace log of the SQL query confirms this:</span></span>

[!code-console[Main](part-4/samples/sample3.sql)]

<span data-ttu-id="87783-116">L’instruction SELECT prend la table Books et ne fait pas référence à la table author.</span><span class="sxs-lookup"><span data-stu-id="87783-116">The SELECT statement takes from the Books table, and does not reference the Author table.</span></span>

<span data-ttu-id="87783-117">Pour référence, voici la méthode dans la classe `BooksController` qui retourne la liste des livres.</span><span class="sxs-lookup"><span data-stu-id="87783-117">For reference, here is the method in the `BooksController` class that returns the list of books.</span></span>

[!code-csharp[Main](part-4/samples/sample4.cs)]

<span data-ttu-id="87783-118">Voyons comment nous pouvons retourner l’auteur dans le cadre des données JSON.</span><span class="sxs-lookup"><span data-stu-id="87783-118">Let's see how we can return the Author as part of the JSON data.</span></span> <span data-ttu-id="87783-119">Il existe trois façons de charger des données associées dans Entity Framework : le chargement hâtif, le chargement différé et le chargement explicite.</span><span class="sxs-lookup"><span data-stu-id="87783-119">There are three ways to load related data in Entity Framework: eager loading, lazy loading, and explicit loading.</span></span> <span data-ttu-id="87783-120">Il existe des compromis avec chaque technique. il est donc important de comprendre comment elles fonctionnent.</span><span class="sxs-lookup"><span data-stu-id="87783-120">There are trade-offs with each technique, so it's important to understand how they work.</span></span>

### <a name="eager-loading"></a><span data-ttu-id="87783-121">Chargement hâtif</span><span class="sxs-lookup"><span data-stu-id="87783-121">Eager Loading</span></span>

<span data-ttu-id="87783-122">Avec le *chargement hâtif*, EF charge les entités associées dans le cadre de la requête de base de données initiale.</span><span class="sxs-lookup"><span data-stu-id="87783-122">With *eager loading*, EF loads related entities as part of the initial database query.</span></span> <span data-ttu-id="87783-123">Pour effectuer un chargement hâtif, utilisez la méthode d’extension **System. Data. Entity. include** .</span><span class="sxs-lookup"><span data-stu-id="87783-123">To perform eager loading, use the **System.Data.Entity.Include** extension method.</span></span>

[!code-csharp[Main](part-4/samples/sample5.cs)]

<span data-ttu-id="87783-124">Cela indique à EF d’inclure les données de l’auteur dans la requête.</span><span class="sxs-lookup"><span data-stu-id="87783-124">This tells EF to include the Author data in the query.</span></span> <span data-ttu-id="87783-125">Si vous apportez cette modification et exécutez l’application, les données JSON se présenteront comme suit :</span><span class="sxs-lookup"><span data-stu-id="87783-125">If you make this change and run the app, now the JSON data looks like this:</span></span>

[!code-console[Main](part-4/samples/sample6.cmd)]

<span data-ttu-id="87783-126">Le journal des traces indique que EF a effectué une jointure sur le livre et les tables de création.</span><span class="sxs-lookup"><span data-stu-id="87783-126">The trace log shows that EF performed a join on the Book and Author tables.</span></span>

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a><span data-ttu-id="87783-127">Chargement différé</span><span class="sxs-lookup"><span data-stu-id="87783-127">Lazy Loading</span></span>

<span data-ttu-id="87783-128">Avec le chargement différé, EF charge automatiquement une entité associée lorsque la propriété de navigation de cette entité est déréférencée.</span><span class="sxs-lookup"><span data-stu-id="87783-128">With lazy loading, EF automatically loads a related entity when the navigation property for that entity is dereferenced.</span></span> <span data-ttu-id="87783-129">Pour activer le chargement différé, rendez la propriété de navigation virtuelle.</span><span class="sxs-lookup"><span data-stu-id="87783-129">To enable lazy loading, make the navigation property virtual.</span></span> <span data-ttu-id="87783-130">Par exemple, dans la classe Book :</span><span class="sxs-lookup"><span data-stu-id="87783-130">For example, in the Book class:</span></span>

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

<span data-ttu-id="87783-131">Examinons maintenant le code suivant :</span><span class="sxs-lookup"><span data-stu-id="87783-131">Now consider the following code:</span></span>

[!code-csharp[Main](part-4/samples/sample9.cs)]

<span data-ttu-id="87783-132">Lorsque le chargement différé est activé, l’accès à la propriété `Author` sur `books[0]` force EF à interroger la base de données pour l’auteur.</span><span class="sxs-lookup"><span data-stu-id="87783-132">When lazy loading is enabled, accessing the `Author` property on `books[0]` causes EF to query the database for the author.</span></span>

<span data-ttu-id="87783-133">Le chargement différé nécessite plusieurs trajets de base de données, car EF envoie une requête chaque fois qu’il récupère une entité associée.</span><span class="sxs-lookup"><span data-stu-id="87783-133">Lazy loading requires multiple database trips, because EF sends a query each time it retrieves a related entity.</span></span> <span data-ttu-id="87783-134">En règle générale, vous souhaitez que le chargement différé soit désactivé pour les objets que vous sérialisez.</span><span class="sxs-lookup"><span data-stu-id="87783-134">Generally, you want lazy loading disabled for objects that you serialize.</span></span> <span data-ttu-id="87783-135">Le sérialiseur doit lire toutes les propriétés du modèle, ce qui déclenche le chargement des entités associées.</span><span class="sxs-lookup"><span data-stu-id="87783-135">The serializer has to read all of the properties on the model, which triggers loading the related entities.</span></span> <span data-ttu-id="87783-136">Par exemple, Voici les requêtes SQL lorsque EF sérialise la liste des livres pour lesquels le chargement différé est activé.</span><span class="sxs-lookup"><span data-stu-id="87783-136">For example, here are the SQL queries when EF serializes the list of books with lazy loading enabled.</span></span> <span data-ttu-id="87783-137">Vous pouvez voir que EF effectue trois requêtes distinctes pour les trois auteurs.</span><span class="sxs-lookup"><span data-stu-id="87783-137">You can see that EF makes three separate queries for the three authors.</span></span>

[!code-console[Main](part-4/samples/sample10.sql)]

<span data-ttu-id="87783-138">Il se peut encore que vous souhaitiez utiliser le chargement différé.</span><span class="sxs-lookup"><span data-stu-id="87783-138">There are still times when you might want to use lazy loading.</span></span> <span data-ttu-id="87783-139">Le chargement hâtif peut provoquer la génération d’une jointure très complexe par EF.</span><span class="sxs-lookup"><span data-stu-id="87783-139">Eager loading can cause EF to generate a very complex join.</span></span> <span data-ttu-id="87783-140">Vous pouvez également avoir besoin d’entités associées pour un petit sous-ensemble des données, et le chargement différé serait plus efficace.</span><span class="sxs-lookup"><span data-stu-id="87783-140">Or you might need related entities for a small subset of the data, and lazy loading would be more efficient.</span></span>

<span data-ttu-id="87783-141">Pour éviter les problèmes de sérialisation, il est possible de sérialiser des objets DTO (Data Transfer Objects) au lieu d’objets d’entité.</span><span class="sxs-lookup"><span data-stu-id="87783-141">One way to avoid serialization problems is to serialize data transfer objects (DTOs) instead of entity objects.</span></span> <span data-ttu-id="87783-142">Je vais vous montrer cette approche plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="87783-142">I'll show this approach later in the article.</span></span>

### <a name="explicit-loading"></a><span data-ttu-id="87783-143">Chargement explicite</span><span class="sxs-lookup"><span data-stu-id="87783-143">Explicit Loading</span></span>

<span data-ttu-id="87783-144">Le chargement explicite est semblable au chargement différé, à ceci près que vous récupérez explicitement les données associées dans le code ; elle ne se produit pas automatiquement lorsque vous accédez à une propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="87783-144">Explicit loading is similar to lazy loading, except that you explicitly get the related data in code; it doesn't happen automatically when you access a navigation property.</span></span> <span data-ttu-id="87783-145">Le chargement explicite vous permet de mieux contrôler le moment du chargement des données associées, mais nécessite du code supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="87783-145">Explicit loading gives you more control over when to load related data, but requires extra code.</span></span> <span data-ttu-id="87783-146">Pour plus d’informations sur le chargement explicite, consultez [chargement des entités associées](https://msdn.microsoft.com/data/jj574232#explicit).</span><span class="sxs-lookup"><span data-stu-id="87783-146">For more information about explicit loading, see [Loading Related Entities](https://msdn.microsoft.com/data/jj574232#explicit).</span></span>

## <a name="navigation-properties-and-circular-references"></a><span data-ttu-id="87783-147">Propriétés de navigation et références circulaires</span><span class="sxs-lookup"><span data-stu-id="87783-147">Navigation Properties and Circular References</span></span>

<span data-ttu-id="87783-148">Lorsque j’ai défini les modèles Book et Author, j’ai défini une propriété de navigation sur la classe `Book` pour la relation livre-Author, mais je n’ai pas défini de propriété de navigation dans l’autre direction.</span><span class="sxs-lookup"><span data-stu-id="87783-148">When I defined the Book and Author models, I defined a navigation property on the `Book` class for the Book-Author relationship, but I did not define a navigation property in the other direction.</span></span>

<span data-ttu-id="87783-149">Que se passe-t-il si vous ajoutez la propriété de navigation correspondante à la classe `Author` ?</span><span class="sxs-lookup"><span data-stu-id="87783-149">What happens if you add the corresponding navigation property to the `Author` class?</span></span>

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

<span data-ttu-id="87783-150">Malheureusement, cela crée un problème quand vous sérialisez les modèles.</span><span class="sxs-lookup"><span data-stu-id="87783-150">Unfortunately, this creates a problem when you serialize the models.</span></span> <span data-ttu-id="87783-151">Si vous chargez les données associées, elle crée un graphique d’objet circulaire.</span><span class="sxs-lookup"><span data-stu-id="87783-151">If you load the related data, it creates a circular object graph.</span></span>

![](part-4/_static/image1.png)

<span data-ttu-id="87783-152">Lorsque le formateur JSON ou XML tente de sérialiser le graphique, il lève une exception.</span><span class="sxs-lookup"><span data-stu-id="87783-152">When the JSON or XML formatter tries to serialize the graph, it will throw an exception.</span></span> <span data-ttu-id="87783-153">Les deux formateurs lèvent différents messages d’exception.</span><span class="sxs-lookup"><span data-stu-id="87783-153">The two formatters throw different exception messages.</span></span> <span data-ttu-id="87783-154">Voici un exemple pour le formateur JSON :</span><span class="sxs-lookup"><span data-stu-id="87783-154">Here is an example for the JSON formatter:</span></span>

[!code-console[Main](part-4/samples/sample12.cmd)]

<span data-ttu-id="87783-155">Voici le formateur XML :</span><span class="sxs-lookup"><span data-stu-id="87783-155">Here is the XML formatter:</span></span>

[!code-xml[Main](part-4/samples/sample13.xml)]

<span data-ttu-id="87783-156">Une solution consiste à utiliser les DTO, que je décrirai dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="87783-156">One solution is to use DTOs, which I describe in the next section.</span></span> <span data-ttu-id="87783-157">Vous pouvez également configurer les formateurs JSON et XML pour gérer les cycles graphiques.</span><span class="sxs-lookup"><span data-stu-id="87783-157">Alternatively, you can configure the JSON and XML formatters to handle graph cycles.</span></span> <span data-ttu-id="87783-158">Pour plus d’informations, consultez [gestion des références d’objets circulaires](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span><span class="sxs-lookup"><span data-stu-id="87783-158">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span></span>

<span data-ttu-id="87783-159">Pour ce didacticiel, vous n’avez pas besoin de la propriété de navigation `Author.Book`, vous pouvez donc la conserver.</span><span class="sxs-lookup"><span data-stu-id="87783-159">For this tutorial, you don't need the `Author.Book` navigation property, so you can leave it out.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="87783-160">[Précédent](part-3.md)
> [Suivant](part-5.md)</span><span class="sxs-lookup"><span data-stu-id="87783-160">[Previous](part-3.md)
[Next](part-5.md)</span></span>
