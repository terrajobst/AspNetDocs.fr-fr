---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Routage des attributs dans API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 7da1805d8a7066e82743dc9bd7e024cc9813ee89
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554984"
---
# <a name="attribute-routing-in-aspnet-web-api-2"></a>Routage des attributs dans API Web ASP.NET 2

par [Mike Wasson](https://github.com/MikeWasson)

Le *routage* correspond à la façon dont l’API Web correspond à un URI à une action. L’API Web 2 prend en charge un nouveau type de routage, appelé *routage d’attribut*. Comme son nom l’indique, le routage d’attributs utilise des attributs pour définir des itinéraires. Le routage des attributs vous donne davantage de contrôle sur les URI de votre API Web. Par exemple, vous pouvez facilement créer des URI qui décrivent des hiérarchies de ressources.

Le style de routage précédent, appelé routage basé sur des conventions, est toujours entièrement pris en charge. En fait, vous pouvez combiner les deux techniques dans le même projet.

Cette rubrique montre comment activer le routage d’attributs et décrit les différentes options de routage des attributs. Pour obtenir un didacticiel de bout en bout qui utilise le routage d’attributs, consultez [créer une API REST avec routage d’attributs dans Web API 2](create-a-rest-api-with-attribute-routing.md).

## <a name="prerequisites"></a>Conditions préalables requises

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Édition Community, Professional ou Enterprise

Vous pouvez également utiliser le gestionnaire de package NuGet pour installer les packages nécessaires. Dans le menu **Outils** de Visual Studio, sélectionnez **Gestionnaire de package NuGet**, puis sélectionnez **console du gestionnaire de package**. Entrez la commande suivante dans la fenêtre console du gestionnaire de package :

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a>Pourquoi le routage des attributs ?

La première version de l’API Web a utilisé le routage *basé sur des conventions* . Dans ce type de routage, vous définissez un ou plusieurs modèles de routage, qui sont fondamentalement des chaînes paramétrables. Lorsque l’infrastructure reçoit une demande, elle correspond à l’URI par rapport au modèle de routage. (Pour plus d’informations sur le routage basé sur une convention, consultez [routage dans API Web ASP.net](routing-in-aspnet-web-api.md).

L’un des avantages du routage basé sur des conventions est que les modèles sont définis dans un emplacement unique et que les règles de routage sont appliquées de manière cohérente sur tous les contrôleurs. Malheureusement, le routage basé sur des conventions rend difficile la prise en charge de certains modèles d’URI qui sont courants dans les API RESTful. Par exemple, les ressources contiennent souvent des ressources enfants : les clients ont des commandes, des films ont des acteurs, des livres ont des auteurs, et ainsi de suite. Il est naturel de créer des URI qui reflètent les relations suivantes :

`/customers/1/orders`

Ce type d’URI est difficile à créer à l’aide du routage basé sur des conventions. Bien que cela puisse être fait, les résultats ne sont pas correctement mis à l’échelle si vous avez plusieurs contrôleurs ou types de ressources.

Avec le routage d’attributs, il est trivial de définir un itinéraire pour cet URI. Il vous suffit d’ajouter un attribut à l’action du contrôleur :

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

Voici d’autres modèles qui facilitent le routage d’attributs.

**Contrôle de version des API**

Dans cet exemple, « /API/v1/Products » est routé vers un autre contrôleur que « /API/v2/Products ».

`/api/v1/products`
`/api/v2/products`

**Segments d’URI surchargés**

Dans cet exemple, « 1 » est un numéro d’ordre, mais « Pending » est mappé à une collection.

`/orders/1`
`/orders/pending`

**Types de paramètres multiples**

Dans cet exemple, « 1 » est un numéro d’ordre, mais « 2013/06/16 » spécifie une date.

`/orders/1`
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a>Activation du routage des attributs

Pour activer le routage d’attribut, appelez **MapHttpAttributeRoutes** pendant la configuration. Cette méthode d’extension est définie dans la classe **System. Web. http. HttpConfigurationExtensions** .

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

Le routage d’attribut peut être combiné avec [le routage basé sur des conventions](routing-in-aspnet-web-api.md) . Pour définir des itinéraires basés sur des conventions, appelez la méthode **MapHttpRoute** .

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

Pour plus d’informations sur la configuration de l’API Web, consultez [configuration de API Web ASP.NET 2](../advanced/configuring-aspnet-web-api.md).

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a>Remarque : migration à partir de l’API Web 1

Avant l’API Web 2, les modèles de projet d’API Web généraient du code comme celui-ci :

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

Si le routage des attributs est activé, ce code lèvera une exception. Si vous mettez à niveau un projet d’API Web existant pour utiliser le routage d’attributs, veillez à mettre à jour ce code de configuration comme suit :

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> Pour plus d’informations, consultez Configuration de l' [API Web avec l’hébergement ASP.net](../advanced/configuring-aspnet-web-api.md#webhost).

<a id="add-routes"></a>
## <a name="adding-route-attributes"></a>Ajouter des attributs de route

Voici un exemple d’itinéraire défini à l’aide d’un attribut :

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

La chaîne &quot;clients/{customerId}/Orders&quot; est le modèle d’URI pour l’itinéraire. L’API Web tente de faire correspondre l’URI de la demande au modèle. Dans cet exemple, « Customers » et « Orders » sont des segments littéraux et « {customerId} » est un paramètre de variable. Les URI suivants correspondent à ce modèle :

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

Vous pouvez restreindre la correspondance à l’aide de [contraintes](#constraints), décrites plus loin dans cette rubrique.

Notez que le paramètre &quot;{customerId}&quot; dans le modèle de routage correspond au nom du paramètre *CustomerID* dans la méthode. Lorsque l’API Web appelle l’action du contrôleur, elle tente de lier les paramètres d’itinéraire. Par exemple, si l’URI est `http://example.com/customers/1/orders`, l’API Web tente de lier la valeur « 1 » au paramètre *CustomerID* dans l’action.

Un modèle d’URI peut avoir plusieurs paramètres :

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

Toutes les méthodes de contrôleur qui n’ont pas d’attribut de routage utilisent le routage basé sur des conventions. De cette façon, vous pouvez combiner les deux types de routage dans le même projet.

## <a name="http-methods"></a>HTTP Methods

L’API Web sélectionne également des actions basées sur la méthode HTTP de la demande (obtenir, poster, etc.). Par défaut, l’API Web recherche une correspondance ne respectant pas la casse avec le début du nom de la méthode du contrôleur. Par exemple, une méthode de contrôleur nommée `PutCustomers` correspond à une requête HTTP PUT.

Vous pouvez substituer cette Convention en décorant la méthode avec les attributs suivants :

- **HttpDelete**
- **[HttpGet]**
- **[HttpHead]**
- **HttpOptions**
- **[HttpPatch]**
- **HttpPost**
- **HttpPut**

L’exemple suivant mappe la méthode CreateBook aux requêtes HTTP HTTP.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

Pour toutes les autres méthodes HTTP, y compris les méthodes non standard, utilisez l’attribut **AcceptVerbs** , qui prend une liste de méthodes http.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a>Préfixes d’itinéraire

Souvent, les routes d’un contrôleur commencent par le même préfixe. Exemple :

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

Vous pouvez définir un préfixe commun pour un contrôleur entier à l’aide de l’attribut **[RoutePrefix]** :

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

Utilisez un tilde (~) sur l’attribut de méthode pour remplacer le préfixe d’itinéraire :

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

Le préfixe d’itinéraire peut inclure des paramètres :

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a>Contraintes d’itinéraire

Les contraintes de routage vous permettent de limiter la correspondance des paramètres dans le modèle de routage. La syntaxe générale est &quot;{Parameter : Constraint}&quot;. Exemple :

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

Ici, la première route est sélectionnée uniquement si l’ID de &quot;&quot; segment de l’URI est un entier. Dans le cas contraire, le deuxième itinéraire est choisi.

Le tableau suivant répertorie les contraintes prises en charge.

| Contrainte | Description | Exemple |
| --- | --- | --- |
| alpha | Correspond aux caractères de l’alphabet latin en majuscules ou en minuscules (a-z, A-Z) | {x:alpha} |
| bool | Correspond à une valeur booléenne. | {x:bool} |
| datetime | Correspond à une valeur **DateTime** . | {x:datetime} |
| decimal | Correspond à une valeur décimale. | {x:decimal} |
| double | Correspond à une valeur à virgule flottante 64 bits. | {x:double} |
| float | Correspond à une valeur à virgule flottante 32 bits. | {x:float} |
| guid | Correspond à une valeur GUID. | {x:guid} |
| int | Correspond à une valeur entière de 32 bits. | {x:int} |
| length | Correspond à une chaîne avec la longueur spécifiée ou dans une plage de longueurs spécifiée. | {x :length (6)} {x :length (1, 20)} |
| long | Correspond à une valeur entière de 64 bits. | {x:long} |
| max | Correspond à un entier avec une valeur maximale. | {x:max(10)} |
| MaxLength | Correspond à une chaîne d’une longueur maximale. | {x:maxlength(10)} |
| min | Correspond à un entier avec une valeur minimale. | {x:min(10)} |
| minLength | Correspond à une chaîne d’une longueur minimale. | {x :minLength (10)} |
| range | Correspond à un entier dans une plage de valeurs. | {x :Range (10, 50)} |
| regex | Correspond à une expression régulière. | {x :Regex (^ \d{3}-\d{3}-\d{4}$)} |

Notez que certaines des contraintes, telles que &quot;min&quot;, prennent des arguments entre parenthèses. Vous pouvez appliquer plusieurs contraintes à un paramètre, séparées par un signe deux-points.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a>Contraintes d’itinéraire personnalisé

Vous pouvez créer des contraintes d’itinéraire personnalisées en implémentant l’interface **IHttpRouteConstraint** . Par exemple, la contrainte suivante restreint un paramètre à une valeur entière différente de zéro.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

Le code suivant montre comment enregistrer la contrainte :

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

Vous pouvez maintenant appliquer la contrainte dans vos itinéraires :

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

Vous pouvez également remplacer la classe **DefaultInlineConstraintResolver** entière en implémentant l’interface **IInlineConstraintResolver** . Cela va remplacer toutes les contraintes intégrées, sauf si votre implémentation de **IInlineConstraintResolver** les ajoute spécifiquement.

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a>Paramètres d’URI facultatifs et valeurs par défaut

Vous pouvez rendre un paramètre URI facultatif en ajoutant un point d’interrogation au paramètre d’itinéraire. Si un paramètre d’itinéraire est facultatif, vous devez définir une valeur par défaut pour le paramètre de la méthode.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

Dans cet exemple, `/api/books/locale/1033` et `/api/books/locale` retourner la même ressource.

Vous pouvez également spécifier une valeur par défaut dans le modèle de routage, comme suit :

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

C’est presque identique à l’exemple précédent, mais il existe une légère différence de comportement lorsque la valeur par défaut est appliquée.

- Dans le premier exemple (« {LCID : int ?} »), la valeur par défaut de 1033 est assignée directement au paramètre de la méthode, de sorte que le paramètre aura cette valeur exacte.
- Dans le deuxième exemple (« {LCID : int = 1033} »), la valeur par défaut « 1033 » passe par le processus de liaison de modèle. Le Binder de modèle par défaut convertit « 1033 » en la valeur numérique 1033. Toutefois, vous pouvez intégrer un classeur de modèles personnalisé, ce qui peut être différent.

(Dans la plupart des cas, sauf si vous avez des classeurs de modèles personnalisés dans votre pipeline, les deux formulaires sont équivalents.)

<a id="route-names"></a>
## <a name="route-names"></a>Noms d’itinéraires

Dans l’API Web, chaque itinéraire a un nom. Les noms de routes sont utiles pour générer des liens, afin que vous puissiez inclure un lien dans une réponse HTTP.

Pour spécifier le nom de l’itinéraire, définissez la propriété **Name** sur l’attribut. L’exemple suivant montre comment définir le nom de l’itinéraire et comment utiliser le nom de l’itinéraire lors de la génération d’un lien.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a>Ordre de routage

Lorsque le Framework tente de faire correspondre un URI avec un itinéraire, il évalue les itinéraires dans un ordre particulier. Pour spécifier l’ordre, définissez la propriété **Order** sur l’attribut route. Les valeurs inférieures sont évaluées en premier. La valeur d’ordre par défaut est zéro.

Voici comment déterminer le classement total :

1. Comparez la propriété **Order** de l’attribut route.
2. Examinez chaque segment d’URI dans le modèle de routage. Pour chaque segment, l’ordre est le suivant :

    1. Segments littéraux.
    2. Acheminer les paramètres avec des contraintes.
    3. Acheminer les paramètres sans contraintes.
    4. Segments de paramètres génériques avec des contraintes.
    5. Segments de paramètres génériques sans contraintes.
3. Dans le cas d’un lien, les itinéraires sont classés par une comparaison de chaînes ordinale ne respectant pas la casse ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) du modèle de routage.

Voici un exemple : Supposons que vous définissiez le contrôleur suivant :

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

Ces itinéraires sont triés comme suit.

1. commandes/détails
2. commandes/{ID}
3. orders/{customerName}
4. commandes/{date de\*}
5. commandes/en attente

Notez que « Details » est un segment littéral qui apparaît avant « {ID} », mais « Pending » apparaît en dernier, car la propriété **Order** est 1. (Cet exemple suppose qu’il n’existe aucun client nommé « Details » ou « pending ». En général, essayez d’éviter les itinéraires ambigus. Dans cet exemple, un meilleur modèle de routage pour `GetByCustomer` est « clients/{customerName} »)
