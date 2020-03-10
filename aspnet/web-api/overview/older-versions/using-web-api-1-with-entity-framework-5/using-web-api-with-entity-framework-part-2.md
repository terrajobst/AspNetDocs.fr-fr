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
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78621820"
---
# <a name="part-2-creating-the-domain-models"></a>Partie 2 : création des modèles de domaine

par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a>Ajouter des modèles

Il existe trois façons d’aborder Entity Framework :

- Base de données-tout d’abord : vous démarrez avec une base de données et Entity Framework génère le code.
- Modèle-First : vous démarrez avec un modèle visuel et Entity Framework génère la base de données et le code.
- Code-First : vous commencez avec le code et Entity Framework génère la base de données.

Nous utilisons l’approche code First, donc nous commençons par définir nos objets de domaine en tant qu’objets POCO (Plain-Old CLR Objects). Avec l’approche code First, les objets de domaine n’ont pas besoin de code supplémentaire pour prendre en charge la couche de base de données, tels que les transactions ou la persistance. (En particulier, ils n’ont pas besoin d’hériter de la classe [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) .) Vous pouvez toujours utiliser des annotations de données pour contrôler la façon dont Entity Framework crée le schéma de base de données.

Comme les POCO ne transmettent pas de propriétés supplémentaires qui décrivent l' [État de la base de données](https://msdn.microsoft.com/library/system.data.entitystate.aspx), elles peuvent facilement être sérialisées en JSON ou XML. Toutefois, cela ne signifie pas que vous devez toujours exposer vos modèles de Entity Framework directement aux clients, comme nous le verrons plus loin dans ce didacticiel.

Nous allons créer les POCO suivants :

- Produit
- Trier
- OrderDetail

Pour créer chaque classe, cliquez avec le bouton droit sur le dossier Models dans Explorateur de solutions. Dans le menu contextuel, sélectionnez **Ajouter** , puis **classe.**

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

Ajoutez une classe `Product` avec l’implémentation suivante :

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

Par Convention, Entity Framework utilise la propriété `Id` comme clé primaire et la mappe à une colonne d’identité dans la table de base de données. Lorsque vous créez une instance `Product`, vous ne définissez pas de valeur pour `Id`, car la base de données génère la valeur.

L’attribut **ScaffoldColumn** indique à ASP.NET MVC d’ignorer la propriété `Id` lors de la génération d’un formulaire de l’éditeur. L’attribut **Required** est utilisé pour valider le modèle. Il spécifie que la propriété `Name` doit être une chaîne non vide.

Ajoutez la classe `Order` :

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

Ajoutez la classe `OrderDetail` :

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a>Relations de clé étrangère

Une commande contient un grand nombre de détails de commande et chaque détail de commande fait référence à un produit unique. Pour représenter ces relations, la classe `OrderDetail` définit des propriétés nommées `OrderId` et `ProductId`. Entity Framework déduirea que ces propriétés représentent des clés étrangères et ajoutera des contraintes de clé étrangère à la base de données.

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

Les classes `Order` et `OrderDetail` incluent également des propriétés de « navigation », qui contiennent des références aux objets connexes. Dans un ordre donné, vous pouvez accéder aux produits dans l’ordre en suivant les propriétés de navigation.

Compilez le projet maintenant. Entity Framework utilise la réflexion pour découvrir les propriétés des modèles. il requiert donc un assembly compilé pour créer le schéma de base de données.

## <a name="configure-the-media-type-formatters"></a>Configurer les formateurs de type de média

Un [formateur de type de média](../../formats-and-model-binding/media-formatters.md) est un objet qui sérialise vos données lorsque l’API Web écrit le corps de la réponse http. Les formateurs intégrés prennent en charge la sortie JSON et XML. Par défaut, ces deux formateurs sérialisent tous les objets par valeur.

La sérialisation par valeur crée un problème si un graphique d’objet contient des références circulaires. C’est exactement le cas avec les classes `Order` et `OrderDetail`, car chacune contient une référence à l’autre. Le formateur suivra les références, en écrivant chaque objet par valeur, et en suivant des cercles. Par conséquent, nous devons modifier le comportement par défaut.

Dans Explorateur de solutions, développez le dossier de démarrage de l’application\_et ouvrez le fichier nommé WebApiConfig.cs. Ajoutez le code suivant à la classe `WebApiConfig` :

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

Ce code définit le formateur JSON pour conserver les références d’objet et supprime entièrement le formateur XML du pipeline. (Vous pouvez configurer le formateur XML pour conserver les références d’objet, mais il s’agit d’un peu plus de travail, et nous avons uniquement besoin de JSON pour cette application. Pour plus d’informations, consultez [gestion des références d’objets circulaires](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)

> [!div class="step-by-step"]
> [Précédent](using-web-api-with-entity-framework-part-1.md)
> [Suivant](using-web-api-with-entity-framework-part-3.md)
