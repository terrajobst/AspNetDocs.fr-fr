---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: Activation des opérations CRUD dans API Web ASP.NET 1-ASP.NET 4. x
author: MikeWasson
description: Le didacticiel montre comment prendre en charge les opérations CRUD dans un service HTTP à l’aide de API Web ASP.NET pour ASP.NET 4. x.
ms.author: riande
ms.date: 01/28/2012
ms.custom: seoapril2019
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: a096fd1c54df33b40115907a5c2517b2e3fec5b8
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600339"
---
# <a name="enabling-crud-operations-in-aspnet-web-api-1"></a>Activation des opérations CRUD dans API Web ASP.NET 1

par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](https://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> Ce didacticiel montre comment prendre en charge les opérations CRUD dans un service HTTP à l’aide de API Web ASP.NET pour ASP.NET 4. x.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
> 
> 
> - Visual Studio 2012
> - API Web 1 (fonctionne également avec l’API Web 2)

CRUD est l’abréviation de &quot;Create, Read, Update et Delete,&quot; qui sont les quatre opérations de base de données. De nombreux services HTTP modélisent également les opérations CRUD via des API REST ou de type REST.

Dans ce didacticiel, vous allez créer une API Web très simple pour gérer une liste de produits. Chaque produit contient un nom, un prix et une catégorie (par exemple, &quot;Toys&quot; ou &quot;matériel&quot;), ainsi qu’un ID de produit.

L’API Products expose les méthodes suivantes.

| Action | Méthode HTTP | URI relatif |
| --- | --- | --- |
| Obtenir la liste de tous les produits | GET | /api/products |
| Obtenir un produit par ID | GET | *ID* /API/Products/ |
| Obtenir un produit par catégorie | GET | /API/products ? category =*catégorie* |
| Créer un produit | Publier | /api/products |
| Mettre à jour un produit | PUT | *ID* /API/Products/ |
| Supprimer un produit | SUPPR | *ID* /API/Products/ |

Notez que certains des URI incluent l’ID de produit dans path. Par exemple, pour obtenir le produit dont l’ID est 28, le client envoie une demande d’extraction pour `http://hostname/api/products/28`.

### <a name="resources"></a>Ressources

L’API Products définit des URI pour deux types de ressources :

| Ressource | URI |
| --- | --- |
| Liste de tous les produits. | /api/products |
| Un produit individuel. | *ID* /API/Products/ |

### <a name="methods"></a>Méthodes

Les quatre méthodes HTTP principales (obtient, PUT, poster et supprimer) peuvent être mappées aux opérations CRUD comme suit :

- OBTENIR récupère la représentation de la ressource à l’URI spécifié. L’extraction ne doit pas avoir d’effets secondaires sur le serveur.
- PUT met à jour une ressource à l’URI spécifié. PUT peut également être utilisé pour créer une ressource à un URI spécifié, si le serveur permet aux clients de spécifier de nouveaux URI. Pour ce didacticiel, l’API ne prend pas en charge la création via PUT.
- La publication crée une ressource. Le serveur assigne l’URI pour le nouvel objet et retourne cet URI dans le cadre du message de réponse.
- Supprimer supprime une ressource à l’URI spécifié.

Remarque : la méthode PUT remplace l’ensemble de l’entité Product. Autrement dit, le client est censé envoyer une représentation complète du produit mis à jour. Si vous souhaitez prendre en charge des mises à jour partielles, il est préférable d’utiliser la méthode PATCH. Ce didacticiel n’implémente pas les CORRECTIFs.

## <a name="create-a-new-web-api-project"></a>Créer un projet d’API Web

Commencez par exécuter Visual Studio et sélectionnez **nouveau projet** dans la page de **démarrage** . Ou, dans le menu **fichier** , sélectionnez **nouveau** , puis **projet**.

Dans le volet **modèles** , sélectionnez **modèles installés** , puis développez le nœud **visuel C#**  . Sous **visuel C#** , sélectionnez **Web**. Dans la liste des modèles de projet, sélectionnez **application Web ASP.NET MVC 4**. Nommez le projet &quot;&quot; ProductStore, puis cliquez sur **OK**.

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

Dans la boîte de dialogue **nouveau projet ASP.NET MVC 4** , sélectionnez **API Web** , puis cliquez sur **OK**.

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a>Ajout d’un modèle

Un *modèle* est un objet qui représente les données dans votre application. Dans API Web ASP.NET, vous pouvez utiliser des objets CLR fortement typés en tant que modèles, et ils seront automatiquement sérialisés en XML ou JSON pour le client.

Pour l’API ProductStore, nos données sont constituées de produits. nous allons donc créer une nouvelle classe nommée `Product`.

Si Explorateur de solutions n’est pas déjà visible, cliquez sur le menu **affichage** et sélectionnez **Explorateur de solutions**. Dans Explorateur de solutions, cliquez avec le bouton droit sur le dossier **modèles** . Dans le menu contextuel, sélectionnez **Ajouter**, puis **classe**. Nommez la classe &quot;&quot;de produit.

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

Ajoutez les propriétés suivantes à la classe `Product`.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a>Ajout d'une base de données de référentiel

Nous devons stocker une collection de produits. Il est judicieux de séparer la collection de notre implémentation de service. De cette façon, nous pouvons modifier le magasin de stockage sans réécrire la classe de service. Ce type de conception est appelé modèle de *référentiel* . Commencez par définir une interface générique pour le référentiel.

Dans Explorateur de solutions, cliquez avec le bouton droit sur le dossier **modèles** . Sélectionnez **Ajouter**, puis **nouvel élément**.

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

Dans le volet **modèles** , sélectionnez **modèles installés** , puis développez le C# nœud. Sous C#, sélectionnez **code**. Dans la liste des modèles de code, sélectionnez **interface**. Nommez l’interface &quot;&quot;IProductRepository.

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

Ajoutez l’implémentation suivante :

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

À présent, ajoutez une autre classe au dossier Models, nommé &quot;ProductRepository.&quot; cette classe implémentera l’interface `IProductRepository`. Ajoutez l’implémentation suivante :

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

Le référentiel conserve la liste dans la mémoire locale. Cela est correct pour un didacticiel, mais dans une application réelle, vous stockez les données en externe, soit une base de données, soit dans un stockage cloud. Le modèle de référentiel facilite la modification ultérieure de l’implémentation.

## <a name="adding-a-web-api-controller"></a>Ajout d’un contrôleur d’API Web

Si vous avez travaillé avec ASP.NET MVC, vous êtes déjà familiarisé avec les contrôleurs. Dans API Web ASP.NET, un *contrôleur* est une classe qui gère les requêtes http provenant du client. L’Assistant Nouveau projet a créé deux contrôleurs pour vous lors de la création du projet. Pour les afficher, développez le dossier Controllers dans Explorateur de solutions.

- HomeController est un contrôleur MVC ASP.NET traditionnel. Il est responsable de la fourniture des pages HTML pour le site et n’est pas directement lié à notre API Web.
- ValuesController est un exemple de contrôleur WebAPI.

Continuez et supprimez ValuesController, en cliquant avec le bouton droit sur le fichier dans Explorateur de solutions et en sélectionnant **Supprimer.** Ajoutez maintenant un nouveau contrôleur, comme suit :

Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier Controllers. Sélectionnez **Ajouter** , puis **contrôleur**.

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

Dans l’Assistant Ajout d’un **contrôleur** , nommez le contrôleur &quot;&quot;ProductsController. Dans la liste déroulante **modèle** , sélectionnez **contrôleur d’API vide**. Cliquez ensuite sur **Ajouter**.

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> Il n’est pas nécessaire de placer vos contrôleurs dans un dossier nommé contrôleurs. Le nom du dossier n’est pas important. Il s’agit simplement d’un moyen pratique d’organiser vos fichiers sources.

L’Assistant Ajout d’un **contrôleur** crée un fichier nommé ProductsController.cs dans le dossier Controllers. Si ce fichier n’est pas déjà ouvert, double-cliquez sur le fichier pour l’ouvrir. Ajoutez l’instruction **using** suivante :

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

Ajoutez un champ qui contient une instance de **IProductRepository** .

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> L’appel de `new ProductRepository()` dans le contrôleur n’est pas la meilleure conception, car il associe le contrôleur à une implémentation particulière de `IProductRepository`. Pour une meilleure approche, consultez [utilisation du programme de résolution des dépendances de l’API Web](../advanced/dependency-injection.md).

## <a name="getting-a-resource"></a>Obtention d’une ressource

L’API ProductStore expose plusieurs &quot;actions de lecture&quot; en tant que méthodes HTTP d’extraction. Chaque action correspond à une méthode dans la classe `ProductsController`.

| Action | Méthode HTTP | URI relatif |
| --- | --- | --- |
| Obtenir la liste de tous les produits | GET | /api/products |
| Obtenir un produit par ID | GET | *ID* /API/Products/ |
| Obtenir un produit par catégorie | GET | /API/products ? category =*catégorie* |

Pour afficher la liste de tous les produits, ajoutez cette méthode à la classe `ProductsController` :

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

Le nom de la méthode commence par &quot;obtient&quot;, donc par convention il est mappé aux demandes d’extraction. En outre, étant donné que la méthode n’a aucun paramètre, elle est mappée à un URI qui ne contient pas d' *ID de&quot;&quot;* segment dans le chemin d’accès.

Pour obtenir un produit par ID, ajoutez cette méthode à la classe `ProductsController` :

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

Ce nom de méthode commence également par &quot;obtenir&quot;, mais la méthode a un paramètre nommé *ID*. Ce paramètre est mappé à l’ID de &quot;&quot; segment du chemin d’accès de l’URI. L’infrastructure de API Web ASP.NET convertit automatiquement l’ID en type de données correct (**int**) pour le paramètre.

La méthode GetProduct lève une exception de type **HttpResponseException** si l' *ID* n’est pas valide. Cette exception sera traduite par l’infrastructure en erreur 404 (introuvable).

Enfin, ajoutez une méthode pour rechercher les produits par catégorie :

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

Si l’URI de la demande a une chaîne de requête, l’API Web tente de faire correspondre les paramètres de requête aux paramètres de la méthode du contrôleur. Par conséquent, un URI de la forme « API/products ? category =*Category*» est mappé à cette méthode.

## <a name="creating-a-resource"></a>Création d’une ressource

Ensuite, nous allons ajouter une méthode à la classe `ProductsController` pour créer un nouveau produit. Voici une implémentation simple de la méthode :

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

Notez deux choses à propos de cette méthode :

- Le nom de la méthode commence par &quot;billet...&quot;. Pour créer un nouveau produit, le client envoie une requête HTTP après.
- La méthode prend un paramètre de type Product. Dans l’API Web, les paramètres avec des types complexes sont désérialisés à partir du corps de la demande. Par conséquent, nous pensons que le client envoie une représentation sérialisée d’un objet de produit au format XML ou JSON.

Cette implémentation fonctionnera, mais elle n’est pas tout à fait complète. Dans l’idéal, nous aimerions que la réponse HTTP inclue les éléments suivants :

- **Code de réponse :** Par défaut, l’infrastructure d’API Web affecte au code d’état de réponse la valeur 200 (OK). Toutefois, selon le protocole HTTP/1.1, lorsqu’une requête de publication entraîne la création d’une ressource, le serveur doit répondre avec l’État 201 (créé).
- **Emplacement :** Lorsque le serveur crée une ressource, il doit inclure l’URI de la nouvelle ressource dans l’en-tête Location de la réponse.

API Web ASP.NET permet de manipuler facilement le message de réponse HTTP. Voici l’implémentation améliorée :

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

Notez que le type de retour de la méthode est maintenant **HttpResponseMessage**. En retournant un **HttpResponseMessage** au lieu d’un produit, nous pouvons contrôler les détails du message de réponse http, notamment le code d’État et l’en-tête d’emplacement.

La méthode **CreateResponse** crée un **HttpResponseMessage** et écrit automatiquement une représentation sérialisée de l’objet Product dans le corps du message de réponse.

> [!NOTE]
> Cet exemple ne valide pas la `Product`. Pour plus d’informations sur la validation de modèle, consultez [validation de modèle dans API Web ASP.net](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).

## <a name="updating-a-resource"></a>Mise à jour d’une ressource

La mise à jour d’un produit avec PUT est simple :

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

Le nom de la méthode commence par &quot;put...&quot;, de sorte que l’API Web la met en correspondance pour placer les demandes. La méthode accepte deux paramètres, l’ID de produit et le produit mis à jour. Le paramètre *ID* est extrait du chemin d’accès à l’URI et le paramètre *Product* est désérialisé à partir du corps de la demande. Par défaut, le API Web ASP.NET Framework prend des types de paramètres simples de l’itinéraire et des types complexes du corps de la demande.

## <a name="deleting-a-resource"></a>Suppression d’une ressource

Pour supprimer une ressource, définissez une « suppression... » méthode.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

Si une demande de suppression est réussie, elle peut retourner l’état 200 (OK) avec un corps d’entité qui décrit l’État. État 202 (accepté) si la suppression est toujours en attente ; ou état 204 (aucun contenu) sans corps d’entité. Dans ce cas, la méthode `DeleteProduct` a un type de retour `void`, donc API Web ASP.NET le convertit automatiquement en code d’État 204 (aucun contenu).
