---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: Appel d’un service OData à partir d’unC#client .net () | Microsoft Docs
author: MikeWasson
description: Ce didacticiel montre comment appeler un service OData à partir d' C# une application cliente. Versions logicielles utilisées dans le didacticiel Visual Studio 2013 (fonctionne avec les éléments visuels...
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 6a289fcb843634eeeefef1e0767e04e0be8b6973
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614680"
---
# <a name="calling-an-odata-service-from-a-net-client-c"></a>Appel à un service OData à partir d’un client .NET (C#)

par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Ce didacticiel montre comment appeler un service OData à partir d' C# une application cliente.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) (fonctionne avec Visual Studio 2012)
> - [Bibliothèque cliente WCF Data Services](https://msdn.microsoft.com/library/cc668772.aspx)
> - API Web 2. (L’exemple de service OData est généré à l’aide de l’API Web 2, mais l’application cliente ne dépend pas de l’API Web.)

Dans ce didacticiel, je vais vous guider tout au long de la création d’une application cliente qui appelle un service OData. Le service OData expose les entités suivantes :

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

Les articles suivants expliquent comment implémenter le service OData dans l’API Web. (Toutefois, vous n’avez pas besoin de les lire pour comprendre ce didacticiel.)

- [Création d’un point de terminaison OData dans Web API 2](creating-an-odata-endpoint.md)
- [Relations d’entité OData dans l’API Web 2](working-with-entity-relations.md)
- [Actions OData dans Web API 2](odata-actions.md)

## <a name="generate-the-service-proxy"></a>Générer le proxy de service

La première étape consiste à générer un proxy de service. Le proxy de service est une classe .NET qui définit des méthodes pour accéder au service OData. Le proxy convertit les appels de méthode en requêtes HTTP.

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

Commencez par ouvrir le projet de service OData dans Visual Studio. Appuyez sur CTRL + F5 pour exécuter le service localement dans IIS Express. Notez l’adresse locale, y compris le numéro de port que Visual Studio affecte. Vous aurez besoin de cette adresse lorsque vous créerez le proxy.

Ensuite, ouvrez une autre instance de Visual Studio et créez un projet d’application console. L’application console sera notre application cliente OData. (Vous pouvez également ajouter le projet à la même solution que le service.)

> [!NOTE]
> Les étapes restantes font référence au projet console.

Dans Explorateur de solutions, cliquez avec le bouton droit sur **références** et sélectionnez **Ajouter une référence de service**.

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

Dans la boîte de dialogue **Ajouter une référence de service** , tapez l’adresse du service OData :

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

où *port* est le numéro de port.

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

Pour **espace de noms**, tapez « ProductService ». Cette option définit l’espace de noms de la classe proxy.

Cliquez sur **OK**. Visual Studio lit le document de métadonnées OData pour découvrir les entités dans le service.

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

Cliquez sur **OK** pour ajouter la classe proxy à votre projet.

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a>Créer une instance de la classe de proxy de service

Au sein de votre méthode `Main`, créez une nouvelle instance de la classe proxy comme suit :

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

Là encore, utilisez le numéro de port réel dans lequel votre service est en cours d’exécution. Lorsque vous déployez votre service, vous allez utiliser l’URI du service actif. Vous n’avez pas besoin de mettre à jour le proxy.

Le code suivant ajoute un gestionnaire d’événements qui imprime les URI de demande dans la fenêtre de console. Cette étape n’est pas obligatoire, mais il est intéressant de voir les URI pour chaque requête.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a>Interroger le service

Le code suivant obtient la liste des produits à partir du service OData.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

Notez que vous n’avez pas besoin d’écrire du code pour envoyer la requête HTTP ou analyser la réponse. La classe proxy procède automatiquement à l’énumération de la collection `Container.Products` dans la boucle **foreach** .

Lorsque vous exécutez l’application, la sortie doit ressembler à ce qui suit :

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

Pour obtenir une entité par ID, utilisez une clause `where`.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

Pour le reste de cette rubrique, je n’affiche pas l’intégralité de la fonction `Main`, juste le code nécessaire pour appeler le service.

## <a name="apply-query-options"></a>Appliquer les options de requête

OData définit des [options de requête](../supporting-odata-query-options.md) qui peuvent être utilisées pour filtrer, trier, parcourir les données, etc. Dans le proxy de service, vous pouvez appliquer ces options à l’aide de différentes expressions LINQ.

Dans cette section, je vais vous montrer des exemples succincts. Pour plus d’informations, consultez la rubrique [considérations relatives à LINQ (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) sur MSDN.

### <a name="filtering-filter"></a>Filtrage ($filter)

Pour filtrer, utilisez une clause `where`. L’exemple suivant filtre par catégorie de produit.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

Ce code correspond à la requête OData suivante.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

Notez que le proxy convertit la clause `where` en expression OData `$filter`.

### <a name="sorting-orderby"></a>Tri ($orderby)

Pour trier, utilisez une clause `orderby`. L’exemple suivant trie par prix, du plus élevé au plus bas.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

Voici la requête OData correspondante.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a>Pagination côté client ($skip et $top)

Pour les jeux d’entités volumineux, le client peut vouloir limiter le nombre de résultats. Par exemple, un client peut afficher 10 entrées à la fois. C’est ce que l’on appelle la *pagination côté client*. (Il existe également [une pagination côté serveur](../supporting-odata-query-options.md#server-paging), où le serveur limite le nombre de résultats.) Pour effectuer la pagination côté client, utilisez les méthodes LINQ **Skip** et **Take** . L’exemple suivant ignore les 40 premiers résultats et prend le 10 suivant.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

Voici la requête OData correspondante :

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a>Sélectionnez ($select) et développez ($expand)

Pour inclure des entités connexes, utilisez la méthode `DataServiceQuery<t>.Expand`. Par exemple, pour inclure le `Supplier` pour chaque `Product`:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

Voici la requête OData correspondante :

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

Pour modifier la forme de la réponse, utilisez la clause LINQ **Select** . L’exemple suivant obtient uniquement le nom de chaque produit, sans aucune autre propriété.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

Voici la requête OData correspondante :

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

Une clause SELECT peut inclure des entités associées. Dans ce cas, n’appelez pas **expand**; le proxy comprend automatiquement l’extension dans ce cas. L’exemple suivant obtient le nom et le fournisseur de chaque produit.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

Voici la requête OData correspondante. Notez qu’elle comprend l’option **$expand** .

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

Pour plus d’informations sur les $select et les $expand, consultez [utilisation de $Select, $Expand et $value dans Web API 2](../using-select-expand-and-value.md).

## <a name="add-a-new-entity"></a>Ajouter une nouvelle entité

Pour ajouter une nouvelle entité à un jeu d’entités, appelez `AddToEntitySet`, où *EntitySet* est le nom du jeu d’entités. Par exemple, `AddToProducts` ajoute une nouvelle `Product` au jeu d’entités `Products`. Lorsque vous générez le proxy, WCF Data Services crée automatiquement ces méthodes **AddTo** fortement typées.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

Pour ajouter un lien entre deux entités, utilisez les méthodes **AddLink** et **SetLink** . Le code suivant ajoute un nouveau fournisseur et un nouveau produit, puis crée des liens entre eux.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

Utilisez **AddLink** lorsque la propriété de navigation est une collection. Dans cet exemple, nous ajoutons un produit au regroupement `Products` sur le fournisseur.

Utilisez **SetLink** lorsque la propriété de navigation est une entité unique. Dans cet exemple, nous définissons la propriété `Supplier` sur le produit.

## <a name="update--patch"></a>Mise à jour/correctif

Pour mettre à jour une entité, appelez la méthode **UpdateObject** .

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

La mise à jour est effectuée lorsque vous appelez **SaveChanges**. Par défaut, WCF envoie une demande de fusion HTTP. L’option **PatchOnUpdate** indique à WCF d’envoyer un correctif http à la place.

> [!NOTE]
> Pourquoi corriger et FUSIONNer ? La spécification HTTP 1,1 d’origine ([RCF 2616](http://tools.ietf.org/html/rfc2616)) n’a pas défini de méthode http avec une sémantique de « mise à jour partielle ». Pour prendre en charge les mises à jour partielles, la spécification OData définit la méthode MERGE. Dans 2010, [RFC 5789](http://tools.ietf.org/html/rfc5789) a défini la méthode patch pour les mises à jour partielles. Vous pouvez lire certains de l’historique dans ce billet de [blog](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) sur le blog de WCF Data Services. Aujourd’hui, le correctif est préféré à la fusion. Le contrôleur OData créé par la génération de modèles automatique d’API Web prend en charge les deux méthodes.

Si vous souhaitez remplacer l’ensemble de l’entité (sémantique PUT), spécifiez l’option **ReplaceOnUpdate** . WCF envoie alors une requête HTTP PUT.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a>Supprimer une entité

Pour supprimer une entité, appelez **SupprimerObjet**.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a>Appeler une action OData

Dans OData, les [actions](odata-actions.md) sont un moyen d’ajouter des comportements côté serveur qui ne sont pas facilement définis en tant qu’opérations CRUD sur les entités.

Bien que le document de métadonnées OData décrive les actions, la classe proxy ne crée pas de méthodes fortement typées pour elles. Vous pouvez toujours appeler une action OData à l’aide de la méthode **Execute** générique. Toutefois, vous devez connaître les types de données des paramètres et la valeur de retour.

Par exemple, l’action `RateProduct` prend le paramètre nommé « Rating » de type `Int32` et retourne un `double`. Le code suivant montre comment appeler cette action.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

Pour plus d’informations, consultez[appel des opérations et actions de service](https://msdn.microsoft.com/library/hh230677.aspx).

L’une des options consiste à étendre la classe de **conteneur** pour fournir une méthode fortement typée qui appelle l’action :

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
