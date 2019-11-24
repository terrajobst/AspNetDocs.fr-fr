---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: Créer des objets Transfert de données (DTO) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: fc0463420207eba764014b8ec7123c5150e38247
ms.sourcegitcommit: 84b1681d4e6253e30468c8df8a09fe03beea9309
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2019
ms.locfileid: "73445756"
---
# <a name="create-data-transfer-objects-dtos"></a>Créer des objets de transfert de données (DTO)

par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](https://github.com/MikeWasson/BookService)

Pour l’instant, notre API Web expose les entités de base de données au client. Le client reçoit des données qui sont mappées directement à vos tables de base de données. Toutefois, cela n’est pas toujours une bonne idée. Parfois, vous souhaitez modifier la forme des données que vous envoyez au client. Vous pouvez par exemple décider d'effectuer les opérations suivantes :

- Supprimez les références circulaires (voir la section précédente).
- Masquer les propriétés particulières que les clients ne sont pas censés afficher.
- Omettez certaines propriétés afin de réduire la taille de la charge utile.
- Aplatir les graphiques d’objets qui contiennent des objets imbriqués, afin de les rendre plus pratiques pour les clients.
- Évitez les vulnérabilités de « dépassement de la publication ». (Consultez [validation de modèle](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) pour une discussion sur la survalidation.)
- Découplez votre couche de service de votre couche de base de données.

Pour ce faire, vous pouvez définir un *objet de transfert de données* (DTO). Un DTO est un objet qui définit la façon dont les données sont envoyées sur le réseau. Voyons comment cela fonctionne avec l’entité Book. Dans le dossier Models, ajoutez deux classes DTO :

[!code-csharp[Main](part-5/samples/sample1.cs)]

La classe `BookDetailDto` comprend toutes les propriétés du modèle Book, sauf que `AuthorName` est une chaîne qui contiendra le nom de l’auteur. La classe `BookDto` contient un sous-ensemble de propriétés de `BookDetailDto`.

Ensuite, remplacez les deux méthodes d’extraction de la classe `BooksController`, par les versions qui retournent les objets DTO. Nous allons utiliser l’instruction LINQ **Select** pour convertir des entités Book en DTO.

[!code-csharp[Main](part-5/samples/sample2.cs)]

Voici le SQL généré par la nouvelle méthode `GetBooks`. Vous pouvez voir que EF traduit la requête LINQ **Select** en une instruction SQL SELECT.

[!code-sql[Main](part-5/samples/sample3.sql)]

Enfin, modifiez la méthode `PostBook` pour retourner un DTO.

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> Dans ce didacticiel, nous effectuons une conversion manuelle en DTO dans le code. Une autre option consiste à utiliser une bibliothèque comme [AutoMapper](http://automapper.org/) qui gère automatiquement la conversion.
> 
> [!div class="step-by-step"]
> [Précédent](part-4.md)
> [Suivant](part-6.md)
