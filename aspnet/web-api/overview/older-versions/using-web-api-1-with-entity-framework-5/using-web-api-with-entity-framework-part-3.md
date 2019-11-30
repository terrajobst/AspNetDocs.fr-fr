---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'Partie 3 : création d’un contrôleur d’administration | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: f39be7a84e85db93487d246e9f8cb59c401fe5ce
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600044"
---
# <a name="part-3-creating-an-admin-controller"></a>Partie 3 : création d’un contrôleur d’administration

par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a>Ajouter un contrôleur d’administration

Dans cette section, nous allons ajouter un contrôleur d’API Web qui prend en charge les opérations CRUD (création, lecture, mise à jour et suppression) sur les produits. Le contrôleur utilise Entity Framework pour communiquer avec la couche de base de données. Seuls les administrateurs seront en mesure d’utiliser ce contrôleur. Les clients accèderont aux produits par le biais d’un autre contrôleur.

Dans Explorateur de solutions, cliquez avec le bouton droit sur le dossier Controllers. Sélectionnez **Ajouter** , puis **contrôleur**.

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

Dans la boîte de dialogue **Ajouter un contrôleur** , nommez le contrôleur `AdminController`. Sous **modèle**, sélectionnez &quot;contrôleur d’API avec des actions de lecture/écriture, à l’aide de Entity Framework&quot;. Sous **classe de modèle**, sélectionnez « produit (ProductStore. Models) ». Sous **contexte de données**, sélectionnez «&lt;nouveau contexte de données&gt;».

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> Si la liste déroulante **classe de modèle** n’affiche pas de classes de modèle, assurez-vous que vous avez compilé le projet. Entity Framework utilise la réflexion, il a besoin de l’assembly compilé.

Si vous sélectionnez «&lt;nouveau contexte de données&gt;», la boîte de dialogue **nouveau contexte de données** s’ouvre. Nommez le contexte de données `ProductStore.Models.OrdersContext`.

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

Cliquez sur **OK** pour fermer la boîte de dialogue **nouveau contexte de données** . Dans la boîte de dialogue **Ajouter un contrôleur** , cliquez sur **Ajouter**.

Voici ce qui a été ajouté au projet :

- Classe nommée `OrdersContext` qui dérive de **DbContext**. Cette classe fournit le collage entre les modèles POCO et la base de données.
- Un contrôleur d’API Web nommé `AdminController`. Ce contrôleur prend en charge les opérations CRUD sur les instances de `Product`. Elle utilise la classe `OrdersContext` pour communiquer avec Entity Framework.
- Nouvelle chaîne de connexion de base de données dans le fichier Web. config.

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

Ouvrez le fichier OrdersContext.cs. Notez que le constructeur spécifie le nom de la chaîne de connexion à la base de données. Ce nom fait référence à la chaîne de connexion qui a été ajoutée à Web. config.

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

Ajoutez les propriétés suivantes à la classe `OrdersContext` :

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

Un **DbSet** représente un ensemble d’entités qui peuvent être interrogées. Voici la liste complète de la classe `OrdersContext` :

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

La classe `AdminController` définit cinq méthodes qui implémentent la fonctionnalité CRUD de base. Chaque méthode correspond à un URI que le client peut appeler :

| Méthode du contrôleur | Description | URI | Méthode HTTP |
| --- | --- | --- | --- |
| GetProducts | Obtient tous les produits. | API/produits | GET |
| GetProduct | Recherche un produit par ID. | API/produits/*ID* | GET |
| PutProduct | Met à jour un produit. | API/produits/*ID* | PUT |
| PostProduct | Crée un nouveau produit. | API/produits | Publier |
| DeleteProduct | Supprime un produit. | API/produits/*ID* | SUPPR |

Chaque méthode appelle `OrdersContext` pour interroger la base de données. Les méthodes qui modifient la collection (PUT, poster et DELETE) appellent `db.SaveChanges` pour conserver les modifications apportées à la base de données. Les contrôleurs sont créés par requête HTTP, puis supprimés. il est donc nécessaire de conserver les modifications avant le retour d’une méthode.

## <a name="add-a-database-initializer"></a>Ajouter un initialiseur de base de données

Entity Framework dispose d’une fonctionnalité intéressante qui vous permet de remplir la base de données au démarrage et de recréer automatiquement la base de données chaque fois que les modèles changent. Cette fonctionnalité est utile lors du développement, car vous avez toujours des données de test, même si vous modifiez les modèles.

Dans Explorateur de solutions, cliquez avec le bouton droit sur le dossier Models et créez une nouvelle classe nommée `OrdersContextInitializer`. Collez l’implémentation suivante :

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

En héritant de la classe **DropCreateDatabaseIfModelChanges** , nous indiquons Entity Framework de supprimer la base de données chaque fois que nous modifions les classes de modèle. Lorsque Entity Framework crée (ou recrée) la base de données, il appelle la méthode **Seed** pour remplir les tables. Nous utilisons la méthode **Seed** pour ajouter des exemples de produits, ainsi qu’un exemple d’ordre.

Cette fonctionnalité est très utile pour les tests, mais n’utilise pas la classe **DropCreateDatabaseIfModelChanges** en production, car vous risquez de perdre vos données si quelqu’un modifie une classe de modèle.

Ensuite, ouvrez global. asax et ajoutez le code suivant à l' **Application\_méthode Start** :

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a>Envoyer une demande au contrôleur

À ce stade, nous n’avons pas écrit de code client, mais vous pouvez appeler l’API Web à l’aide d’un navigateur Web ou d’un outil de débogage HTTP tel que [Fiddler](http://www.fiddler2.com/fiddler2/). Dans Visual Studio, appuyez sur F5 pour démarrer le débogage. Votre navigateur Web s’ouvre sur `http://localhost:*portnum*/`, où *portnum* est un numéro de port.

Envoyer une requête HTTP à «`http://localhost:*portnum*/api/admin`. La première requête peut être lente à se terminer, car Entity Framework doit créer et amorcer la base de données. La réponse doit ressembler à ce qui suit :

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> [Précédent](using-web-api-with-entity-framework-part-2.md)
> [Suivant](using-web-api-with-entity-framework-part-4.md)
