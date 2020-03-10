---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
title: Conventions de routage dans API Web ASP.NET 2 OData-ASP.NET 4. x
author: MikeWasson
description: Décrit les conventions de routage que l’API Web 2 dans ASP.NET 4. x utilise pour les points de terminaison OData.
ms.author: riande
ms.date: 07/31/2013
ms.custom: seoapril2019
ms.assetid: adbc175a-14eb-4ab2-a441-d056ffa8266f
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
msc.type: authoredcontent
ms.openlocfilehash: 63df4a82cd8df92631485b2544117844cfd0ca56
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614715"
---
# <a name="routing-conventions-in-aspnet-web-api-2-odata"></a>Conventions de routage dans API Web ASP.NET 2 OData

par [Mike Wasson](https://github.com/MikeWasson)

> Cet article décrit les conventions de routage que l’API Web 2 dans ASP.NET 4. x utilise pour les points de terminaison OData.

Lorsque l’API Web obtient une demande OData, elle mappe la demande à un nom de contrôleur et à un nom d’action. Le mappage est basé sur la méthode HTTP et l’URI. Par exemple, `GET /odata/Products(1)` est mappé à `ProductsController.GetProduct`.

Dans la première partie de cet article, je décrirai les conventions de routage OData intégrées. Ces conventions sont conçues spécifiquement pour les points de terminaison OData, et elles remplacent le système de routage d’API Web par défaut. (Le remplacement se produit lorsque vous appelez **MapODataRoute**.)

Dans la partie 2, je montre comment ajouter des conventions de routage personnalisées. Actuellement, les conventions intégrées ne couvrent pas la plage complète des URI OData, mais vous pouvez les étendre pour gérer des cas supplémentaires.

- [Conventions de routage intégrées](#conventions)
- [Conventions de routage personnalisées](#custom)

<a id="conventions"></a>
## <a name="built-in-routing-conventions"></a>Conventions de routage intégrées

Avant de décrire les conventions de routage OData dans l’API Web, il est utile de comprendre les URI OData. Un [URI OData](http://www.odata.org/documentation/odata-v3-documentation/url-conventions/) se compose des éléments suivants :

- Racine du service
- Le chemin d’accès de la ressource
- Options de requête

![](odata-routing-conventions/_static/image1.png)

Pour le routage, la partie importante est le chemin d’accès à la ressource. Le chemin d’accès de la ressource est divisé en segments. Par exemple, `/Products(1)/Supplier` a trois segments :

- `Products` fait référence à un jeu d’entités nommé « Products ».
- `1` est une clé d’entité, en sélectionnant une entité unique à partir de l’ensemble.
- `Supplier` est une propriété de navigation qui sélectionne une entité associée.

Ce chemin d’accès récupère donc le fournisseur du produit 1.

> [!NOTE]
> Les segments de chemin d’accès OData ne correspondent pas toujours aux segments d’URI. Par exemple, « 1 » est considéré comme un segment de chemin d’accès.

**Noms des contrôleurs.** Le nom du contrôleur est toujours dérivé du jeu d’entités à la racine du chemin d’accès de la ressource. Par exemple, si le chemin d’accès de la ressource est `/Products(1)/Supplier`, l’API Web recherche un contrôleur nommé `ProductsController`.

**Noms d’action.** Les noms d’action sont dérivés des segments de chemin d’accès et du modèle EDM (Entity Data Model), comme indiqué dans les tableaux suivants. Dans certains cas, vous avez le choix entre deux options pour le nom de l’action. Par exemple, « obtenir » ou &quot;GetProducts&quot;.

**Interrogation d’entités**

| Requête | Exemple d’URI | Nom de l'action | Exemple d’action |
| --- | --- | --- | --- |
| Obtient/EntitySet | /Products | GetEntitySet ou obtient | GetProducts |
| Obtient/EntitySet (clé) | /Products (1) | GetEntityType ou obtient | GetProduct |
| Obtient/EntitySet (clé)/Cast | /Products(1)/Models.Book | GetEntityType ou obtient | GetBook |

Pour plus d’informations, consultez [créer un point de terminaison OData en lecture seule](odata-v3/creating-an-odata-endpoint.md).

**Création, mise à jour et suppression d’entités**

| Requête | Exemple d’URI | Nom de l'action | Exemple d’action |
| --- | --- | --- | --- |
| POSTER/EntitySet | /Products | PostEntityType ou poster | PostProduct |
| PUT/EntitySet (clé) | /Products (1) | PutEntityType ou put | PutProduct |
| PUT/EntitySet (clé)/Cast | /Products(1)/Models.Book | PutEntityType ou put | PutBook |
| CORRECTIF/EntitySet (clé) | /Products (1) | PatchEntityType ou correctif | PatchProduct |
| PATCH/EntitySet (Key)/Cast | /Products(1)/Models.Book | PatchEntityType ou correctif | PatchBook |
| SUPPRIMER/EntitySet (clé) | /Products (1) | DeleteEntityType ou supprimer | DeleteProduct |
| SUPPRIMER/EntitySet (clé)/Cast | /Products(1)/Models.Book | DeleteEntityType ou supprimer | DeleteBook |

**Interrogation d’une propriété de navigation**

| Requête | Exemple d’URI | Nom de l'action | Exemple d’action |
| --- | --- | --- | --- |
| Obtient/EntitySet (clé)/navigation | /Products (1)/Supplier | GetNavigationFromEntityType ou GetNavigation | GetSupplierFromProduct |
| Obtient/EntitySet (clé)/Cast/navigation | /Products(1)/Models.Book/Author | GetNavigationFromEntityType ou GetNavigation | GetAuthorFromBook |

Pour plus d’informations, consultez [utilisation des relations d’entité](odata-v3/working-with-entity-relations.md).

**Création et suppression de liens**

| Requête | Exemple d’URI | Nom de l'action |
| --- | --- | --- |
| POSTER/EntitySet (clé)/$links/navigation | /Products (1)/$links/Supplier | CreateLink |
| PUT/EntitySet (clé)/$links/navigation | /Products (1)/$links/Supplier | CreateLink |
| SUPPRIMER/EntitySet (clé)/$links/navigation | /Products (1)/$links/Supplier | DeleteLink |
| DELETE /entityset(key)/$links/navigation(relatedKey) | /Products/(1)/$links/Suppliers (1) | DeleteLink |

Pour plus d’informations, consultez [utilisation des relations d’entité](odata-v3/working-with-entity-relations.md).

**Propriétés**

*Requiert Web API 2*

| Requête | Exemple d’URI | Nom de l'action | Exemple d’action |
| --- | --- | --- | --- |
| Obtient/EntitySet (clé)/Property | /Products(1)/Name | GetPropertyFromEntityType ou GetProperty | GetNameFromProduct |
| Obtient/EntitySet (clé)/Cast/Property | /Products(1)/Models.Book/Author | GetPropertyFromEntityType ou GetProperty | GetTitleFromBook |

**Actions**

| Requête | Exemple d’URI | Nom de l'action | Exemple d’action |
| --- | --- | --- | --- |
| POST /entityset(key)/action | /Products (1)/rate | ActionNameOnEntityType ou ActionName | RateOnProduct |
| /EntitySet de publication (clé)/Cast/action | /Products(1)/Models.Book/CheckOut | ActionNameOnEntityType ou ActionName | CheckOutOnBook |

Pour plus d’informations, consultez [actions OData](odata-v3/odata-actions.md).

**Signatures de méthode**

Voici quelques règles pour les signatures de méthode :

- Si le chemin d’accès contient une clé, l’action doit avoir un paramètre nommé *Key*.
- Si le chemin d’accès contient une clé dans une propriété de navigation, l’action doit avoir un paramètre nommé *relatedKey*.
- Décorez les paramètres *Key* et *relatedKey* avec le paramètre **[FromODataUri]** .
- Les demandes de publication et de placement acceptent un paramètre du type d’entité.
- Les demandes de correctif prennent un paramètre de type **Delta&lt;t&gt;** , où *t* est le type d’entité.

Pour référence, voici un exemple qui montre des signatures de méthode pour chaque convention de routage OData intégrée.

[!code-csharp[Main](odata-routing-conventions/samples/sample1.cs)]

<a id="custom"></a>
## <a name="custom-routing-conventions"></a>Conventions de routage personnalisées

Actuellement, les conventions intégrées ne couvrent pas tous les URI OData possibles. Vous pouvez ajouter de nouvelles conventions en implémentant l’interface **IODataRoutingConvention** . Cette interface a deux méthodes :

[!code-csharp[Main](odata-routing-conventions/samples/sample2.cs)]

- **SelectController** retourne le nom du contrôleur.
- **SelectAction** retourne le nom de l’action.

Pour les deux méthodes, si la Convention ne s’applique pas à cette demande, la méthode doit retourner la valeur null.

Le paramètre **ODataPath** représente le chemin d’accès de la ressource OData analysée. Il contient une liste d’instances **[segment](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatapathsegment.aspx)** , une pour chaque segment du chemin d’accès de la ressource. **Segment** est une classe abstraite ; chaque type de segment est représenté par une classe qui dérive de **segment**.

La propriété **ODataPath. TemplatePath** est une chaîne qui représente la concaténation de tous les segments de chemin d’accès. Par exemple, si l’URI est `/Products(1)/Supplier`, le modèle de chemin d’accès est &quot;~/EntitySet/Key/navigation&quot;. Notez que les segments ne correspondent pas directement aux segments d’URI. Par exemple, la clé d’entité (1) est représentée comme son propre **segment**.

En général, une implémentation de **IODataRoutingConvention** effectue les opérations suivantes :

1. Comparez le modèle de chemin d’accès pour voir si cette Convention s’applique à la requête actuelle. Si elle ne s’applique pas, retourne la valeur null.
2. Si la Convention s’applique, utilisez les propriétés des instances **segment** pour dériver les noms de contrôleur et d’action.
3. Pour les actions, ajoutez toutes les valeurs au dictionnaire de routage qui doivent être liées aux paramètres de l’action (en général, les clés d’entité).

Examinons un exemple spécifique. Les conventions de routage intégrées ne prennent pas en charge l’indexation dans une collection de navigation. En d’autres termes, il n’existe aucune convention pour les URI comme suit :

[!code-javascript[Main](odata-routing-conventions/samples/sample3.js)]

Voici une convention de routage personnalisée pour gérer ce type de requête.

[!code-csharp[Main](odata-routing-conventions/samples/sample4.cs)]

Remarques :

1. Je dérive de **EntitySetRoutingConvention**, car la méthode **SelectController** de cette classe est appropriée pour cette nouvelle Convention de routage. Cela signifie que je n’ai pas besoin d’implémenter à nouveau **SelectController**.
2. La Convention s’applique uniquement aux demandes d’extraction et uniquement lorsque le modèle de chemin d’accès est &quot;~/EntitySet/Key/navigation/Key&quot;.
3. Le nom de l’action est &quot;{EntityType}&quot;, où *{EntityType}* est le type de la collection de navigation. Par exemple, &quot;&quot;GetSupplier. Vous pouvez utiliser n’importe quelle convention d’affectation &#8212; de noms pour vous assurer que les actions du contrôleur correspondent.
4. L’action accepte deux paramètres nommés *Key* et *relatedKey*. (Pour obtenir la liste de certains noms de paramètres prédéfinis, consultez [ODataRouteConstants](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatarouteconstants.aspx).)

L’étape suivante consiste à ajouter la nouvelle Convention à la liste des conventions de routage. Cela se produit pendant la configuration, comme illustré dans le code suivant :

[!code-csharp[Main](odata-routing-conventions/samples/sample5.cs)]

Voici d’autres exemples de conventions de routage qui sont utiles pour étudier les éléments suivants :

- [CompositeKeyRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataCompositeKeySample/ODataCompositeKeySample/Extensions/CompositeKeyRoutingConvention.cs)
- [CustomNavigationRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataServiceSample/ODataService/Extensions/CustomNavigationRoutingConvention.cs)
- [NonBindableActionRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataActionsSample/ODataActionsSample/NonBindableActionRoutingConvention.cs)
- [ODataVersionRouteConstraint](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataVersioningSample/ODataVersioningSample/Extensions/ODataVersionRouteConstraint.cs)

Et bien évidemment, l’API Web elle-même est open-source, ce qui vous permet de voir le [code source](http://aspnetwebstack.codeplex.com/) des conventions de routage intégrées. Celles-ci sont définies dans l’espace de noms **System. Web. http. OData. Routing. conventions** .
