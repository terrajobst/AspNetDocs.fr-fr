---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: 'Partie 2 : création des modèles de domaine | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/03/2012
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: 7c5ed1bdb4b390c94907b14e208231f16ad42d96
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600359"
---
# <a name="part-2-creating-the-domain-models"></a><span data-ttu-id="af5ab-102">Partie 2 : création des modèles de domaine</span><span class="sxs-lookup"><span data-stu-id="af5ab-102">Part 2: Creating the Domain Models</span></span>

<span data-ttu-id="af5ab-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="af5ab-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="af5ab-104">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="af5ab-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a><span data-ttu-id="af5ab-105">Ajouter des modèles</span><span class="sxs-lookup"><span data-stu-id="af5ab-105">Add Models</span></span>

<span data-ttu-id="af5ab-106">Il existe trois façons d’aborder Entity Framework :</span><span class="sxs-lookup"><span data-stu-id="af5ab-106">There are three ways to approach Entity Framework:</span></span>

- <span data-ttu-id="af5ab-107">Base de données-tout d’abord : vous démarrez avec une base de données et Entity Framework génère le code.</span><span class="sxs-lookup"><span data-stu-id="af5ab-107">Database-first: You start with a database, and Entity Framework generates the code.</span></span>
- <span data-ttu-id="af5ab-108">Modèle-First : vous démarrez avec un modèle visuel et Entity Framework génère la base de données et le code.</span><span class="sxs-lookup"><span data-stu-id="af5ab-108">Model-first: You start with a visual model, and Entity Framework generates both the database and code.</span></span>
- <span data-ttu-id="af5ab-109">Code-First : vous commencez avec le code et Entity Framework génère la base de données.</span><span class="sxs-lookup"><span data-stu-id="af5ab-109">Code-first: You start with code, and Entity Framework generates the database.</span></span>

<span data-ttu-id="af5ab-110">Nous utilisons l’approche code First, donc nous commençons par définir nos objets de domaine en tant qu’objets POCO (Plain-Old CLR Objects).</span><span class="sxs-lookup"><span data-stu-id="af5ab-110">We are using the code-first approach, so we start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="af5ab-111">Avec l’approche code First, les objets de domaine n’ont pas besoin de code supplémentaire pour prendre en charge la couche de base de données, tels que les transactions ou la persistance.</span><span class="sxs-lookup"><span data-stu-id="af5ab-111">With the code-first approach, domain objects don't need any extra code to support the database layer, such as transactions or persistence.</span></span> <span data-ttu-id="af5ab-112">(En particulier, ils n’ont pas besoin d’hériter de la classe [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) .) Vous pouvez toujours utiliser des annotations de données pour contrôler la façon dont Entity Framework crée le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="af5ab-112">(Specifically, they do not need to inherit from the [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) class.) You can still use data annotations to control how Entity Framework creates the database schema.</span></span>

<span data-ttu-id="af5ab-113">Comme les POCO ne transmettent pas de propriétés supplémentaires qui décrivent l' [État de la base de données](https://msdn.microsoft.com/library/system.data.entitystate.aspx), elles peuvent facilement être sérialisées en JSON ou XML.</span><span class="sxs-lookup"><span data-stu-id="af5ab-113">Because POCOs do not carry any extra properties that describe [database state](https://msdn.microsoft.com/library/system.data.entitystate.aspx), they can easily be serialized to JSON or XML.</span></span> <span data-ttu-id="af5ab-114">Toutefois, cela ne signifie pas que vous devez toujours exposer vos modèles de Entity Framework directement aux clients, comme nous le verrons plus loin dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="af5ab-114">However, that does not mean you should always expose your Entity Framework models directly to clients, as we'll see later in the tutorial.</span></span>

<span data-ttu-id="af5ab-115">Nous allons créer les POCO suivants :</span><span class="sxs-lookup"><span data-stu-id="af5ab-115">We will create the following POCOs:</span></span>

- <span data-ttu-id="af5ab-116">Produit</span><span class="sxs-lookup"><span data-stu-id="af5ab-116">Product</span></span>
- <span data-ttu-id="af5ab-117">Order</span><span class="sxs-lookup"><span data-stu-id="af5ab-117">Order</span></span>
- <span data-ttu-id="af5ab-118">OrderDetail</span><span class="sxs-lookup"><span data-stu-id="af5ab-118">OrderDetail</span></span>

