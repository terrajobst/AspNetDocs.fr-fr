---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: 'Partie 7 : création de la page principale | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: fe4074c701159a137be3644d65ca844f160c2399
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598678"
---
# <a name="part-7-creating-the-main-page"></a>Partie 7 : création de la page principale

par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a>Création de la page principale

Dans cette section, vous allez créer la page principale de l’application. Cette page est plus complexe que la page d’administration. nous allons donc l’aborder en plusieurs étapes. En cours de route, vous verrez des techniques plus avancées de Knockout. js. Voici la disposition de base de la page :

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- « Products » contient un tableau de produits.
- « Cart » contient un tableau de produits avec des quantités. Cliquez sur « Ajouter au panier » pour mettre à jour le panier.
- « Orders » contient un tableau d’ID de commande.
- « Détails » contient un détail de commande, qui est un tableau d’éléments (produits avec quantités)

Nous allons commencer par définir une mise en page de base en HTML, sans liaison de données ni script. Ouvrez le fichier views/orig/index. cshtml et remplacez tout le contenu par ce qui suit :

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

Ensuite, ajoutez une section scripts et créez un modèle d’affichage vide :

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

Selon la conception esquissée précédemment, notre modèle de vue a besoin de observables pour les produits, les paniers, les commandes et les détails. Ajoutez les variables suivantes à l’objet `AppViewModel` :

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

Les utilisateurs peuvent ajouter des éléments à partir de la liste des produits dans le panier et supprimer des éléments du panier. Pour encapsuler ces fonctions, nous allons créer une autre classe de modèle d’affichage qui représente un produit. Ajoutez le code suivant à `AppViewModel` :

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

La classe `ProductViewModel` contient deux fonctions qui permettent de déplacer le produit vers et à partir du panier : `addItemToCart` ajoute une unité du produit au panier et `removeAllFromCart` supprime toutes les quantités du produit.

Les utilisateurs peuvent sélectionner une commande existante et récupérer les détails de la commande. Nous encapsulerons cette fonctionnalité dans un autre modèle d’affichage :

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

La `OrderDetailsViewModel` est initialisée avec une commande et extrait les détails de la commande en envoyant une demande AJAX au serveur.

Notez également la propriété `total` sur le `OrderDetailsViewModel`. Cette propriété est un type spécial d’observable appelé [observable calculé](http://knockoutjs.com/documentation/computedObservables.html). Comme son nom l’indique, un observable calculé vous permet de lier des données à une valeur&#8212;calculée dans ce cas, le coût total de la commande.

Ensuite, ajoutez ces fonctions à `AppViewModel`:

- `resetCart` supprime tous les éléments du panier.
- `getDetails` obtient les détails d’une commande (en envoyant un nouvel `OrderDetailsViewModel` dans la liste `details`).
- `createOrder` crée une nouvelle commande et vide le panier.

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

Enfin, initialisez le modèle de vue en effectuant des demandes AJAX pour les produits et les commandes :

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

Bon, c’est beaucoup de code, mais nous l’avons créé pas à pas, donc j’espère que la conception est claire. Nous pouvons à présent ajouter des liaisons Knockout. js au code HTML.

**Produits**

Voici les liaisons de la liste de produits :

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

Cela permet d’effectuer une itération sur le tableau Products et d’afficher le nom et le prix. Le bouton « Ajouter à la commande » n’est visible que lorsque l’utilisateur a ouvert une session.

Le bouton « Ajouter à la commande » appelle `addItemToCart` sur l’instance `ProductViewModel` pour le produit. Cela démontre une fonctionnalité intéressante de Knockout. js : quand un modèle de vue contient d’autres modèles d’affichage, vous pouvez appliquer les liaisons au modèle interne. Dans cet exemple, les liaisons au sein du `foreach` sont appliquées à chacune des instances de `ProductViewModel`. Cette approche est bien plus propre que de placer toutes les fonctionnalités dans un modèle de vue unique.

**Caddie**

Voici les liaisons du panier :

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

Cela permet d’effectuer une itération sur le tableau Cart et d’afficher le nom, le prix et la quantité. Notez que le lien « supprimer » et le bouton « créer une commande » sont liés aux fonctions de modèle d’affichage.

**Commandes**

Voici les liaisons de la liste Orders :

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

Cela permet d’effectuer une itération sur les commandes et d’afficher l’ID de commande. L’événement Click sur le lien est lié à la fonction `getDetails`.

**Détails de la commande**

Voici les liaisons pour les détails de la commande :

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

Cela permet d’effectuer une itération sur les éléments dans l’ordre et d’afficher le produit, le prix et la quantité. La balise div environnante est visible uniquement si le tableau de détails contient un ou plusieurs éléments.

## <a name="conclusion"></a>Conclusion

Dans ce didacticiel, vous avez créé une application qui utilise Entity Framework pour communiquer avec la base de données, et API Web ASP.NET pour fournir une interface publique en plus de la couche de données. Nous utilisons ASP.NET MVC 4 pour afficher les pages HTML et Knockout. js plus jQuery pour fournir des interactions dynamiques sans rechargements de pages.

Ressources supplémentaires :

- [Plan de contenu d’accès aux données ASP.NET](https://msdn.microsoft.com/library/6759sth4.aspx)
- [Centre de développement Entity Framework](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [Précédent](using-web-api-with-entity-framework-part-6.md)
