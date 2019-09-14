---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: Liaison de paramètre dans API Web ASP.NET-ASP.NET 4. x
author: MikeWasson
description: Décrit comment les API Web lient les paramètres et comment personnaliser le processus de liaison dans ASP.NET 4. x.
ms.author: riande
ms.date: 07/11/2013
ms.custom: seoapril2019
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 5386532ab581e023d93d16a5d4107e07f40b986f
ms.sourcegitcommit: 4b324a11131e38f920126066b94ff478aa9927f8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70985822"
---
# <a name="parameter-binding-in-aspnet-web-api"></a>Liaison de paramètre dans API Web ASP.NET

par [Mike Wasson](https://github.com/MikeWasson)

[!INCLUDE[](~/includes/coreWebAPI.md)]

Cet article décrit comment les API Web lient les paramètres et comment vous pouvez personnaliser le processus de liaison. Lorsque l’API Web appelle une méthode sur un contrôleur, elle doit définir des valeurs pour les paramètres, un processus appelé *Binding*.

Par défaut, l’API Web utilise les règles suivantes pour lier les paramètres :

- Si le paramètre est un type « simple », l’API Web tente d’obtenir la valeur de l’URI. Les types simples incluent les [types primitifs](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) .net (**int**, **bool**, **double**, etc.), plus **TimeSpan**, **DateTime**, **GUID**, **Decimal**et **String**, *ainsi* que tout type avec un type convertisseur qui peut effectuer une conversion à partir d’une chaîne. (En savoir plus sur les convertisseurs de type ultérieurement.)
- Pour les types complexes, l’API Web tente de lire la valeur à partir du corps du message, à l’aide d’un [formateur de type média](media-formatters.md).

Par exemple, voici une méthode de contrôleur d’API Web typique :

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

Le paramètre *ID* est un &quot;type&quot; simple, de sorte que l’API Web tente d’obtenir la valeur à partir de l’URI de la demande. Le paramètre d' *élément* est un type complexe, de sorte que l’API Web utilise un formateur de type de média pour lire la valeur du corps de la demande.

Pour obtenir une valeur à partir de l’URI, l’API Web examine les données d’itinéraire et la chaîne de requête d’URI. Les données d’itinéraire sont renseignées lorsque le système de routage analyse l’URI et le met en correspondance avec un itinéraire. Pour plus d’informations, consultez [routage et sélection des actions](../web-api-routing-and-actions/routing-and-action-selection.md).

Dans le reste de cet article, je vais vous montrer comment vous pouvez personnaliser le processus de liaison de modèle. Toutefois, pour les types complexes, envisagez d’utiliser des formateurs de type de média dans la mesure du possible. L’un des principaux principes du protocole HTTP est que les ressources sont envoyées dans le corps du message, à l’aide de la négociation de contenu pour spécifier la représentation de la ressource. Les formateurs de type de média ont été conçus à cet effet.

## <a name="using-fromuri"></a>Utilisation de [FromUri]

Pour forcer l’API Web à lire un type complexe à partir de l’URI, ajoutez l’attribut **[FromUri]** au paramètre. L’exemple suivant définit un `GeoPoint` type, ainsi qu’une méthode de contrôleur qui obtient `GeoPoint` le à partir de l’URI.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

Le client peut placer les valeurs de latitude et de longitude dans la chaîne de requête et l’API Web les utilisera pour construire un `GeoPoint`. Par exemple :

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a>Utilisation de [FromBody]

Pour forcer l’API Web à lire un type simple à partir du corps de la requête, ajoutez l’attribut **[FromBody]** au paramètre :

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

Dans cet exemple, l’API Web utilise un formateur de type de média pour lire la valeur du *nom* dans le corps de la demande. Voici un exemple de demande de client.

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

Lorsqu’un paramètre a la valeur [FromBody], l’API Web utilise l’en-tête Content-type pour sélectionner un formateur. Dans cet exemple, le type de contenu &quot;est application/&quot; JSON et le corps de la demande est une chaîne JSON brute (pas un objet JSON).

Au plus un paramètre est autorisé à lire à partir du corps du message. Cela ne fonctionnera donc pas :

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

La raison de cette règle est que le corps de la demande peut être stocké dans un flux non mis en mémoire tampon qui ne peut être lu qu’une seule fois.

## <a name="type-converters"></a>Convertisseurs de type

Vous pouvez faire en sorte que l’API Web traite une classe comme un type simple (afin que l’API Web tente de la lier à partir de l’URI) en créant un **TypeConverter** et en fournissant une conversion de chaîne.

Le code suivant illustre une `GeoPoint` classe qui représente un point géographique, plus un **TypeConverter** qui convertit des chaînes `GeoPoint` en instances. La `GeoPoint` classe est décorée avec un attribut **[TypeConverter]** pour spécifier le convertisseur de type. (Cet exemple s’est inspiré du billet de blog de Mike Cabinet [Comment lier des objets personnalisés dans des signatures d’action dans MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

Désormais, l’API Web `GeoPoint` traitera comme un type simple, ce qui signifie qu' `GeoPoint` elle essaiera de lier les paramètres à partir de l’URI. Vous n’avez pas besoin d’inclure **[FromUri]** sur le paramètre.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

Le client peut appeler la méthode avec un URI comme suit :

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a>Classeurs de modèles

Une option plus flexible qu’un convertisseur de type consiste à créer un classeur de modèles personnalisé. Avec un classeur de modèles, vous avez accès à des éléments tels que la requête HTTP, la description de l’action et les valeurs brutes des données d’itinéraire.

Pour créer un classeur de modèles, implémentez l’interface **IModelBinder** . Cette interface définit une méthode unique, **BindModel**:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

Voici un classeur de modèles pour `GeoPoint` les objets.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

Un classeur de modèles obtient des valeurs d’entrée brutes à partir d’un *fournisseur de valeurs*. Cette conception sépare deux fonctions distinctes :

- Le fournisseur de valeur prend la requête HTTP et remplit un dictionnaire de paires clé-valeur.
- Le Binder de modèle utilise ce dictionnaire pour remplir le modèle.

Le fournisseur de valeurs par défaut dans l’API Web obtient des valeurs à partir des données d’itinéraire et de la chaîne de requête. Par exemple, si l’URI est `http://localhost/api/values/1?location=48,-122`, le fournisseur de valeurs crée les paires clé-valeur suivantes :

- id = &quot;1&quot;
- emplacement = &quot;48 122&quot;

(Je suppose que le modèle de routage par défaut est &quot;l’API/{Controller}/{ID}&quot;.)

Le nom du paramètre à lier est stocké dans la propriété **ModelBindingContext. ModelName** . Le Binder de modèle recherche une clé avec cette valeur dans le dictionnaire. Si la valeur existe et peut être convertie `GeoPoint`en un, le classeur de modèles affecte la valeur liée à la propriété **ModelBindingContext. Model** .

Notez que le classeur de modèles n’est pas limité à une conversion de type simple. Dans cet exemple, le Binder de modèle commence par Rechercher dans une table d’emplacements connus et, en cas d’échec, il utilise la conversion de type.

**Définition du classeur de modèles**

Il existe plusieurs façons de définir un classeur de modèles. Tout d’abord, vous pouvez ajouter un attribut **[ModelBinder]** au paramètre.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

Vous pouvez également ajouter un attribut **[ModelBinder]** au type. L’API Web utilise le classeur de modèles spécifié pour tous les paramètres de ce type.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

Enfin, vous pouvez ajouter un fournisseur de classeurs de modèles à **HttpConfiguration**. Un fournisseur de classeurs de modèles est simplement une classe de fabrique qui crée un classeur de modèles. Vous pouvez créer un fournisseur en dérivant de la classe [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) . Toutefois, si votre classeur de modèles gère un type unique, il est plus facile d’utiliser le **SimpleModelBinderProvider**intégré, conçu à cet effet. Le code suivant montre comment procéder.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

Avec un fournisseur de liaison de modèle, vous devez toujours ajouter l’attribut **[ModelBinder]** au paramètre pour indiquer à l’API Web qu’il doit utiliser un classeur de modèles et non un formateur de type de média. Mais maintenant, vous n’avez pas besoin de spécifier le type de Binder de modèle dans l’attribut :

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a>Fournisseurs de valeurs

J’ai indiqué qu’un Binder de modèle obtient des valeurs à partir d’un fournisseur de valeurs. Pour écrire un fournisseur de valeurs personnalisées, implémentez l’interface **IValueProvider** . Voici un exemple qui extrait les valeurs des cookies de la requête :

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

Vous devez également créer une fabrique de fournisseur de valeur en dérivant de la classe **ValueProviderFactory** .

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

Ajoutez la fabrique de fournisseur de valeur au **HttpConfiguration** comme suit.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

L’API Web compose tous les fournisseurs de valeurs, donc lorsqu’un Binder de modèle appelle **ValueProvider. GetValue**, le Binder de modèle reçoit la valeur du premier fournisseur de valeur capable de le produire.

Vous pouvez également définir la fabrique de fournisseur de valeur au niveau du paramètre à l’aide de l’attribut **ValueProvider** , comme suit :

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

Cela indique à l’API Web d’utiliser la liaison de modèle avec la fabrique de fournisseur de valeur spécifiée, et de ne pas utiliser les autres fournisseurs de valeurs inscrits.

## <a name="httpparameterbinding"></a>HttpParameterBinding

Les classeurs de modèles sont une instance spécifique d’un mécanisme plus général. Si vous examinez l’attribut **[ModelBinder]** , vous verrez qu’il dérive de la classe abstraite **ParameterBindingAttribute** . Cette classe définit une méthode unique, **GetBinding**, qui retourne un objet **HttpParameterBinding** :

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

Un **HttpParameterBinding** est chargé de lier un paramètre à une valeur. Dans le cas de **[ModelBinder]** , l’attribut retourne une implémentation **HttpParameterBinding** qui utilise un **IModelBinder** pour effectuer la liaison réelle. Vous pouvez également implémenter votre propre **HttpParameterBinding**.

Par exemple, supposons que vous souhaitiez obtenir des `if-match` ETags à partir des en-têtes et `if-none-match` dans la demande. Nous allons commencer par définir une classe pour représenter des ETags.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

Nous allons également définir une énumération pour indiquer si l’ETag doit être obtenu `if-match` à partir de `if-none-match` l’en-tête ou de l’en-tête.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

Voici un **HttpParameterBinding** qui obtient l’ETag de l’en-tête souhaité et le lie à un paramètre de type ETag :

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

La méthode **ExecuteBindingAsync** effectue la liaison. Dans cette méthode, ajoutez la valeur de paramètre lié au dictionnaire **ActionArgument** dans le **HttpActionContext**.

> [!NOTE]
> Si votre méthode **ExecuteBindingAsync** lit le corps du message de demande, remplacez la valeur de la propriété **WillReadBody** pour retourner true. Le corps de la demande peut être un flux non mis en mémoire tampon qui ne peut être lu qu’une seule fois. par conséquent, l’API Web applique une règle qui permet au plus à une liaison de lire le corps du message.

Pour appliquer un **HttpParameterBinding**personnalisé, vous pouvez définir un attribut qui dérive de **ParameterBindingAttribute**. Pour `ETagParameterBinding`, nous allons définir deux attributs, un pour `if-match` les en-têtes et `if-none-match` un pour les en-têtes. Les deux dérivent d’une classe de base abstraite.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

Voici une méthode de contrôleur qui utilise l' `[IfNoneMatch]` attribut.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

Outre **ParameterBindingAttribute**, il existe un autre raccordement pour l’ajout d’un **HttpParameterBinding**personnalisé. Sur l' **objet HttpConfiguration** , la propriété **ParameterBindingRules** est une collection de fonctions anonymes de type (**HttpParameterDescriptor**  -&gt; HttpParameterBinding). Par exemple, vous pouvez ajouter une règle qu’un paramètre ETag sur une méthode d’extraction `ETagParameterBinding` utilise `if-none-match`avec :

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

La fonction doit retourner `null` les paramètres pour lesquels la liaison n’est pas applicable.

## <a name="iactionvaluebinder"></a>IActionValueBinder

L’ensemble du processus de liaison de paramètres est contrôlé par un service enfichable, **IActionValueBinder**. L’implémentation par défaut de **IActionValueBinder** effectue les opérations suivantes :

1. Recherchez un **ParameterBindingAttribute** sur le paramètre. Cela comprend les attributs **[FromBody]** , **[FromUri]** et **[ModelBinder]** , ou les attributs personnalisés.
2. Sinon, recherchez dans **HttpConfiguration. ParameterBindingRules** une fonction qui retourne un **HttpParameterBinding**non null.
3. Dans le cas contraire, utilisez les règles par défaut que j’ai décrites précédemment. 

    - Si le type de paramètre est « simple » ou a un convertisseur de type, établissez une liaison à partir de l’URI. Cela équivaut à placer l’attribut **[FromUri]** sur le paramètre.
    - Dans le cas contraire, essayez de lire le paramètre à partir du corps du message. Cela équivaut à placer **[FromBody]** sur le paramètre.

Si vous le souhaitez, vous pouvez remplacer l’ensemble du service **IActionValueBinder** par une implémentation personnalisée.

## <a name="additional-resources"></a>Ressources supplémentaires

[Exemple de liaison de paramètre personnalisé](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

Mike Stall a écrit une bonne série de billets de blog sur la liaison de paramètres d’API Web :

- [Mode de liaison des paramètres par l’API Web](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [Liaison de paramètre de style MVC pour l’API Web](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [Comment lier des objets personnalisés dans des signatures d’action dans MVC/API Web](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [Comment créer un fournisseur de valeurs personnalisées dans l’API Web](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [Liaison de paramètres d’API Web en coulisses](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