<span data-ttu-id="af5ab-119">Pour créer chaque classe, cliquez avec le bouton droit sur le dossier Models dans Explorateur de solutions.</span><span class="sxs-lookup"><span data-stu-id="af5ab-119">To create each class, right-click the Models folder in Solution Explorer.</span></span> <span data-ttu-id="af5ab-120">Dans le menu contextuel, sélectionnez **Ajouter** , puis **classe.**</span><span class="sxs-lookup"><span data-stu-id="af5ab-120">From the context menu, select **Add** and then select **Class.**</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

<span data-ttu-id="af5ab-121">Ajoutez une classe `Product` avec l’implémentation suivante :</span><span class="sxs-lookup"><span data-stu-id="af5ab-121">Add a `Product` class with the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

<span data-ttu-id="af5ab-122">Par Convention, Entity Framework utilise la propriété `Id` comme clé primaire et la mappe à une colonne d’identité dans la table de base de données.</span><span class="sxs-lookup"><span data-stu-id="af5ab-122">By convention, Entity Framework uses the `Id` property as the primary key and maps it to an identity column in the database table.</span></span> <span data-ttu-id="af5ab-123">Lorsque vous créez une instance `Product`, vous ne définissez pas de valeur pour `Id`, car la base de données génère la valeur.</span><span class="sxs-lookup"><span data-stu-id="af5ab-123">When you create a new `Product` instance, you won't set a value for `Id`, because the database generates the value.</span></span>

<span data-ttu-id="af5ab-124">L’attribut **ScaffoldColumn** indique à ASP.NET MVC d’ignorer la propriété `Id` lors de la génération d’un formulaire de l’éditeur.</span><span class="sxs-lookup"><span data-stu-id="af5ab-124">The **ScaffoldColumn** attribute tells ASP.NET MVC to skip the `Id` property when generating an editor form.</span></span> <span data-ttu-id="af5ab-125">L’attribut **Required** est utilisé pour valider le modèle.</span><span class="sxs-lookup"><span data-stu-id="af5ab-125">The **Required** attribute is used to validate the model.</span></span> <span data-ttu-id="af5ab-126">Il spécifie que la propriété `Name` doit être une chaîne non vide.</span><span class="sxs-lookup"><span data-stu-id="af5ab-126">It specifies that the `Name` property must be a non-empty string.</span></span>

<span data-ttu-id="af5ab-127">Ajoutez la classe `Order` :</span><span class="sxs-lookup"><span data-stu-id="af5ab-127">Add the `Order` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

<span data-ttu-id="af5ab-128">Ajoutez la classe `OrderDetail` :</span><span class="sxs-lookup"><span data-stu-id="af5ab-128">Add the `OrderDetail` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a><span data-ttu-id="af5ab-129">Relations de clé étrangère</span><span class="sxs-lookup"><span data-stu-id="af5ab-129">Foreign Key Relations</span></span>

<span data-ttu-id="af5ab-130">Une commande contient un grand nombre de détails de commande et chaque détail de commande fait référence à un produit unique.</span><span class="sxs-lookup"><span data-stu-id="af5ab-130">An order contains many order details, and each order detail refers to a single product.</span></span> <span data-ttu-id="af5ab-131">Pour représenter ces relations, la classe `OrderDetail` définit des propriétés nommées `OrderId` et `ProductId`.</span><span class="sxs-lookup"><span data-stu-id="af5ab-131">To represent these relations, the `OrderDetail` class defines properties named `OrderId` and `ProductId`.</span></span> <span data-ttu-id="af5ab-132">Entity Framework déduirea que ces propriétés représentent des clés étrangères et ajoutera des contraintes de clé étrangère à la base de données.</span><span class="sxs-lookup"><span data-stu-id="af5ab-132">Entity Framework will infer that these properties represent foreign keys, and will add foreign-key constraints to the database.</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

<span data-ttu-id="af5ab-133">Les classes `Order` et `OrderDetail` incluent également des propriétés de « navigation », qui contiennent des références aux objets connexes.</span><span class="sxs-lookup"><span data-stu-id="af5ab-133">The `Order` and `OrderDetail` classes also include "navigation" properties, which contain references to the related objects.</span></span> <span data-ttu-id="af5ab-134">Dans un ordre donné, vous pouvez accéder aux produits dans l’ordre en suivant les propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="af5ab-134">Given an order, you can navigate to the products in the order by following the navigation properties.</span></span>

