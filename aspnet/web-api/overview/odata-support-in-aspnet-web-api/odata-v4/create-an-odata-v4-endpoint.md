---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: Créer un point de terminaison OData v4 à l’aide de API Web ASP.NET 2,2 | Microsoft Docs
author: MikeWasson
description: Le Open Data Protocol (OData) est un protocole d’accès aux données pour le Web. OData offre une méthode uniforme pour interroger et manipuler des jeux de données à l’aide d’opérations CRUD...
ms.author: riande
ms.date: 01/23/2019
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 81d134cbd3231b9a0d5537ccbd1bbfe6419254af
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598734"
---
# <a name="create-an-odata-v4-endpoint-using-aspnet-web-api"></a>Créer un point de terminaison OData v4 à l’aide de API Web ASP.NET 

> Le Open Data Protocol (OData) est un protocole d’accès aux données pour le Web. OData offre un moyen uniforme d’interroger et de manipuler des jeux de données via des opérations CRUD (création, lecture, mise à jour et suppression).
>
> API Web ASP.NET prend en charge v3 et v4 du protocole. Vous pouvez même avoir un point de terminaison v4 qui s’exécute côte à côte avec un point de terminaison v3.
>
> Ce didacticiel montre comment créer un point de terminaison OData v4 qui prend en charge les opérations CRUD.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
>
> - API Web 5,2
> - OData v4
> - Visual Studio 2017 (Télécharger Visual Studio 2017 [ici](https://visualstudio.microsoft.com/downloads/))
> - Entity Framework 6
> - 4\.7.2 .NET
>
> ## <a name="tutorial-versions"></a>Versions du didacticiel
>
> Pour OData version 3, consultez [création d’un point de terminaison OData v3](../odata-v3/creating-an-odata-endpoint.md).

## <a name="create-the-visual-studio-project"></a>Créer le projet Visual Studio

Dans Visual Studio, dans le menu **fichier** , sélectionnez **nouveau** &gt; **projet**.

Développez **installé** &gt;  **C# Visual** &gt; **Web**, puis sélectionnez le modèle **application Web ASP.net (.NET Framework)** . Nommez le projet &quot;&quot;ProductService.

[![](create-an-odata-v4-endpoint/_static/image7.png)](create-an-odata-v4-endpoint/_static/image7.png)

Sélectionnez **OK**.

[![](create-an-odata-v4-endpoint/_static/image8.png)](create-an-odata-v4-endpoint/_static/image8.png)

Sélectionnez le modèle **Vide**. Sous **Ajouter des dossiers et des références principales pour :** , sélectionnez **API Web**. Sélectionnez **OK**.

## <a name="install-the-odata-packages"></a>Installer les packages OData

Dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet** &gt; **console du gestionnaire de package**. Dans la fenêtre de la console du gestionnaire de package, tapez :

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

Cette commande installe les derniers packages NuGet OData.

## <a name="add-a-model-class"></a>Ajouter une classe de modèle

Un *modèle* est un objet qui représente une entité de données dans votre application.

Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le dossier Modèles. Dans le menu contextuel, sélectionnez **ajouter** &gt; **classe**.

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> Par Convention, les classes de modèle sont placées dans le dossier Models, mais vous n’êtes pas obligé de suivre cette Convention dans vos propres projets.

Nommez la classe `Product`. Dans le fichier Product.cs, remplacez le code réutilisable par ce qui suit :

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

La propriété `Id` est la clé d’entité. Les clients peuvent interroger les entités par clé. Par exemple, pour obtenir le produit avec l’ID 5, l’URI est `/Products(5)`. La propriété `Id` est également la clé primaire dans la base de données principale.

## <a name="enable-entity-framework"></a>Activer Entity Framework

Pour ce didacticiel, nous allons utiliser Entity Framework (EF) Code First pour créer la base de données principale.

> [!NOTE]
> L’API Web OData ne requiert pas EF. Utilisez n’importe quelle couche d’accès aux données capable de convertir les entités de base de données en modèles.

Tout d’abord, installez le package NuGet pour EF. Dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet** &gt; **console du gestionnaire de package**. Dans la fenêtre de la console du gestionnaire de package, tapez :

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

Ouvrez le fichier Web. config et ajoutez la section suivante à l’intérieur de l’élément de **configuration** , après l’élément **configSections** .

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

Ce paramètre ajoute une chaîne de connexion pour une base de données de base de données locale. Cette base de données sera utilisée lorsque vous exécuterez l’application localement.

Ensuite, ajoutez une classe nommée `ProductsContext` au dossier Models :

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

Dans le constructeur, `"name=ProductsContext"` donne le nom de la chaîne de connexion.

## <a name="configure-the-odata-endpoint"></a>Configurer le point de terminaison OData

Ouvrez le fichier d’application\_Start/WebApiConfig. cs. Ajoutez les instructions **using** suivantes :

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

Ajoutez ensuite le code suivant à la méthode **Register** :

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

Ce code fait deux choses :

- Crée un Entity Data Model (EDM).
- Ajoute un itinéraire.

Un modèle EDM est un modèle abstrait de données. Le modèle EDM est utilisé pour créer le document de métadonnées du service. La classe **ODataConventionModelBuilder** crée un modèle EDM à l’aide des conventions d’affectation des noms par défaut. Cette approche nécessite le moins de code. Si vous souhaitez davantage de contrôle sur le modèle EDM, vous pouvez utiliser la classe **ODataModelBuilder** pour créer le modèle EDM en ajoutant explicitement des propriétés, des clés et des propriétés de navigation.

Un *itinéraire* indique à l’API Web comment acheminer les requêtes http vers le point de terminaison. Pour créer un itinéraire OData v4, appelez la méthode d’extension **MapODataServiceRoute** .

Si votre application comporte plusieurs points de terminaison OData, créez un itinéraire distinct pour chacun d’entre eux. Donnez à chaque itinéraire un nom et un préfixe d’itinéraire uniques.

## <a name="add-the-odata-controller"></a>Ajouter le contrôleur OData

Un *contrôleur* est une classe qui gère les requêtes http. Vous créez un contrôleur distinct pour chaque jeu d’entités dans votre service OData. Dans ce didacticiel, vous allez créer un contrôleur pour l’entité `Product`.

Dans Explorateur de solutions, cliquez avec le bouton droit sur le dossier Controllers et sélectionnez **ajouter** &gt; **classe**. Nommez la classe `ProductsController`.

> [!NOTE]
> La version de ce didacticiel pour OData v3 utilise la génération de modèles automatique de **contrôleur Add** . Actuellement, il n’existe pas de génération de modèles automatique pour OData v4.

Remplacez le code réutilisable dans ProductsController.cs par le code suivant.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

Le contrôleur utilise la classe `ProductsContext` pour accéder à la base de données à l’aide d’EF. Notez que le contrôleur remplace la méthode **dispose** pour supprimer **ProductsContext**.

Il s’agit du point de départ du contrôleur. Nous allons ensuite ajouter des méthodes pour toutes les opérations CRUD.

## <a name="query-the-entity-set"></a>Interroger le jeu d’entités

Ajoutez les méthodes suivantes à `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

La version sans paramètre de la méthode `Get` retourne l’ensemble de la collection Products. La méthode `Get` avec un paramètre de *clé* recherche un produit par sa clé (dans ce cas, la propriété `Id`).

L’attribut **[EnableQuery]** permet aux clients de modifier la requête, en utilisant des options de requête telles que $filter, $sort et $page. Pour plus d’informations, consultez [prise en charge des options de requête OData](../supporting-odata-query-options.md).

## <a name="add-an-entity-to-the-entity-set"></a>Ajouter une entité au jeu d’entités

Pour permettre aux clients d’ajouter un nouveau produit à la base de données, ajoutez la méthode suivante à `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="update-an-entity"></a>Mise à jour d'une entité

OData prend en charge deux sémantiques différentes pour la mise à jour d’une entité, d’un correctif et d’un PUT.

- PATCH effectue une mise à jour partielle. Le client spécifie uniquement les propriétés à mettre à jour.
- PUT remplace l’entité entière.

L’inconvénient de PUT est que le client doit envoyer des valeurs pour toutes les propriétés de l’entité, y compris les valeurs qui ne changent pas. La [spécification OData](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) indique que le correctif est préféré.

Dans tous les cas, voici le code pour les méthodes PATCH et PUT :

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

Dans le cas de PATCH, le contrôleur utilise le type **Delta&lt;t&gt;** pour suivre les modifications.

## <a name="delete-an-entity"></a>Suppression d’une entité

Pour permettre aux clients de supprimer un produit de la base de données, ajoutez la méthode suivante à `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
