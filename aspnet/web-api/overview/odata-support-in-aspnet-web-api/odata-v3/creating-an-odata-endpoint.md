---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: Création d’un point de terminaison OData v3 avec l’API Web 2 | Microsoft Docs
author: MikeWasson
description: Le Open Data Protocol (OData) est un protocole d’accès aux données pour le Web. OData offre une méthode uniforme pour structurer les données, interroger les données et manipuler les données...
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: e68a454398f109dfd089be9c9a44d3fe662acc2f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556412"
---
# <a name="creating-an-odata-v3-endpoint-with-web-api-2"></a>Création d’un point de terminaison OData v3 avec l’API Web 2

par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Le [Open Data Protocol](http://www.odata.org/) (OData) est un protocole d’accès aux données pour le Web. OData offre une méthode uniforme pour structurer les données, interroger les données et manipuler le jeu de données via des opérations CRUD (création, lecture, mise à jour et suppression). OData prend en charge les formats AtomPub (XML) et JSON. OData définit également un moyen d’exposer les métadonnées relatives aux données. Les clients peuvent utiliser les métadonnées pour découvrir les informations de type et les relations pour le jeu de données.
>
> API Web ASP.NET permet de créer facilement un point de terminaison OData pour un jeu de données. Vous pouvez contrôler exactement les opérations OData prises en charge par le point de terminaison. Vous pouvez héberger plusieurs points de terminaison OData, ainsi que des points de terminaison non OData. Vous disposez d’un contrôle total sur votre modèle de données, la logique métier principale et la couche de données.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - API Web 2
> - OData version 3
> - Entity Framework 6
> - [Proxy de débogage Web Fiddler (facultatif)](http://www.fiddler2.com)
>
> La prise en charge de l’API Web OData a été ajoutée dans [ASP.net et Web Tools mise à jour 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650). Toutefois, ce didacticiel utilise la génération de modèles automatique qui a été ajoutée dans Visual Studio 2013.

Dans ce didacticiel, vous allez créer un point de terminaison OData simple que les clients peuvent interroger. Vous allez également créer un C# client pour le point de terminaison. Une fois ce didacticiel terminé, les didacticiels suivants montrent comment ajouter d’autres fonctionnalités, notamment les relations d’entité, les actions et les $expand/$select.

- [Créer le projet Visual Studio](#create-project)
- [Ajouter un modèle d’entité](#add-model)
- [Ajouter un contrôleur OData](#add-controller)
- [Ajouter le modèle EDM et l’itinéraire](#edm)
- [Amorcer la base de données (facultatif)](#seed-db)
- [Exploration du point de terminaison OData](#explore)
- [Formats de sérialisation OData](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a>Créer le projet Visual Studio

Dans ce didacticiel, vous allez créer un point de terminaison OData qui prend en charge les opérations CRUD de base. Le point de terminaison expose une ressource unique, une liste de produits. Les didacticiels ultérieurs ajoutent des fonctionnalités supplémentaires.

Démarrez Visual Studio et sélectionnez **nouveau projet** dans la page de démarrage. Ou, dans le menu **fichier** , sélectionnez **nouveau** , puis **projet**.

Dans le volet **modèles** , sélectionnez **modèles installés** , puis développez C# le nœud visuel. Sous **visuel C#** , sélectionnez **Web**. Sélectionnez **le modèle application Web ASP.net** .

![](creating-an-odata-endpoint/_static/image1.png)

Dans la boîte de dialogue **nouveau projet ASP.net** , sélectionnez le modèle **vide** . Sous &quot;ajouter des dossiers et des références principales pour...&quot;, consultez **API Web**. Cliquez sur **OK**.

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a>Ajouter un modèle d’entité

Un *modèle* est un objet qui représente les données dans votre application. Pour ce didacticiel, nous avons besoin d’un modèle qui représente un produit. Le modèle correspond à notre type d’entité OData.

Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le dossier Modèles. Dans le menu contextuel, sélectionnez **Ajouter**, puis **Classe**.

![](creating-an-odata-endpoint/_static/image3.png)

Dans la boîte de dialogue **Ajouter un nouvel** élément, nommez la classe &quot;&quot;produit.

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> Par Convention, les classes de modèle sont placées dans le dossier Models. Vous n’êtes pas obligé de suivre cette Convention dans vos propres projets, mais nous allons l’utiliser pour ce didacticiel.

Dans le fichier Product.cs, ajoutez la définition de classe suivante :

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

La propriété ID est la clé d’entité. Les clients peuvent interroger les produits par ID. Ce champ est également la clé primaire de la base de données principale.

Générez le projet maintenant. À l’étape suivante, nous allons utiliser une structure de génération de modèles automatique de Visual Studio qui utilise la réflexion pour rechercher le type de produit.

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a>Ajouter un contrôleur OData

Un *contrôleur* est une classe qui gère les requêtes http. Vous définissez un contrôleur distinct pour chaque jeu d’entités dans votre service OData. Dans ce didacticiel, nous allons créer un seul contrôleur.

Dans Explorateur de solutions, cliquez avec le bouton droit sur le dossier Controllers. Sélectionnez **Ajouter**, puis **Contrôleur**.

![](creating-an-odata-endpoint/_static/image5.png)

Dans la boîte de dialogue **Ajouter une structure** , sélectionnez &quot;contrôleur OData API Web 2 avec actions, à l’aide de Entity Framework&quot;.

![](creating-an-odata-endpoint/_static/image6.png)

Dans la boîte de dialogue **Ajouter un contrôleur** , nommez le contrôleur « ProductsController ». Activez la case à cocher &quot;utiliser les actions du contrôleur Async&quot;. Dans la liste déroulante **modèle** , sélectionnez la classe Product.

![](creating-an-odata-endpoint/_static/image7.png)

Cliquez sur le bouton **nouveau contexte de données..** .. Laissez le nom par défaut pour le type de contexte de données, puis cliquez sur **Ajouter**.

![](creating-an-odata-endpoint/_static/image8.png)

Cliquez sur Ajouter dans la boîte de dialogue Ajouter un contrôleur pour ajouter le contrôleur.

![](creating-an-odata-endpoint/_static/image9.png)

Remarque : Si vous obtenez un message d’erreur qui indique &quot;une erreur s’est produite lors de l’obtention du&quot;de type..., assurez-vous que vous avez créé le projet Visual Studio après avoir ajouté la classe Product. La génération de modèles automatique utilise la réflexion pour rechercher la classe.

![](creating-an-odata-endpoint/_static/image10.png)

La génération de modèles automatique ajoute deux fichiers de code au projet :

- Products.cs définit le contrôleur d’API Web qui implémente le point de terminaison OData.
- ProductServiceContext.cs fournit des méthodes pour interroger la base de données sous-jacente à l’aide de Entity Framework.

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a>Ajouter le modèle EDM et l’itinéraire

Dans Explorateur de solutions, développez le dossier de démarrage de l’application\_et ouvrez le fichier nommé WebApiConfig.cs. Cette classe contient le code de configuration pour l’API Web. Remplacez ce code par ce qui suit :

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

Ce code fait deux choses :

- Crée un Entity Data Model (EDM) pour le point de terminaison OData.
- Ajoute un itinéraire pour le point de terminaison.

Un modèle EDM est un modèle abstrait de données. Le modèle EDM est utilisé pour créer le document de métadonnées et définir les URI pour le service. **ODataConventionModelBuilder** crée un modèle EDM à l’aide d’un ensemble de conventions d’affectation de noms par défaut EDM. Cette approche nécessite le moins de code. Si vous souhaitez davantage de contrôle sur le modèle EDM, vous pouvez utiliser la classe **ODataModelBuilder** pour créer le modèle EDM en ajoutant explicitement des propriétés, des clés et des propriétés de navigation.

La méthode **EntitySet** ajoute un jeu d’entités à l’EDM :

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

La chaîne « Products » définit le nom du jeu d’entités. Le nom du contrôleur doit correspondre au nom du jeu d’entités. Dans ce didacticiel, le jeu d’entités est nommé « Products » et le contrôleur est nommé `ProductsController`. Si vous avez nommé le jeu d’entités « ProductSet », vous nommez le contrôleur `ProductSetController`. Notez qu’un point de terminaison peut avoir plusieurs jeux d’entités. Appelez **EntitySet&lt;t&gt;** pour chaque jeu d’entités, puis définissez un contrôleur correspondant.

La méthode **MapODataRoute** ajoute un itinéraire pour le point de terminaison OData.

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

Le premier paramètre est un nom convivial pour l’itinéraire. Les clients de votre service ne voient pas ce nom. Le deuxième paramètre est le préfixe URI du point de terminaison. À partir de ce code, l’URI du jeu d’entités Products est http://<em>hostname</em>/OData/Products. Votre application peut avoir plusieurs points de terminaison OData. Pour chaque point de terminaison, appelez <strong>MapODataRoute</strong> et fournissez un nom d’itinéraire unique et un préfixe URI unique.

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a>Amorcer la base de données (facultatif)

Dans cette étape, vous allez utiliser Entity Framework pour amorcer la base de données avec des données de test. Cette étape est facultative, mais elle vous permet de tester votre point de terminaison OData immédiatement.

Dans le menu **Outils** , sélectionnez **Gestionnaire de package NuGet**, puis sélectionnez **console du gestionnaire de package**. Dans la fenêtre Console du Gestionnaire de package, entrez la commande suivante :

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

Cela ajoute un dossier nommé migrations et un fichier de code nommé Configuration.cs.

![](creating-an-odata-endpoint/_static/image12.png)

Ouvrez ce fichier et ajoutez le code suivant à la méthode `Configuration.Seed`.

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

Dans la fenêtre console du gestionnaire de package, entrez les commandes suivantes :

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

Ces commandes génèrent du code qui crée la base de données, puis exécute ce code.

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a>Exploration du point de terminaison OData

Dans cette section, nous allons utiliser le [proxy de débogage Web Fiddler](http://www.fiddler2.com) pour envoyer des demandes au point de terminaison et examiner les messages de réponse. Cela vous aidera à comprendre les fonctionnalités d’un point de terminaison OData.

Dans Visual Studio, appuyez sur F5 pour démarrer le débogage. Par défaut, Visual Studio ouvre votre navigateur pour `http://localhost:*port*`, où *port* est le numéro de port configuré dans les paramètres du projet.

Vous pouvez modifier le numéro de port dans les paramètres du projet. Dans Explorateur de solutions, cliquez avec le bouton droit sur le projet, puis sélectionnez **Propriétés**. Dans la fenêtre Propriétés, sélectionnez **Web**. Entrez le numéro de port sous **URL du projet**.

### <a name="service-document"></a>Document de service

Le *document de service* contient une liste des jeux d’entités pour le point de terminaison OData. Pour obtenir le document de service, envoyez une requête d’extraction à l’URI racine du service.

À l’aide de Fiddler, entrez l’URI suivant sous l’onglet **compositeur** : `http://localhost:port/odata/`, où *port* est le numéro de port.

![](creating-an-odata-endpoint/_static/image13.png)

Cliquez sur le bouton **Exécuter** . Fiddler envoie une requête HTTP-to à votre application. La réponse doit apparaître dans la liste sessions Web. Si tout fonctionne, le code d’État sera 200.

![](creating-an-odata-endpoint/_static/image14.png)

Double-cliquez sur la réponse dans la liste sessions Web pour afficher les détails du message de réponse sous l’onglet inspecteurs.

![](creating-an-odata-endpoint/_static/image15.png)

Le message de réponse HTTP brut doit ressembler à ce qui suit :

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

Par défaut, l’API Web retourne le document de service au format AtomPub. Pour demander JSON, ajoutez l’en-tête suivant à la requête HTTP :

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

La réponse HTTP contient maintenant une charge utile JSON :

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a>Document de métadonnées de service

Le *document de métadonnées du service* décrit le modèle de données du service, à l’aide d’un langage XML appelé CSDL (Conceptual Schema Definition Language). Le document de métadonnées affiche la structure des données dans le service et peut être utilisé pour générer le code client.

Pour obtenir le document de métadonnées, envoyez une demande de récupération à `http://localhost:port/odata/$metadata`. Voici les métadonnées du point de terminaison présenté dans ce didacticiel.

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a>Jeu d'entités

Pour obtenir le jeu d’entités Products, envoyez une demande de récupération à `http://localhost:port/odata/Products`.

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a>Entité

Pour obtenir un produit individuel, envoyez une demande de récupération à `http://localhost:port/odata/Products(1)`, où « 1 » est l’ID du produit.

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a>Formats de sérialisation OData

OData prend en charge plusieurs formats de sérialisation :

- Pub Atom (XML)
- JSON « Light » (introduit dans OData v3)
- JSON « verbose » (OData v2)

Par défaut, l’API Web utilise le format « Light » AtomPubJSON.

Pour accéder au format AtomPub, définissez l’en-tête Accept sur « application/Atom + XML ». Voici un exemple de corps de réponse :

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

Vous pouvez voir un inconvénient évident du format Atom : il est beaucoup plus détaillé que la lumière JSON. Toutefois, si vous avez un client qui comprend AtomPub, le client peut préférer ce format sur JSON.

Voici la version JSON Light de la même entité :

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

Le format JSON Light a été introduit dans la version 3 du protocole OData. Pour la compatibilité descendante, un client peut demander l’ancien format JSON « Verbose ». Pour demander un code JSON détaillé, définissez l’en-tête Accept sur `application/json;odata=verbose`. Voici la version détaillée :

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

Ce format achemine davantage de métadonnées dans le corps de la réponse, ce qui peut ajouter une surcharge considérable sur toute une session. En outre, il ajoute un niveau d’indirection en encapsulant l’objet dans une propriété nommée « d ».

## <a name="next-steps"></a>Étapes suivantes

- [Ajouter des relations d’entité](working-with-entity-relations.md)
- [Ajouter des actions OData](odata-actions.md)
- [Appeler le service OData à partir d’un client .NET](calling-an-odata-service-from-a-net-client.md)
