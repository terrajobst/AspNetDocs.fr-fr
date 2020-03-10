---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: Prise en charge des relations d’entité dans OData v3 avec l’API Web 2 | Microsoft Docs
author: MikeWasson
description: 'La plupart des jeux de données définissent les relations entre les entités : les clients ont des commandes. les ouvrages ont des auteurs ; les produits ont des fournisseurs. À l’aide d’OData, les clients peuvent naviguer...'
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: 726a7d51123805e05f6831ef9cd7eaa84b6c44bd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598741"
---
# <a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a>Prise en charge des relations d’entité dans OData v3 avec l’API Web 2

par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> La plupart des jeux de données définissent les relations entre les entités : les clients ont des commandes. les ouvrages ont des auteurs ; les produits ont des fournisseurs. À l’aide d’OData, les clients peuvent naviguer sur les relations d’entité. Pour un produit donné, vous pouvez trouver le fournisseur. Vous pouvez également créer ou supprimer des relations. Par exemple, vous pouvez définir le fournisseur d’un produit.
> 
> Ce didacticiel montre comment prendre en charge ces opérations dans API Web ASP.NET. Ce didacticiel s’appuie sur le didacticiel [création d’un point de terminaison OData v3 avec l’API Web 2](creating-an-odata-endpoint.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
> 
> 
> - API Web 2
> - OData version 3
> - Entity Framework 6

## <a name="add-a-supplier-entity"></a>Ajouter une entité fournisseur

Tout d’abord, nous devons ajouter un nouveau type d’entité à notre flux OData. Nous allons ajouter une classe `Supplier`.

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

Cette classe utilise une chaîne pour la clé d’entité. Dans la pratique, cela peut être moins courant que l’utilisation d’une clé entière. Toutefois, il est important de voir comment OData gère d’autres types de clés en plus des entiers.

Ensuite, nous allons créer une relation en ajoutant une propriété `Supplier` à la classe `Product` :

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

Ajoutez un nouveau **DbSet** à la classe `ProductServiceContext`, afin que Entity Framework inclue la table `Supplier` dans la base de données.

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

Dans WebApiConfig.cs, ajoutez une entité « Suppliers » au modèle EDM :

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a>Propriétés de navigation

Pour obtenir le fournisseur d’un produit, le client envoie une demande d’extraction :

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

Ici, « Supplier » est une propriété de navigation sur le type de `Product`. Dans ce cas, `Supplier` fait référence à un seul élément, mais une propriété de navigation peut également retourner une collection (relation un-à-plusieurs ou plusieurs-à-plusieurs).

Pour prendre en charge cette demande, ajoutez la méthode suivante à la classe `ProductsController` :

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

Le paramètre de *clé* est la clé du produit. La méthode retourne l’entité&#8212;associée dans ce cas, une instance de `Supplier`. Le nom de la méthode et le nom du paramètre sont tous deux importants. En général, si la propriété de navigation est nommée « X », vous devez ajouter une méthode nommée « GetX ». La méthode doit prendre un paramètre nommé «*Key*» qui correspond au type de données de la clé du parent.

Il est également important d’inclure l’attribut **[FromOdataUri]** dans le paramètre de *clé* . Cet attribut indique à l’API Web d’utiliser les règles de syntaxe OData lorsqu’elle analyse la clé à partir de l’URI de la demande.

## <a name="creating-and-deleting-links"></a>Création et suppression de liens

OData prend en charge la création ou la suppression de relations entre deux entités. Dans la terminologie OData, la relation est un « lien ». Chaque lien a un URI au format *entité*/$Links/*entité*. Par exemple, le lien du produit au fournisseur ressemble à ceci :

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

Pour créer un nouveau lien, le client envoie une demande de publication à l’URI de lien. Le corps de la demande est l’URI de l’entité cible. Par exemple, supposons qu’il existe un fournisseur avec la clé « CTSO ». Pour créer un lien à partir de « Product (1) » vers « Supplier («CTSO »)», le client envoie une requête semblable à la suivante :

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

Pour supprimer un lien, le client envoie une requête DELETE à l’URI du lien.

**Création de liens**

Pour permettre à un client de créer des liens vers des fournisseurs de produits, ajoutez le code suivant à la classe `ProductsController` :

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

Cette méthode accepte trois paramètres :

- *clé*: clé de l’entité parente (le produit)
- *navigationProperty*: nom de la propriété de navigation. Dans cet exemple, la seule propriété de navigation valide est « Supplier ».
- *lien*: URI OData de l’entité associée. Cette valeur est extraite du corps de la demande. Par exemple, l’URI de lien peut être «`http://localhost/odata/Suppliers('CTSO')`, ce qui signifie que le fournisseur avec ID = «CTSO ».

La méthode utilise le lien pour rechercher le fournisseur. Si le fournisseur correspondant est trouvé, la méthode définit la propriété `Product.Supplier` et enregistre le résultat dans la base de données.

La partie la plus difficile est l’analyse de l’URI de lien. Fondamentalement, vous devez simuler le résultat de l’envoi d’une demande d’extraction à cet URI. La méthode d’assistance suivante indique comment procéder. La méthode appelle le processus de routage de l’API Web et retourne une instance **ODataPath** qui représente le chemin d’accès OData analysé. Pour un URI de lien, l’un des segments doit être la clé d’entité. (Dans le cas contraire, le client a envoyé un URI incorrect.)

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

**Suppression de liens**

Pour supprimer un lien, ajoutez le code suivant à la classe `ProductsController` :

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

Dans cet exemple, la propriété de navigation est une entité `Supplier` unique. Si la propriété de navigation est une collection, l’URI pour supprimer un lien doit inclure une clé pour l’entité associée. Exemple :

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

Cette demande supprime la commande 1 du client 1. Dans ce cas, la méthode DeleteLink aura la signature suivante :

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

Le paramètre *relatedKey* indique la clé de l’entité associée. Ainsi, dans votre méthode `DeleteLink`, recherchez l’entité principale par le paramètre de *clé* , recherchez l’entité associée par le paramètre *relatedKey* , puis supprimez l’Association. Selon votre modèle de données, vous devrez peut-être implémenter les deux versions de `DeleteLink`. L’API Web appellera la version appropriée en fonction de l’URI de la demande.
