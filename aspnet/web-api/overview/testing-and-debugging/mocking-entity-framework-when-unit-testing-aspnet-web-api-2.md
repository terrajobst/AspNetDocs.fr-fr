---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Simulation d’Entity Framework lors du test unitaire API Web ASP.NET 2 | Microsoft Docs
author: Rick-Anderson
description: Ce guide et cette application montrent comment créer des tests unitaires pour votre application Web API 2 qui utilise l’Entity Framework. Il montre comment modifier le...
ms.author: riande
ms.date: 12/13/2013
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 258450107ee7443c4efd43a3b8e4851249745227
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555089"
---
# <a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a>Simulation d’Entity Framework lors du test unitaire API Web ASP.NET 2

par [Tom FitzMacken](https://github.com/tfitzmac)

[Télécharger le projet terminé](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> Ce guide et cette application montrent comment créer des tests unitaires pour votre application Web API 2 qui utilise l’Entity Framework. Il montre comment modifier le contrôleur de génération de modèles automatique pour permettre le passage d’un objet de contexte à des fins de test, et comment créer des objets de test qui fonctionnent avec Entity Framework.
>
> Pour une introduction aux tests unitaires avec API Web ASP.NET, consultez [tests unitaires avec API Web ASP.NET 2](unit-testing-with-aspnet-web-api.md).
>
> Ce didacticiel part du principe que vous êtes familiarisé avec les concepts de base de API Web ASP.NET. Pour obtenir un didacticiel d’introduction, consultez [prise en main avec API Web ASP.NET 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
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
- [Créer la classe de modèle](#modelclass)
- [Ajouter le contrôleur](#controller)
- [Ajouter une injection de dépendances](#dependency)
- [Installer des packages NuGet dans un projet de test](#testpackages)
- [Créer un contexte de test](#testcontext)
- [Créer des tests](#tests)
- [Exécuter les tests](#runtests)

Si vous avez déjà effectué les étapes du [test unitaire avec API Web ASP.NET 2](unit-testing-with-aspnet-web-api.md), vous pouvez passer à la section [Ajouter le contrôleur](#controller).

<a id="prereqs"></a>
## <a name="prerequisites"></a>Conditions préalables requises

Visual Studio 2017, édition Professional ou Enterprise

<a id="download"></a>
## <a name="download-code"></a>Télécharger le code

Téléchargez le [projet terminé](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). Le projet téléchargeable comprend du code de test unitaire pour cette rubrique et la rubrique [test unitaire API Web ASP.NET 2](unit-testing-with-aspnet-web-api.md) .

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Créer une application avec un projet de test unitaire

Vous pouvez créer un projet de test unitaire lors de la création de votre application ou ajouter un projet de test unitaire à une application existante. Ce didacticiel montre comment créer un projet de test unitaire lors de la création de l’application.

Créez une nouvelle application Web ASP.NET nommée **StoreApp**.

Dans les fenêtres nouveau projet ASP.NET, sélectionnez le modèle **vide** et ajoutez des dossiers et des références principales pour l’API Web. Sélectionnez l’option **Ajouter des tests unitaires** . Le projet de test unitaire est automatiquement nommé **StoreApp. tests**. Vous pouvez conserver ce nom.

![créer un projet de test unitaire](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

Après avoir créé l’application, vous verrez qu’elle contient deux projets : **StoreApp** et **StoreApp. tests**.

<a id="modelclass"></a>
## <a name="create-the-model-class"></a>Créer la classe de modèle

Dans votre projet StoreApp, ajoutez un fichier de classe au dossier **Models** nommé **Product.cs**. Remplacez le contenu du fichier par le code suivant.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

Générez la solution.

<a id="controller"></a>
## <a name="add-the-controller"></a>Ajouter le contrôleur

Cliquez avec le bouton droit sur le dossier contrôleurs et sélectionnez **Ajouter** et **nouvel élément de génération de modèles**automatique. Sélectionnez contrôleur d’API Web 2 avec des actions, à l’aide de Entity Framework.

![Ajouter un nouveau contrôleur](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

Définissez les valeurs suivantes :

- Nom du contrôleur : **ProductController**
- Classe de modèle : **produit**
- Classe de contexte de données : [sélectionner **un nouveau bouton de contexte de données** qui renseigne les valeurs affichées ci-dessous]

![spécifier le contrôleur](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

Cliquez sur **Ajouter** pour créer le contrôleur avec le code généré automatiquement. Le code comprend des méthodes pour la création, la récupération, la mise à jour et la suppression d’instances de la classe Product. Le code suivant illustre la méthode d’ajout d’un produit. Notez que la méthode retourne une instance de **IHttpActionResult**.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

IHttpActionResult est l’une des nouvelles fonctionnalités de l’API Web 2 et simplifie le développement de tests unitaires.

Dans la section suivante, vous allez personnaliser le code généré pour faciliter le passage d’objets de test au contrôleur.

<a id="dependency"></a>
## <a name="add-dependency-injection"></a>Ajouter une injection de dépendances

Actuellement, la classe ProductController est codée en dur pour utiliser une instance de la classe StoreAppContext. Vous allez utiliser un modèle appelé injection de dépendances pour modifier votre application et supprimer cette dépendance codée en dur. En rompant cette dépendance, vous pouvez passer un objet fictif lors du test.

Cliquez avec le bouton droit sur le dossier **Models** , puis ajoutez une nouvelle interface nommée **IStoreAppContext**.

Remplacez le code par celui-ci :

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

Ouvrez le fichier StoreAppContext.cs et apportez les modifications suivantes en surbrillance. Les changements importants à noter sont les suivants :

- La classe StoreAppContext implémente l’interface IStoreAppContext
- La méthode MarkAsModified est implémentée

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

Ouvrez le fichier ProductController.cs. Modifiez le code existant pour qu’il corresponde au code mis en surbrillance. Ces modifications rompent la dépendance sur StoreAppContext et permettent à d’autres classes de passer un objet différent pour la classe de contexte. Cette modification vous permettra de passer un contexte de test pendant les tests unitaires.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

Vous devez apporter une autre modification dans ProductController. Dans la méthode **PutProduct** , remplacez la ligne qui définit l’état de l’entité par modifié par un appel à la méthode MarkAsModified.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

Générez la solution.

Vous êtes maintenant prêt à configurer le projet de test.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Installer des packages NuGet dans un projet de test

Quand vous utilisez le modèle vide pour créer une application, le projet de test unitaire (StoreApp. tests) n’inclut aucun package NuGet installé. D’autres modèles, tels que le modèle d’API Web, incluent des packages NuGet dans le projet de test unitaire. Pour ce didacticiel, vous devez inclure le package Entity Framework et le package Microsoft ASP.NET API Web 2 Core au projet de test.

Cliquez avec le bouton droit sur le projet StoreApp. tests, puis sélectionnez **gérer les packages NuGet**. Vous devez sélectionner le projet StoreApp. tests pour ajouter les packages à ce projet.

![gérer les packages](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

À partir des packages en ligne, recherchez et installez le package EntityFramework (version 6,0 ou ultérieure). S’il apparaît que le package EntityFramework est déjà installé, vous avez peut-être sélectionné le projet StoreApp au lieu du projet StoreApp. tests.

![Ajouter Entity Framework](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

Recherchez et installez Microsoft ASP.NET package principal API Web 2.

![installer le package d’API Web Core](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

Fermez la fenêtre gérer les packages NuGet.

<a id="testcontext"></a>
## <a name="create-test-context"></a>Créer un contexte de test

Ajoutez une classe nommée **TestDbSet** au projet de test. Cette classe sert de classe de base pour votre jeu de données de test. Remplacez le code par celui-ci :

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

Ajoutez une classe nommée **TestProductDbSet** au projet de test qui contient le code suivant.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

Ajoutez une classe nommée **TestStoreAppContext** et remplacez le code existant par le code suivant.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a>Créer des tests

Par défaut, votre projet de test contient un fichier de test vide nommé **UnitTest1.cs**. Ce fichier montre les attributs que vous utilisez pour créer des méthodes de test. Pour ce didacticiel, vous pouvez supprimer ce fichier, car vous allez ajouter une nouvelle classe de test.

Ajoutez une classe nommée **TestProductController** au projet de test. Remplacez le code par celui-ci :

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Exécuter les tests

Vous êtes maintenant prêt à exécuter les tests. Toutes les méthodes marquées avec l’attribut **TestMethod** seront testées. À partir de l’élément de menu **test** , exécutez les tests.

![exécuter des tests](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

Ouvrez la fenêtre **Explorateur de tests** et observez les résultats des tests.

![résultats des tests](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
