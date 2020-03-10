---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: Test unitaire API Web ASP.NET 2 | Microsoft Docs
author: Rick-Anderson
description: Ce guide et cette application montrent comment créer des tests unitaires simples pour votre application Web API 2. Ce didacticiel montre comment inclure un proj test unitaire...
ms.author: riande
ms.date: 06/05/2014
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: f2d60b977475e048a3a74aabff4adc768ee22baf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554970"
---
# <a name="unit-testing-aspnet-web-api-2"></a>Tests unitaires API Web ASP.NET 2

par [Tom FitzMacken](https://github.com/tfitzmac)

[Télécharger le projet terminé](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> Ce guide et cette application montrent comment créer des tests unitaires simples pour votre application Web API 2. Ce didacticiel montre comment inclure un projet de test unitaire dans votre solution et écrire des méthodes de test qui vérifient les valeurs retournées à partir d’une méthode de contrôleur.
>
> Ce didacticiel part du principe que vous êtes familiarisé avec les concepts de base de API Web ASP.NET. Pour obtenir un didacticiel d’introduction, consultez [prise en main avec API Web ASP.NET 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
>
> Les tests unitaires de cette rubrique sont intentionnellement limités aux scénarios de données simples. Pour les tests unitaires de scénarios de données plus avancés, consultez [simulation d’Entity Framework lors du test unitaire API Web ASP.NET 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - API Web 2

## <a name="in-this-topic"></a>Dans cette rubrique

Cette rubrique contient les sections suivantes :

- [Conditions préalables](#prereqs)
- [Télécharger le code](#download)
- [Créer une application avec un projet de test unitaire](#appwithunittest)
    - [Ajouter un projet de test unitaire lors de la création de l’application](#whencreate)
    - [Ajouter un projet de test unitaire à une application existante](#addtoexisting)
- [Configurer l’application Web API 2](#setupproject)
- [Installer des packages NuGet dans un projet de test](#testpackages)
- [Créer des tests](#tests)
- [Exécuter les tests](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a>Conditions préalables requises

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional ou Enterprise Edition

<a id="download"></a>
## <a name="download-code"></a>Télécharger le code

Téléchargez le [projet terminé](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). Le projet téléchargeable comprend du code de test unitaire pour cette rubrique et pour les [Entity Framework factices lorsque les tests unitaires API Web ASP.net](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) rubrique.

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Créer une application avec un projet de test unitaire

Vous pouvez créer un projet de test unitaire lors de la création de votre application ou ajouter un projet de test unitaire à une application existante. Ce didacticiel présente les deux méthodes de création d’un projet de test unitaire. Pour suivre ce didacticiel, vous pouvez utiliser l’une ou l’autre des approches.

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a>Ajouter un projet de test unitaire lors de la création de l’application

Créez une nouvelle application Web ASP.NET nommée **StoreApp**.

![créer un projet](unit-testing-with-aspnet-web-api/_static/image1.png)

Dans les fenêtres nouveau projet ASP.NET, sélectionnez le modèle **vide** et ajoutez des dossiers et des références principales pour l’API Web. Sélectionnez l’option **Ajouter des tests unitaires** . Le projet de test unitaire est automatiquement nommé **StoreApp. tests**. Vous pouvez conserver ce nom.

![créer un projet de test unitaire](unit-testing-with-aspnet-web-api/_static/image2.png)

Après avoir créé l’application, vous verrez qu’elle contient deux projets.

![deux projets](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a>Ajouter un projet de test unitaire à une application existante

Si vous n’avez pas créé le projet de test unitaire quand vous avez créé votre application, vous pouvez en ajouter un à tout moment. Par exemple, supposons que vous disposez déjà d’une application nommée StoreApp et que vous souhaitez ajouter des tests unitaires. Pour ajouter un projet de test unitaire, cliquez avec le bouton droit sur votre solution, puis sélectionnez **Ajouter** et **nouveau projet**.

![Ajouter un nouveau projet à la solution](unit-testing-with-aspnet-web-api/_static/image4.png)

Sélectionnez **test** dans le volet gauche, puis sélectionnez **projet de test unitaire** pour le type de projet. Nommez le projet **StoreApp. tests**.

![Ajouter un projet de test unitaire](unit-testing-with-aspnet-web-api/_static/image5.png)

Vous verrez le projet de test unitaire dans votre solution.

Dans le projet de test unitaire, ajoutez une référence de projet au projet d’origine.

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a>Configurer l’application Web API 2

Dans votre projet StoreApp, ajoutez un fichier de classe au dossier **Models** nommé **Product.cs**. Remplacez le contenu du fichier par le code suivant.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

Générez la solution.

Cliquez avec le bouton droit sur le dossier contrôleurs et sélectionnez **Ajouter** et **nouvel élément de génération de modèles**automatique. Sélectionnez **contrôleur d’API Web 2-vide**.

![Ajouter un nouveau contrôleur](unit-testing-with-aspnet-web-api/_static/image6.png)

Définissez le nom du contrôleur sur **SimpleProductController**, puis cliquez sur **Ajouter**.

![spécifier le contrôleur](unit-testing-with-aspnet-web-api/_static/image7.png)

Remplacez le code existant par le code ci-dessous. Pour simplifier cet exemple, les données sont stockées dans une liste plutôt que dans une base de données. La liste définie dans cette classe représente les données de production. Notez que le contrôleur comprend un constructeur qui prend comme paramètre une liste d’objets Product. Ce constructeur vous permet de passer des données de test lors des tests unitaires. Le contrôleur comprend également deux méthodes **Async** pour illustrer des méthodes asynchrones de test unitaire. Ces méthodes Async ont été implémentées en appelant **Task. FromResult** pour réduire le code superflu, mais normalement les méthodes incluent des opérations gourmandes en ressources.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

La méthode GetProduct retourne une instance de l’interface **IHttpActionResult** . IHttpActionResult est l’une des nouvelles fonctionnalités de l’API Web 2 et simplifie le développement de tests unitaires. Les classes qui implémentent l’interface IHttpActionResult se trouvent dans l’espace de noms [System. Web. http. Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) . Ces classes représentent les réponses possibles d’une demande d’action et correspondent à des codes d’état HTTP.

Générez la solution.

Vous êtes maintenant prêt à configurer le projet de test.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Installer des packages NuGet dans un projet de test

Quand vous utilisez le modèle vide pour créer une application, le projet de test unitaire (StoreApp. tests) n’inclut aucun package NuGet installé. D’autres modèles, tels que le modèle d’API Web, incluent des packages NuGet dans le projet de test unitaire. Pour ce didacticiel, vous devez inclure le package Microsoft ASP.NET Web API 2 Core au projet de test.

Cliquez avec le bouton droit sur le projet StoreApp. tests, puis sélectionnez **gérer les packages NuGet**. Vous devez sélectionner le projet StoreApp. tests pour ajouter les packages à ce projet.

![gérer les packages](unit-testing-with-aspnet-web-api/_static/image8.png)

Recherchez et installez Microsoft ASP.NET package principal API Web 2.

![installer le package d’API Web Core](unit-testing-with-aspnet-web-api/_static/image9.png)

Fermez la fenêtre gérer les packages NuGet.

<a id="tests"></a>
## <a name="create-tests"></a>Créer des tests

Par défaut, votre projet de test contient un fichier de test vide nommé UnitTest1.cs. Ce fichier montre les attributs que vous utilisez pour créer des méthodes de test. Pour vos tests unitaires, vous pouvez utiliser ce fichier ou créer votre propre fichier.

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

Pour ce didacticiel, vous allez créer votre propre classe de test. Vous pouvez supprimer le fichier UnitTest1.cs. Ajoutez une classe nommée **TestSimpleProductController.cs**et remplacez le code par le code suivant.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Exécuter les tests

Vous êtes maintenant prêt à exécuter les tests. Toutes les méthodes marquées avec l’attribut **TestMethod** seront testées. À partir de l’élément de menu **test** , exécutez les tests.

![exécuter des tests](unit-testing-with-aspnet-web-api/_static/image11.png)

Ouvrez la fenêtre **Explorateur de tests** et observez les résultats des tests.

![résultats des tests](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a>Récapitulatif

Vous avez terminé ce didacticiel. Les données de ce didacticiel ont été intentionnellement simplifiées pour se concentrer sur les conditions de test unitaire. Pour les tests unitaires de scénarios de données plus avancés, consultez [simulation d’Entity Framework lors du test unitaire API Web ASP.NET 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
