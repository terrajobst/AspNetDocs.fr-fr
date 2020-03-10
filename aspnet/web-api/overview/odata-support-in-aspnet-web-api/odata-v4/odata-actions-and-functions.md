---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Actions et fonctions dans OData v4 à l’aide de API Web ASP.NET 2,2 | Microsoft Docs
author: MikeWasson
description: Dans OData, les actions et les fonctions sont un moyen d’ajouter des comportements côté serveur qui ne sont pas facilement définis en tant qu’opérations CRUD sur les entités. Ce didacticiel montre comment...
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: f5af94e93e5b7f2351d40febbf1a468d635c9db1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556223"
---
# <a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a>Actions et fonctions dans OData v4 à l’aide de API Web ASP.NET 2,2

par [Mike Wasson](https://github.com/MikeWasson)

> Dans OData, les actions et les fonctions sont un moyen d’ajouter des comportements côté serveur qui ne sont pas facilement définis en tant qu’opérations CRUD sur les entités. Ce didacticiel montre comment ajouter des actions et des fonctions à un point de terminaison OData v4, à l’aide de l’API Web 2,2. Le didacticiel s’appuie sur le didacticiel [créer un point de terminaison OData v4 à l’aide de API Web ASP.NET 2](create-an-odata-v4-endpoint.md)
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
>
> - API Web 2,2
> - OData v4
> - Visual Studio 2013 (Téléchargez Visual Studio 2017 [ici](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))
> - .NET 4.5
>
> ## <a name="tutorial-versions"></a>Versions du didacticiel
>
> Pour OData version 3, consultez [actions OData dans API Web ASP.NET 2](../odata-v3/odata-actions.md).

La différence entre les *actions* et les *fonctions* est que les actions peuvent avoir des effets secondaires, et non des fonctions. Les actions et les fonctions peuvent retourner des données. Certaines utilisations des actions sont les suivantes :

- Transactions complexes.
- Manipulation de plusieurs entités à la fois.
- Autorisation des mises à jour uniquement pour certaines propriétés d’une entité.
- Envoi de données qui ne sont pas une entité.

Les fonctions sont utiles pour retourner des informations qui ne correspondent pas directement à une entité ou à une collection.

Une action (ou fonction) peut cibler une entité ou une collection unique. Dans la terminologie OData, il s’agit de la *liaison*. Vous pouvez également disposer d' &quot;&quot; actions/fonctions indépendantes, qui sont appelées comme des opérations statiques sur le service.

## <a name="example-adding-an-action"></a>Exemple : ajout d’une action

Nous allons définir une action pour évaluer un produit.

> [!NOTE]
> Ce didacticiel s’appuie sur le didacticiel [créer un point de terminaison OData v4 à l’aide de API Web ASP.NET 2](create-an-odata-v4-endpoint.md)

Tout d’abord, ajoutez un modèle de `ProductRating` pour représenter les évaluations.

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

Ajoutez également un **DbSet** à la classe `ProductsContext`, afin que EF crée une table d’évaluation dans la base de données.

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a>Ajouter l’action à l’EDM

Dans WebApiConfig.cs, ajoutez le code suivant :

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

La méthode **EntityTypeConfiguration. action** ajoute une action au modèle EDM (Entity Data Model). La méthode de **paramètre** spécifie un paramètre typé pour l’action.

Ce code définit également l’espace de noms pour le modèle EDM. L’espace de noms est important car l’URI de l’action comprend le nom d’action complet :

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> Dans une configuration IIS standard, le point dans cette URL entraîne le renvoi de l’erreur 404 par IIS. Pour résoudre ce risque, ajoutez la section suivante à votre fichier Web. config :

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a>Ajouter une méthode de contrôleur pour l’action

Pour activer l’action&quot; rate &quot;, ajoutez la méthode suivante à `ProductsController`:

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

Notez que le nom de la méthode correspond au nom de l’action. L’attribut **[HttpPost]** spécifie que la méthode est une méthode http postale.

Pour appeler l’action, le client envoie une requête HTTP HTTP semblable à la suivante :

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

L’action &quot;rate&quot; est liée aux instances de produit. l’URI de l’action est donc le nom d’action complet ajouté à l’URI de l’entité. (Rappelez-vous que nous définissons l’espace de noms EDM sur &quot;ProductService&quot;, le nom d’action complet est donc &quot;ProductService. rate&quot;.)

Le corps de la requête contient les paramètres d’action sous la forme d’une charge utile JSON. L’API Web convertit automatiquement la charge utile JSON en objet **ODataActionParameters** , qui est simplement un dictionnaire de valeurs de paramètre. Utilisez ce dictionnaire pour accéder aux paramètres de votre méthode de contrôleur.

Si le client envoie les paramètres d’action dans un format incorrect, la valeur de **ModelState. IsValid** est false. Cochez cet indicateur dans votre méthode de contrôleur et retournez une erreur si **IsValid** a la valeur false.

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a>Exemple : ajout d’une fonction

À présent, nous allons ajouter une fonction OData qui retourne le produit le plus onéreux. Comme précédemment, la première étape consiste à ajouter la fonction à l’EDM. Dans WebApiConfig.cs, ajoutez le code suivant.

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

Dans ce cas, la fonction est liée à la collection Products, plutôt qu’à des instances de produit individuelles. Les clients appellent la fonction en envoyant une requête d’extraction :

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

Voici la méthode de contrôleur pour cette fonction :

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

Notez que le nom de la méthode correspond au nom de la fonction. L’attribut **[HttpGet]** spécifie que la méthode est une méthode http d’extraction.

Voici la réponse HTTP :

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a>Exemple : ajout d’une fonction indépendante

L’exemple précédent était une fonction liée à une collection. Dans l’exemple suivant, nous allons créer une fonction *indépendante* . Les fonctions indépendantes sont appelées en tant qu’opérations statiques sur le service. Dans cet exemple, la fonction retourne les taxes de vente pour un code postal donné.

Dans le fichier WebApiConfig, ajoutez la fonction au modèle EDM :

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

Notez que nous appelons la **fonction** directement sur le **ODataModelBuilder**, au lieu du type d’entité ou de la collection. Cela indique au générateur de modèles que la fonction est détachée.

Voici la méthode de contrôleur qui implémente la fonction :

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

Peu importe le contrôleur d’API Web dans lequel vous placez cette méthode. Vous pouvez le placer dans `ProductsController`ou définir un contrôleur séparé. L’attribut **[ODataRoute]** définit le modèle d’URI pour la fonction.

Voici un exemple de demande de client :

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

Réponse HTTP :

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
