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
# <a name="handling-entity-relations"></a>Gestion des relations d’entité

par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](https://github.com/MikeWasson/BookService)

Cette section décrit en détail comment EF charge les entités associées et comment gérer les propriétés de navigation circulaires dans vos classes de modèle. (Cette section fournit des informations générales et n’est pas nécessaire pour suivre le didacticiel. Si vous préférez, passez à la [partie 5.](part-5.md).)

## <a name="eager-loading-versus-lazy-loading"></a>Chargement hâtif et chargement différé

Lorsque EF est utilisé avec une base de données relationnelle, il est important de comprendre comment EF charge les données associées.

Il est également utile de voir les requêtes SQL générées par EF. Pour tracer le SQL, ajoutez la ligne de code suivante au constructeur `BookServiceContext` :

[!code-csharp[Main](part-4/samples/sample1.cs)]

Si vous envoyez une requête d’extraction à/API/Books, elle retourne JSON comme suit :

[!code-console[Main](part-4/samples/sample2.cmd)]

Vous pouvez voir que la propriété auteur a la valeur null, même si le livre contient un type d’auteur valide. Cela est dû au fait que EF ne charge pas les entités d’auteur associées. Le journal des traces de la requête SQL confirme ce qui suit :

[!code-console[Main](part-4/samples/sample3.sql)]

L’instruction SELECT prend la table Books et ne fait pas référence à la table author.

Pour référence, voici la méthode dans la classe `BooksController` qui retourne la liste des livres.

[!code-csharp[Main](part-4/samples/sample4.cs)]

Voyons comment nous pouvons retourner l’auteur dans le cadre des données JSON. Il existe trois façons de charger des données associées dans Entity Framework : le chargement hâtif, le chargement différé et le chargement explicite. Il existe des compromis avec chaque technique. il est donc important de comprendre comment elles fonctionnent.

### <a name="eager-loading"></a>Chargement hâtif

Avec le *chargement hâtif*, EF charge les entités associées dans le cadre de la requête de base de données initiale. Pour effectuer un chargement hâtif, utilisez la méthode d’extension **System. Data. Entity. include** .

[!code-csharp[Main](part-4/samples/sample5.cs)]

Cela indique à EF d’inclure les données de l’auteur dans la requête. Si vous apportez cette modification et exécutez l’application, les données JSON se présenteront comme suit :

[!code-console[Main](part-4/samples/sample6.cmd)]

Le journal des traces indique que EF a effectué une jointure sur le livre et les tables de création.

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a>Chargement différé

Avec le chargement différé, EF charge automatiquement une entité associée lorsque la propriété de navigation de cette entité est déréférencée. Pour activer le chargement différé, rendez la propriété de navigation virtuelle. Par exemple, dans la classe Book :

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

Examinons maintenant le code suivant :

[!code-csharp[Main](part-4/samples/sample9.cs)]

Lorsque le chargement différé est activé, l’accès à la propriété `Author` sur `books[0]` force EF à interroger la base de données pour l’auteur.

Le chargement différé nécessite plusieurs trajets de base de données, car EF envoie une requête chaque fois qu’il récupère une entité associée. En règle générale, vous souhaitez que le chargement différé soit désactivé pour les objets que vous sérialisez. Le sérialiseur doit lire toutes les propriétés du modèle, ce qui déclenche le chargement des entités associées. Par exemple, Voici les requêtes SQL lorsque EF sérialise la liste des livres pour lesquels le chargement différé est activé. Vous pouvez voir que EF effectue trois requêtes distinctes pour les trois auteurs.

[!code-console[Main](part-4/samples/sample10.sql)]

Il se peut encore que vous souhaitiez utiliser le chargement différé. Le chargement hâtif peut provoquer la génération d’une jointure très complexe par EF. Vous pouvez également avoir besoin d’entités associées pour un petit sous-ensemble des données, et le chargement différé serait plus efficace.

Pour éviter les problèmes de sérialisation, il est possible de sérialiser des objets DTO (Data Transfer Objects) au lieu d’objets d’entité. Je vais vous montrer cette approche plus loin dans cet article.

### <a name="explicit-loading"></a>Chargement explicite

Le chargement explicite est semblable au chargement différé, à ceci près que vous récupérez explicitement les données associées dans le code ; elle ne se produit pas automatiquement lorsque vous accédez à une propriété de navigation. Le chargement explicite vous permet de mieux contrôler le moment du chargement des données associées, mais nécessite du code supplémentaire. Pour plus d’informations sur le chargement explicite, consultez [chargement des entités associées](https://msdn.microsoft.com/data/jj574232#explicit).

## <a name="navigation-properties-and-circular-references"></a>Propriétés de navigation et références circulaires

Lorsque j’ai défini les modèles Book et Author, j’ai défini une propriété de navigation sur la classe `Book` pour la relation livre-Author, mais je n’ai pas défini de propriété de navigation dans l’autre direction.

Que se passe-t-il si vous ajoutez la propriété de navigation correspondante à la classe `Author` ?

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

Malheureusement, cela crée un problème quand vous sérialisez les modèles. Si vous chargez les données associées, elle crée un graphique d’objet circulaire.

![](part-4/_static/image1.png)

Lorsque le formateur JSON ou XML tente de sérialiser le graphique, il lève une exception. Les deux formateurs lèvent différents messages d’exception. Voici un exemple pour le formateur JSON :

[!code-console[Main](part-4/samples/sample12.cmd)]

Voici le formateur XML :

[!code-xml[Main](part-4/samples/sample13.xml)]

Une solution consiste à utiliser les DTO, que je décrirai dans la section suivante. Vous pouvez également configurer les formateurs JSON et XML pour gérer les cycles graphiques. Pour plus d’informations, consultez [gestion des références d’objets circulaires](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).

Pour ce didacticiel, vous n’avez pas besoin de la propriété de navigation `Author.Book`, vous pouvez donc la conserver.

> [!div class="step-by-step"]
> [Précédent](part-3.md)
> [Suivant](part-5.md)
