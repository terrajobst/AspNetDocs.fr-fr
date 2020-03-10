---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: Routage et sélection des actions dans API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 12/14/2018
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: 62114e56fb29e80c93b82dcb78ce2bc2a123a83b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554886"
---
# <a name="routing-and-action-selection-in-aspnet-web-api"></a>Routage et sélection des actions dans API Web ASP.NET

par [Mike Wasson](https://github.com/MikeWasson)

Cet article explique comment API Web ASP.NET achemine une requête HTTP vers une action particulière sur un contrôleur.

> [!NOTE]
> Pour obtenir une vue d’ensemble générale du routage, consultez [routage dans API Web ASP.net](routing-in-aspnet-web-api.md).

Cet article examine en détail le processus de routage. Si vous créez un projet d’API Web et que certaines demandes ne sont pas acheminées comme prévu, cet article vous aidera.

Le routage comporte trois phases principales :

1. Correspondance de l’URI avec un modèle de routage.
2. Sélection d’un contrôleur.
3. Sélection d’une action.

Vous pouvez remplacer certaines parties du processus par vos propres comportements personnalisés. Dans cet article, je décrirai le comportement par défaut. À la fin, je remarque les endroits où vous pouvez personnaliser le comportement.

## <a name="route-templates"></a>Modèles de routage

Un modèle de routage ressemble à un chemin d’accès d’URI, mais il peut avoir des valeurs d’espace réservé, indiquées par des accolades :

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

Lorsque vous créez un itinéraire, vous pouvez fournir des valeurs par défaut pour certains ou tous les espaces réservés :

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

Vous pouvez également fournir des contraintes, qui restreignent la manière dont un segment d’URI peut correspondre à un espace réservé :

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

L’infrastructure tente de faire correspondre les segments dans le chemin d’accès de l’URI au modèle. Les littéraux dans le modèle doivent correspondre exactement. Un espace réservé correspond à n’importe quelle valeur, sauf si vous spécifiez des contraintes. Le Framework ne correspond pas à d’autres parties de l’URI, telles que le nom d’hôte ou les paramètres de requête. L’infrastructure sélectionne le premier itinéraire dans la table de routage qui correspond à l’URI.

Il existe deux espaces réservés spéciaux : « {Controller} » et « {action} ».

- « {Controller} » fournit le nom du contrôleur.
- « {action} » fournit le nom de l’action. Dans l’API Web, la Convention habituelle consiste à omettre « {action} ».

### <a name="defaults"></a>Valeurs par défaut

Si vous fournissez des valeurs par défaut, l’itinéraire correspond à un URI qui ne contient pas ces segments. Exemple :

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

Les URI `http://localhost/api/products/all` et `http://localhost/api/products` correspondent à l’itinéraire précédent. Dans ce dernier URI, la valeur par défaut `all`est assignée au segment de `{category}` manquant.

### <a name="route-dictionary"></a>Dictionnaire de routage

Si le Framework trouve une correspondance pour un URI, il crée un dictionnaire qui contient la valeur de chaque espace réservé. Les clés sont les noms des espaces réservés, à l’exclusion des accolades. Les valeurs sont extraites du chemin d’accès de l’URI ou des valeurs par défaut. Le dictionnaire est stocké dans l’objet **IHttpRouteData** .

Pendant cette phase de correspondance de routage, les espaces réservés « {Controller} » et « {action} » spéciaux sont traités comme les autres espaces réservés. Elles sont simplement stockées dans le dictionnaire avec les autres valeurs.

Une valeur par défaut peut avoir la valeur spéciale **RouteParameter. optional**. Si cette valeur est assignée à un espace réservé, la valeur n’est pas ajoutée au dictionnaire de routage. Exemple :

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

Pour le chemin d’accès URI « API/Products », le dictionnaire de routage contient les éléments suivants :

- contrôleur : « Products »
- Catégorie : « tous »

Toutefois, pour « API/Products/Toys/123 », le dictionnaire de routes contient les éléments suivants :

- contrôleur : « Products »
- Catégorie : « jouets »
- ID : « 123 »

Les valeurs par défaut peuvent également inclure une valeur qui n’apparaît nulle part dans le modèle de routage. Si l’itinéraire correspond, cette valeur est stockée dans le dictionnaire. Exemple :

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

Si le chemin d’accès de l’URI est « API/root/8 », le dictionnaire contiendra deux valeurs :

- contrôleur : « clients »
- ID : « 8 »

## <a name="selecting-a-controller"></a>Sélection d’un contrôleur

La sélection du contrôleur est gérée par la méthode **IHttpControllerSelector. SelectController** . Cette méthode prend une instance **HttpRequestMessage** et retourne un **HttpControllerDescriptor**. L’implémentation par défaut est fournie par la classe **DefaultHttpControllerSelector** . Cette classe utilise un algorithme simple :

1. Recherchez la clé « Controller » dans le dictionnaire de routage.
2. Prenez la valeur de cette clé et ajoutez la chaîne « Controller » pour obtenir le nom du type de contrôleur.
3. Recherchez un contrôleur d’API Web avec ce nom de type.

Par exemple, si le dictionnaire de routage contient la paire clé-valeur « Controller » = « Products », le type de contrôleur est « ProductsController ». S’il n’y a aucun type correspondant, ou plusieurs correspondances, le Framework retourne une erreur au client.

Pour l’étape 3, **DefaultHttpControllerSelector** utilise l’interface **IHttpControllerTypeResolver** pour obtenir la liste des types de contrôleurs d’API Web. L’implémentation par défaut de **IHttpControllerTypeResolver** retourne toutes les classes publiques qui (a) implémentent **IHttpController**, (b) ne sont pas abstraites et (c) ont un nom qui se termine par « Controller ».

## <a name="action-selection"></a>Sélection de l’action

Après avoir sélectionné le contrôleur, le Framework sélectionne l’action en appelant la méthode **IHttpActionSelector. SelectAction** . Cette méthode prend un **HttpControllerContext** et retourne un **HttpActionDescriptor**.

L’implémentation par défaut est fournie par la classe **ApiControllerActionSelector** . Pour sélectionner une action, elle se présente comme suit :

- Méthode HTTP de la demande.
- Espace réservé « {action} » dans le modèle de routage, le cas échéant.
- Paramètres des actions sur le contrôleur.

Avant d’examiner l’algorithme de sélection, nous devons comprendre certains aspects des actions du contrôleur.

**Quelles méthodes sur le contrôleur sont considérées comme « actions » ?** Lors de la sélection d’une action, l’infrastructure examine uniquement les méthodes d’instance publiques sur le contrôleur. En outre, elle exclut les méthodes de [« nom spécial »](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) (constructeurs, événements, surcharges d’opérateur, etc.) et les méthodes héritées de la classe **ApiController** .

**Méthodes HTTP.** L’infrastructure choisit uniquement les actions qui correspondent à la méthode HTTP de la requête, déterminée comme suit :

1. Vous pouvez spécifier la méthode HTTP avec un attribut : **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**, **HttpOptions**, **HttpPatch**, **HttpPost**ou **HttpPut**.
2. Dans le cas contraire, si le nom de la méthode de contrôleur commence par « obtient », « après », « put », « Delete », « Head », « options » ou « patch », alors que l’action prend en charge cette méthode HTTP.
3. Si aucun des éléments ci-dessus n’est présent, la méthode prend en charge l’envoi.

**Liaisons de paramètres.** Une liaison de paramètre indique comment l’API Web crée une valeur pour un paramètre. Voici la règle par défaut pour la liaison de paramètre :

- Les types simples sont extraits de l’URI.
- Les types complexes sont extraits du corps de la demande.

Les types simples incluent tous les [.NET Framework types primitifs](https://msdn.microsoft.com/library/system.type.isprimitive), plus **DateTime**, **Decimal**, **GUID**, **String**et **TimeSpan**. Pour chaque action, au plus un paramètre peut lire le corps de la demande.

> [!NOTE]
> Il est possible de remplacer les règles de liaison par défaut. Consultez [liaison de paramètre WebAPI sous le capot](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).

Avec cet arrière-plan, voici l’algorithme de sélection d’action.

1. Créez une liste de toutes les actions sur le contrôleur qui correspondent à la méthode de requête HTTP.
2. Si le dictionnaire de routage a une entrée « action », supprimez les actions dont le nom ne correspond pas à cette valeur.
3. Essayez de faire correspondre les paramètres d’action à l’URI, comme suit : 

    1. Pour chaque action, récupérez la liste des paramètres qui sont de type simple, où la liaison obtient le paramètre à partir de l’URI. Excluez les paramètres facultatifs.
    2. Dans cette liste, essayez de trouver une correspondance pour chaque nom de paramètre, soit dans le dictionnaire d’itinéraires, soit dans la chaîne de requête d’URI. Les correspondances ne respectent pas la casse et ne dépendent pas de l’ordre des paramètres.
    3. Sélectionnez une action où chaque paramètre de la liste a une correspondance dans l’URI.
    4. Si plus d’une action répond à ces critères, choisissez celle dont les paramètres correspondent le plus.
4. Ignorez les actions avec l’attribut **[not action]** .

L’étape #3 est probablement le plus confus. L’idée de base est qu’un paramètre peut obtenir sa valeur à partir de l’URI, du corps de la demande ou d’une liaison personnalisée. Pour les paramètres provenant de l’URI, nous voulons nous assurer que l’URI contient effectivement une valeur pour ce paramètre, soit dans le chemin d’accès (via le dictionnaire d’itinéraires), soit dans la chaîne de requête.

Par exemple, considérez l’action suivante :

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

Le paramètre *ID* est lié à l’URI. Par conséquent, cette action ne peut correspondre qu’à un URI qui contient une valeur pour « ID », soit dans le dictionnaire d’itinéraires, soit dans la chaîne de requête.

Les paramètres facultatifs sont une exception, car ils sont facultatifs. Pour un paramètre facultatif, il est OK si la liaison ne peut pas obtenir la valeur de l’URI.

Les types complexes sont une exception pour une raison différente. Un type complexe ne peut être lié qu’à l’URI par le biais d’une liaison personnalisée. Mais dans ce cas, l’infrastructure ne peut pas savoir à l’avance si le paramètre doit être lié à un URI particulier. Pour déterminer, il faut appeler la liaison. L’objectif de l’algorithme de sélection consiste à sélectionner une action à partir de la description statique, avant d’appeler des liaisons. Par conséquent, les types complexes sont exclus de l’algorithme de correspondance.

Une fois l’action sélectionnée, toutes les liaisons de paramètres sont appelées.

Résumé :

- L’action doit correspondre à la méthode HTTP de la requête.
- Le nom de l’action doit correspondre à l’entrée « action » dans le dictionnaire de routage, le cas échéant.
- Pour chaque paramètre de l’action, si le paramètre est extrait de l’URI, le nom du paramètre doit être trouvé dans le dictionnaire d’itinéraires ou dans la chaîne de requête d’URI. (Les paramètres facultatifs et les paramètres avec des types complexes sont exclus.)
- Essayez de faire correspondre le plus grand nombre de paramètres. La meilleure correspondance peut être une méthode sans paramètres.

## <a name="extended-example"></a>Exemple étendu

Poursuit

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

Contrôleur :

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

Requête HTTP :

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a>Correspondance d’itinéraire

L’URI correspond à l’itinéraire nommé « DefaultApi ». Le dictionnaire de routage contient les entrées suivantes :

- contrôleur : « Products »
- ID : « 1 »

Le dictionnaire de routage ne contient pas les paramètres de chaîne de requête « version » et « détails », mais ils seront toujours pris en compte lors de la sélection de l’action.

### <a name="controller-selection"></a>Sélection du contrôleur

À partir de l’entrée « Controller » dans le dictionnaire de routage, le type de contrôleur est `ProductsController`.

### <a name="action-selection"></a>Sélection de l’action

La requête HTTP est une requête d’extraction. Les actions de contrôleur qui prennent en charge sont `GetAll`, `GetById`et `FindProductsByName`. Le dictionnaire de routage ne contient pas d’entrée pour « action », nous n’avons donc pas besoin de faire correspondre le nom de l’action.

Ensuite, nous essayons de faire correspondre les noms de paramètres pour les actions, en regardant uniquement les actions obtenir.

| Action | Paramètres à faire correspondre |
| --- | --- |
| `GetAll` | aucun |
| `GetById` | « ID » |
| `FindProductsByName` | nomme |

Notez que le paramètre de *version* de `GetById` n’est pas pris en compte, car il s’agit d’un paramètre facultatif.

La méthode `GetAll` correspond à triflaconly. La méthode `GetById` correspond également, car le dictionnaire de routes contient « ID ». La méthode `FindProductsByName` ne correspond pas.

La méthode `GetById` gagne, car elle correspond à un paramètre, et non pas aux paramètres de `GetAll`. La méthode est appelée avec les valeurs de paramètre suivantes :

- *ID* = 1
- *version* = 1,5

Notez que même si la *version* n’a pas été utilisée dans l’algorithme de sélection, la valeur du paramètre provient de la chaîne de requête d’URI.

## <a name="extension-points"></a>Points d’extension

L’API Web fournit des points d’extension pour certaines parties du processus de routage.

| Interface | Description |
| --- | --- |
| **IHttpControllerSelector** | Sélectionne le contrôleur. |
| **IHttpControllerTypeResolver** | Obtient la liste des types de contrôleurs. Le **DefaultHttpControllerSelector** choisit le type de contrôleur dans cette liste. |
| **IAssembliesResolver** | Obtient la liste des assemblys de projet. L’interface **IHttpControllerTypeResolver** utilise cette liste pour rechercher les types de contrôleur. |
| **IHttpControllerActivator** | Crée de nouvelles instances de contrôleur. |
| **IHttpActionSelector** | Sélectionne l'action. |
| **IHttpActionInvoker** | Appelle l’action. |

Pour fournir votre propre implémentation pour l’une de ces interfaces, utilisez la collection **services** sur l’objet **HttpConfiguration** :

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