<span data-ttu-id="af5ab-135">Compilez le projet maintenant.</span><span class="sxs-lookup"><span data-stu-id="af5ab-135">Compile the project now.</span></span> <span data-ttu-id="af5ab-136">Entity Framework utilise la réflexion pour découvrir les propriétés des modèles. il requiert donc un assembly compilé pour créer le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="af5ab-136">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

## <a name="configure-the-media-type-formatters"></a><span data-ttu-id="af5ab-137">Configurer les formateurs de type de média</span><span class="sxs-lookup"><span data-stu-id="af5ab-137">Configure the Media-Type Formatters</span></span>

<span data-ttu-id="af5ab-138">Un [formateur de type de média](../../formats-and-model-binding/media-formatters.md) est un objet qui sérialise vos données lorsque l’API Web écrit le corps de la réponse http.</span><span class="sxs-lookup"><span data-stu-id="af5ab-138">A [media-type formatter](../../formats-and-model-binding/media-formatters.md) is an object that serializes your data when Web API writes the HTTP response body.</span></span> <span data-ttu-id="af5ab-139">Les formateurs intégrés prennent en charge la sortie JSON et XML.</span><span class="sxs-lookup"><span data-stu-id="af5ab-139">The built-in formatters support JSON and XML output.</span></span> <span data-ttu-id="af5ab-140">Par défaut, ces deux formateurs sérialisent tous les objets par valeur.</span><span class="sxs-lookup"><span data-stu-id="af5ab-140">By default, both of these formatters serialize all objects by value.</span></span>

<span data-ttu-id="af5ab-141">La sérialisation par valeur crée un problème si un graphique d’objet contient des références circulaires.</span><span class="sxs-lookup"><span data-stu-id="af5ab-141">Serialization by value creates a problem if an object graph contains circular references.</span></span> <span data-ttu-id="af5ab-142">C’est exactement le cas avec les classes `Order` et `OrderDetail`, car chacune contient une référence à l’autre.</span><span class="sxs-lookup"><span data-stu-id="af5ab-142">That's exactly the case with the `Order` and `OrderDetail` classes, because each holds a reference to the other.</span></span> <span data-ttu-id="af5ab-143">Le formateur suivra les références, en écrivant chaque objet par valeur, et en suivant des cercles.</span><span class="sxs-lookup"><span data-stu-id="af5ab-143">The formatter will follow the references, writing each object by value, and go in circles.</span></span> <span data-ttu-id="af5ab-144">Par conséquent, nous devons modifier le comportement par défaut.</span><span class="sxs-lookup"><span data-stu-id="af5ab-144">Therefore, we need to change the default behavior.</span></span>

<span data-ttu-id="af5ab-145">Dans Explorateur de solutions, développez le dossier de démarrage de l’application\_et ouvrez le fichier nommé WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="af5ab-145">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="af5ab-146">Ajoutez le code suivant à la classe `WebApiConfig` :</span><span class="sxs-lookup"><span data-stu-id="af5ab-146">Add the following code to the `WebApiConfig` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

<span data-ttu-id="af5ab-147">Ce code définit le formateur JSON pour conserver les références d’objet et supprime entièrement le formateur XML du pipeline.</span><span class="sxs-lookup"><span data-stu-id="af5ab-147">This code sets the JSON formatter to preserve object references, and removes the XML formatter from the pipeline entirely.</span></span> <span data-ttu-id="af5ab-148">(Vous pouvez configurer le formateur XML pour conserver les références d’objet, mais il s’agit d’un peu plus de travail, et nous avons uniquement besoin de JSON pour cette application.</span><span class="sxs-lookup"><span data-stu-id="af5ab-148">(You can configure the XML formatter to preserve object references, but it's a little more work, and we only need JSON for this application.</span></span> <span data-ttu-id="af5ab-149">Pour plus d’informations, consultez [gestion des références d’objets circulaires](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span><span class="sxs-lookup"><span data-stu-id="af5ab-149">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="af5ab-150">[Précédent](using-web-api-with-entity-framework-part-1.md)
> [Suivant](using-web-api-with-entity-framework-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="af5ab-150">[Previous](using-web-api-with-entity-framework-part-1.md)
[Next](using-web-api-with-entity-framework-part-3.md)</span></span>
