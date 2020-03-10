---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: Relations d’entité dans OData v4 à l’aide de API Web ASP.NET 2,2 | Microsoft Docs
author: MikeWasson
description: 'La plupart des jeux de données définissent les relations entre les entités : les clients ont des commandes. les ouvrages ont des auteurs ; les produits ont des fournisseurs. À l’aide d’OData, les clients peuvent naviguer...'
ms.author: riande
ms.date: 06/26/2014
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: fbafb2b2346689271905db5790cdddeeb809b070
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598692"
---
# <a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a>Relations d’entité dans OData v4 à l’aide de API Web ASP.NET 2,2

par [Mike Wasson](https://github.com/MikeWasson)

> La plupart des jeux de données définissent les relations entre les entités : les clients ont des commandes. les ouvrages ont des auteurs ; les produits ont des fournisseurs. À l’aide d’OData, les clients peuvent naviguer sur les relations d’entité. Pour un produit donné, vous pouvez trouver le fournisseur. Vous pouvez également créer ou supprimer des relations. Par exemple, vous pouvez définir le fournisseur d’un produit.
>
> Ce didacticiel montre comment prendre en charge ces opérations dans OData v4 à l’aide de API Web ASP.NET. Le didacticiel s’appuie sur le didacticiel [créer un point de terminaison OData v4 à l’aide de API Web ASP.NET 2](create-an-odata-v4-endpoint.md).
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
>
> - API Web 2,1
> - OData v4
> - Visual Studio 2013 (Téléchargez Visual Studio 2017 [ici](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))
> - Entity Framework 6
> - .NET 4.5
>
> ## <a name="tutorial-versions"></a>Versions du didacticiel
>
> Pour la version 3 d’OData, consultez [prise en charge des relations d’entité dans OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).

## <a name="add-a-supplier-entity"></a>Ajouter une entité fournisseur

> [!NOTE]
> Le didacticiel s’appuie sur le didacticiel [créer un point de terminaison OData v4 à l’aide de API Web ASP.NET 2](create-an-odata-v4-endpoint.md).

Tout d’abord, nous avons besoin d’une entité associée. Ajoutez une classe nommée `Supplier` dans le dossier Models.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

Ajoutez une propriété de navigation à la classe `Product` :

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

Ajoutez un nouveau **DbSet** à la classe `ProductsContext`, afin que Entity Framework inclue la table Supplier dans la base de données.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

Dans WebApiConfig.cs, ajoutez un jeu d’entités &quot;Suppliers&quot; à l’Entity Data Model :

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a>Ajouter un contrôleur de fournisseurs

Ajoutez une classe `SuppliersController` au dossier Controllers.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

Je n’indique pas comment ajouter des opérations CRUD pour ce contrôleur. Les étapes sont les mêmes que pour le contrôleur Products (voir [créer un point de terminaison OData v4](create-an-odata-v4-endpoint.md)).

## <a name="getting-related-entities"></a>Obtention des entités associées

Pour obtenir le fournisseur d’un produit, le client envoie une demande d’extraction :

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

Pour prendre en charge cette demande, ajoutez la méthode suivante à la classe `ProductsController` :

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

Cette méthode utilise une convention d’affectation de noms par défaut

- Nom de la méthode : GetX, où X est la propriété de navigation.
- Nom du paramètre : *clé*

Si vous suivez cette Convention d’affectation de noms, l’API Web mappe automatiquement la requête HTTP à la méthode de contrôleur.

Exemple de requête HTTP :

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

Exemple de réponse HTTP :

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a>Obtention d’un regroupement connexe

Dans l’exemple précédent, un produit a un fournisseur. Une propriété de navigation peut également retourner une collection. Le code suivant obtient les produits pour un fournisseur :

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

Dans ce cas, la méthode retourne un **IQueryable** à la place d’un **SingleResult&lt;t&gt;**

Exemple de requête HTTP :

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

Exemple de réponse HTTP :

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a>Création d’une relation entre des entités

OData prend en charge la création ou la suppression de relations entre deux entités existantes. Dans la terminologie OData v4, la relation est une&quot;de référence &quot;. (Dans OData v3, la relation était appelée un *lien*. Les différences de protocole n’ont pas d’importance pour ce didacticiel.)

Une référence a son propre URI, avec la forme `/Entity/NavigationProperty/$ref`. Par exemple, voici l’URI pour traiter la référence entre un produit et son fournisseur :

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

Pour ajouter une relation, le client envoie une demande de publication ou de placement à cette adresse.

- Placez si la propriété de navigation est une entité unique, par exemple `Product.Supplier`.
- PUBLIEz si la propriété de navigation est une collection, telle que `Supplier.Products`.

Le corps de la requête contient l’URI de l’autre entité dans la relation. Voici un exemple de demande :

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

Dans cet exemple, le client envoie une demande PUT à `/Products(6)/Supplier/$ref`, qui est l’URI $ref pour le `Supplier` du produit avec ID = 6. Si la demande est réussie, le serveur envoie une réponse 204 (sans contenu) :

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

Voici la méthode du contrôleur pour ajouter une relation à un `Product`:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

Le paramètre *navigationProperty* spécifie la relation à définir. (S’il existe plusieurs propriétés de navigation sur l’entité, vous pouvez ajouter d’autres instructions `case`.)

Le paramètre *Link* contient l’URI du fournisseur. L’API Web analyse automatiquement le corps de la requête pour obtenir la valeur de ce paramètre.

Pour rechercher le fournisseur, nous avons besoin de l’ID (ou de la clé), qui fait partie du paramètre *Link* . Pour ce faire, utilisez la méthode d’assistance suivante :

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

Fondamentalement, cette méthode utilise la bibliothèque OData pour fractionner le chemin d’accès de l’URI en segments, Rechercher le segment qui contient la clé et convertir la clé dans le type approprié.

## <a name="deleting-a-relationship-between-entities"></a>Suppression d’une relation entre des entités

Pour supprimer une relation, le client envoie une requête HTTP DELETE à l’URI de la $ref :

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

Voici la méthode du contrôleur pour supprimer la relation entre un produit et un fournisseur :

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

Dans ce cas, `Product.Supplier` est la terminaison &quot;1&quot; d’une relation un-à-plusieurs, de sorte que vous pouvez supprimer la relation simplement en définissant `Product.Supplier` sur `null`.

Dans le &quot;plusieurs&quot; terminaison d’une relation, le client doit spécifier l’entité associée à supprimer. Pour ce faire, le client envoie l’URI de l’entité associée dans la chaîne de requête de la requête. Par exemple, pour supprimer « Product 1 » de « Supplier 1 » :

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

Pour la prendre en charge dans l’API Web, nous devons inclure un paramètre supplémentaire dans la méthode `DeleteRef`. Voici la méthode du contrôleur pour supprimer un produit de la relation `Supplier.Products`.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

Le paramètre de *clé* est la clé du fournisseur, et le paramètre *relatedKey* est la clé du produit à supprimer de la relation `Products`. Notez que l’API Web obtient automatiquement la clé à partir de la chaîne de requête.
