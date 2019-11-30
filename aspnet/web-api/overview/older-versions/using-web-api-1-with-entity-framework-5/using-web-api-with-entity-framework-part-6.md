---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: 'Partie 6 : création de contrôleurs de produit et de commandes | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: e0bf88e3477acbde910cde956042449bc86ce79a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600027"
---
# <a name="part-6-creating-product-and-order-controllers"></a>Partie 6 : création de contrôleurs de produit et de commandes

par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a>Ajouter un contrôleur Products

Le contrôleur d’administration est destiné aux utilisateurs disposant de privilèges d’administrateur. En revanche, les clients peuvent afficher les produits, mais ils ne peuvent pas les créer, les mettre à jour ou les supprimer.

Nous pouvons facilement limiter l’accès aux méthodes de publication, put et Delete, tout en laissant les méthodes d’extraction ouvertes. Examinez les données retournées pour un produit :

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

La propriété `ActualCost` ne doit pas être visible pour les clients. La solution consiste à définir un *objet de transfert de données* (DTO) qui comprend un sous-ensemble de propriétés qui doivent être visibles par les clients. Nous allons utiliser LINQ pour projeter des instances de `Product` pour `ProductDTO` instances.

Ajoutez une classe nommée `ProductDTO` au dossier Models.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

Ajoutez maintenant le contrôleur. Dans Explorateur de solutions, cliquez avec le bouton droit sur le dossier Controllers. Sélectionnez **Ajouter**, puis **contrôleur**. Dans la boîte de dialogue **Ajouter un contrôleur** , nommez le contrôleur &quot;&quot;ProductsController. Sous **modèle**, sélectionnez **contrôleur d’API vide**.

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

Remplacez tout ce qui se trouve dans le fichier source par le code suivant :

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

Le contrôleur utilise toujours le `OrdersContext` pour interroger la base de données. Mais au lieu de retourner directement des instances de `Product`, nous appelons `MapProducts` pour les projeter sur `ProductDTO` instances :

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

La méthode `MapProducts` retourne un **IQueryable**, donc nous pouvons composer le résultat avec d’autres paramètres de requête. Vous pouvez le voir dans la méthode `GetProduct`, qui ajoute une clause **Where** à la requête :

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a>Ajouter un contrôleur de commandes

Ensuite, ajoutez un contrôleur qui permet aux utilisateurs de créer et d’afficher des commandes.

Nous allons commencer par un autre DTO. Dans Explorateur de solutions, cliquez avec le bouton droit sur le dossier Models et ajoutez une classe nommée `OrderDTO` utilisez l’implémentation suivante :

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

Ajoutez maintenant le contrôleur. Dans Explorateur de solutions, cliquez avec le bouton droit sur le dossier Controllers. Sélectionnez **Ajouter**, puis **contrôleur**. Dans la boîte de dialogue **Ajouter un contrôleur** , définissez les options suivantes :

- Sous **nom du contrôleur**, entrez « OrdersController ».
- Sous **modèle**, sélectionnez contrôleur d’API avec actions en lecture/écriture, à l’aide de Entity Framework».
- Sous **classe de modèle**, sélectionnez &quot;commande (ProductStore. Models)&quot;.
- Sous **classe de contexte de données**, sélectionnez &quot;OrdersContext (ProductStore. Models)&quot;.

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

Cliquez sur **Ajouter**. Cela ajoute un fichier nommé OrdersController.cs. Ensuite, nous devons modifier l’implémentation par défaut du contrôleur.

Tout d’abord, supprimez les méthodes `PutOrder` et `DeleteOrder`. Pour cet exemple, les clients ne peuvent pas modifier ou supprimer des commandes existantes. Dans une application réelle, vous auriez besoin d’un grand nombre de logiques principales pour gérer ces cas. (Par exemple, la commande a-t-elle déjà été expédiée ?)

Modifiez la méthode `GetOrders` pour retourner uniquement les commandes qui appartiennent à l’utilisateur :

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

Modifiez la méthode `GetOrder` comme suit :

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

Voici les modifications que nous avons apportées à la méthode :

- La valeur de retour est une instance de `OrderDTO`, au lieu d’une `Order`.
- Lors de l’interrogation de la base de données pour la commande, nous utilisons la méthode [DbQuery. include](https://msdn.microsoft.com/library/gg696395) pour extraire les entités `OrderDetail` et `Product` associées.
- Nous aplatirons le résultat à l’aide d’une projection.

La réponse HTTP contient un tableau de produits dont les quantités sont :

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

Ce format est plus facile à utiliser par les clients que le graphique d’objets d’origine, qui contient des entités imbriquées (Order, Details et Products).

Dernière méthode à prendre en compte `PostOrder`. Pour le moment, cette méthode prend une instance de `Order`. Mais réfléchissez à ce qui se passe si un client envoie un corps de requête comme suit :

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

Il s’agit d’un ordre bien structuré et Entity Framework l’insère dans la base de données. Mais il contient une entité Product qui n’existait pas précédemment. Le client vient de créer un nouveau produit dans notre base de données. Ce sera une surprise pour le service Order envois, lorsqu’ils verront une commande pour Koala. Le moral est, soyez très prudent quant aux données que vous acceptez dans une demande de publication ou de placement.

Pour éviter ce problème, modifiez la méthode `PostOrder` pour prendre une `OrderDTO` instance. Utilisez la `OrderDTO` pour créer l' `Order`.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

Notez que nous utilisons les propriétés `ProductID` et `Quantity`, et que nous ignorons toutes les valeurs que le client a envoyées pour le nom ou le prix du produit. Si l’ID de produit n’est pas valide, il violera la contrainte de clé étrangère dans la base de données, et l’insertion échouera, comme c’est le cas.

Voici la méthode `PostOrder` complète :

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

Enfin, ajoutez l’attribut **Authorize** au contrôleur :

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

Désormais, seuls les utilisateurs inscrits peuvent créer ou afficher des commandes.

> [!div class="step-by-step"]
> [Précédent](using-web-api-with-entity-framework-part-5.md)
> [Suivant](using-web-api-with-entity-framework-part-7.md)
