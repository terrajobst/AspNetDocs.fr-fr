---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: Ajouter des modèles et des contrôleurs | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: 57dacda421968f341284d89c9a3ad80040c16e25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557539"
---
# <a name="add-models-and-controllers"></a>Ajouter des modèles et des contrôleurs

par [Mike Wasson](https://github.com/MikeWasson)

[Télécharger le projet terminé](https://github.com/MikeWasson/BookService)

Dans cette section, vous allez ajouter des classes de modèle qui définissent les entités de base de données. Ensuite, vous allez ajouter des contrôleurs d’API Web qui effectuent des opérations CRUD sur ces entités.

## <a name="add-model-classes"></a>Ajouter des classes de modèle

Dans ce didacticiel, nous allons créer la base de données à l’aide de l’approche « Code First » de Entity Framework (EF). Avec Code First, vous écrivez C# des classes qui correspondent à des tables de base de données et EF crée la base de données. (Pour plus d’informations, consultez [Entity Framework approches de développement](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)

Nous commençons par définir nos objets de domaine en tant qu’objets POCO (Plain-Old CLR Objects). Nous allons créer les POCO suivants :

- Auteur
- Book

Dans Explorateur de solutions, cliquez avec le bouton droit sur le dossier modèles. Sélectionnez **Ajouter**, puis **classe**. Nommez la classe `Author`.

![](part-2/_static/image1.png)

Remplacez tout le code réutilisable dans Author.cs par le code suivant.

[!code-csharp[Main](part-2/samples/sample1.cs)]

Ajoutez une autre classe nommée `Book`, avec le code suivant.

[!code-csharp[Main](part-2/samples/sample2.cs)]

Entity Framework utilisera ces modèles pour créer des tables de base de données. Pour chaque modèle, la propriété `Id` deviendra la colonne de clé primaire de la table de base de données.

Dans la classe Book, le `AuthorId` définit une clé étrangère dans la table `Author`. (Par souci de simplicité, je suppose que chaque livre a un seul auteur.) La classe Book contient également une propriété de navigation pour la `Author`associée. Vous pouvez utiliser la propriété de navigation pour accéder aux `Author` connexes dans le code. Je dis plus d’informations sur les propriétés de navigation dans la partie 4, [gestion des relations d’entité](part-4.md).

## <a name="add-web-api-controllers"></a>Ajouter des contrôleurs d’API Web

Dans cette section, nous allons ajouter des contrôleurs d’API Web qui prennent en charge les opérations CRUD (création, lecture, mise à jour et suppression). Les contrôleurs utilisent Entity Framework pour communiquer avec la couche de base de données.

Tout d’abord, vous pouvez supprimer le fichier Controllers/ValuesController. cs. Ce fichier contient un exemple de contrôleur d’API Web, mais vous n’en avez pas besoin pour ce didacticiel.

![](part-2/_static/image2.png)

Ensuite, générez le projet. La génération de modèles automatique d’API Web utilise la réflexion pour rechercher les classes de modèle. elle a donc besoin de l’assembly compilé.

Dans Explorateur de solutions, cliquez avec le bouton droit sur le dossier Controllers. Sélectionnez **Ajouter**, puis **contrôleur**.

![](part-2/_static/image3.png)

Dans la boîte de dialogue **Ajouter une structure** , sélectionnez « contrôleur d’API Web 2 avec actions, à l’aide de Entity Framework ». Cliquez sur **Ajouter**.

![](part-2/_static/image4.png)

Dans la boîte de dialogue **Ajouter un contrôleur** , procédez comme suit :

1. Dans la liste déroulante **classe de modèle** , sélectionnez la classe `Author`. (Si vous ne le voyez pas dans la liste déroulante, assurez-vous que vous avez créé le projet.)
2. Cochez la case « utiliser les actions du contrôleur Async ».
3. Laissez le nom du contrôleur &quot;&quot;AuthorsController.
4. Cliquez sur le bouton plus (+) en regard de **classe du contexte de données**.

![](part-2/_static/image5.png)

Dans la boîte de dialogue **nouveau contexte de données** , laissez le nom par défaut et cliquez sur **Ajouter**.

![](part-2/_static/image6.png)

Cliquez sur **Ajouter** pour terminer la boîte de dialogue **Ajouter un contrôleur** . La boîte de dialogue ajoute deux classes à votre projet :

- `AuthorsController` définit un contrôleur d’API Web. Le contrôleur implémente l’API REST utilisée par les clients pour effectuer des opérations CRUD sur la liste des auteurs.
- `BookServiceContext` gère les objets d’entité au moment de l’exécution, ce qui comprend le remplissage des objets avec les données d’une base de données, le suivi des modifications et la persistance des données dans la base de données. Hérite de `DbContext`.

![](part-2/_static/image7.png)

À ce stade, régénérez le projet. À présent, suivez les mêmes étapes pour ajouter un contrôleur d’API pour les entités `Book`. Cette fois, sélectionnez `Book` pour la classe de modèle, puis sélectionnez la classe de `BookServiceContext` existante pour la classe de contexte de données. (Ne créez pas de nouveau contexte de données.) Cliquez sur **Ajouter** pour ajouter le contrôleur.

![](part-2/_static/image8.png)

> [!div class="step-by-step"]
> [Précédent](part-1.md)
> [Suivant](part-3.md)
