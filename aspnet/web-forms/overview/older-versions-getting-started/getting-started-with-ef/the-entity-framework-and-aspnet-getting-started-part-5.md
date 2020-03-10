---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
title: Prise en main avec Entity Framework 4,0 Database First et ASP.NET 4 Web Forms-part 5 | Microsoft Docs
author: tdykstra
description: L’exemple d’application Web Contoso University montre comment créer des applications de Web Forms ASP.NET à l’aide de l’Entity Framework. L’exemple d’application est...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: 24ad4379-3fb2-44dc-ba59-85fe0ffcb2ae
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
msc.type: authoredcontent
ms.openlocfilehash: 75328e67abb4295b619cac5423a9eb970942fff7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78527999"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-5"></a>Prise en main avec Entity Framework 4,0 Database First et ASP.NET 4 Web Forms-part 5

par [Tom Dykstra](https://github.com/tdykstra)

> L’exemple d’application Web Contoso University montre comment créer des applications ASP.NET Web Forms à l’aide de la Entity Framework 4,0 et de Visual Studio 2010. Pour plus d’informations sur la série de didacticiels, consultez [le premier didacticiel de la série](the-entity-framework-and-aspnet-getting-started-part-1.md)

## <a name="working-with-related-data-continued"></a>Utilisation des données associées, suite

Dans le didacticiel précédent, vous avez commencé à utiliser le contrôle `EntityDataSource` pour travailler avec des données associées. Vous avez affiché plusieurs niveaux de hiérarchie et de données modifiées dans les propriétés de navigation. Dans ce didacticiel, vous allez continuer à utiliser les données associées en ajoutant et en supprimant des relations, et en ajoutant une nouvelle entité qui a une relation à une entité existante.

Vous allez créer une page qui ajoute des cours qui sont affectés à des départements. Les services existent déjà et, lorsque vous créez un nouveau cours, vous établissez une relation entre celui-ci et un service existant.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image1.png)

Vous allez également créer une page qui fonctionne avec une relation plusieurs-à-plusieurs en affectant un formateur à un cours (en ajoutant une relation entre deux entités que vous sélectionnez) ou en supprimant un formateur d’un cours (en supprimant une relation entre deux entités que vous Sélectionnez). Dans la base de données, l’ajout d’une relation entre un formateur et un cours entraîne l’ajout d’une nouvelle ligne à la table d’association `CourseInstructor` ; la suppression d’une relation implique la suppression d’une ligne de la table d’association `CourseInstructor`. Toutefois, vous le faites dans la Entity Framework en définissant les propriétés de navigation, sans faire référence explicitement à la table `CourseInstructor`.

[![image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image3.png)

## <a name="adding-an-entity-with-a-relationship-to-an-existing-entity"></a>Ajout d’une entité avec une relation à une entité existante

Créez une nouvelle page Web nommée *CoursesAdd. aspx* qui utilise la page maître *site. Master* , puis ajoutez le balisage suivant au contrôle `Content` nommé `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample1.aspx)]

Ce balisage crée un contrôle `EntityDataSource` qui sélectionne des cours, qui active l’insertion et qui spécifie un gestionnaire pour l’événement `Inserting`. Vous utiliserez le gestionnaire pour mettre à jour la propriété de navigation `Department` lors de la création d’une nouvelle entité `Course`.

Le balisage crée également un contrôle `DetailsView` à utiliser pour l’ajout de nouvelles entités `Course`. Le balisage utilise des champs liés pour `Course` propriétés d’entité. Vous devez entrer la valeur de `CourseID`, car il ne s’agit pas d’un champ d’ID généré par le système. Au lieu de cela, il s’agit d’un numéro de cours qui doit être spécifié manuellement lors de la création du cours.

Vous utilisez un champ de modèle pour la propriété de navigation `Department`, car les propriétés de navigation ne peuvent pas être utilisées avec des contrôles `BoundField`. Le champ modèle fournit une liste déroulante pour sélectionner le service. La liste déroulante est liée au jeu d’entités `Departments` à l’aide de `Eval` plutôt que `Bind`, car vous ne pouvez pas lier directement les propriétés de navigation pour les mettre à jour. Vous spécifiez un gestionnaire pour l’événement `Init` du contrôle `DropDownList`, afin de pouvoir stocker une référence au contrôle en vue d’une utilisation par le code qui met à jour la clé étrangère `DepartmentID`.

Dans *CoursesAdd.aspx.cs* juste après la déclaration de classe partielle, ajoutez un champ de classe pour contenir une référence au contrôle `DepartmentsDropDownList` :

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample2.cs)]

Ajoutez un gestionnaire pour l’événement `Init` du contrôle `DepartmentsDropDownList`, afin de pouvoir stocker une référence au contrôle. Cela vous permet d’accéder à la valeur entrée par l’utilisateur et de l’utiliser pour mettre à jour la valeur `DepartmentID` de l’entité `Course`.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample3.cs)]

Ajoutez un gestionnaire pour l’événement `Inserting` du contrôle `DetailsView` :

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample4.cs)]

Lorsque l’utilisateur clique sur `Insert`, l’événement `Inserting` est déclenché avant l’insertion du nouvel enregistrement. Le code dans le gestionnaire obtient le `DepartmentID` à partir du contrôle `DropDownList` et l’utilise pour définir la valeur qui sera utilisée pour la propriété `DepartmentID` de l’entité `Course`.

Le Entity Framework s’occupera de l’ajout de ce cours à la propriété de navigation `Courses` de l’entité `Department` associée. Elle ajoute également le département à la propriété de navigation `Department` de l’entité `Course`.

Exécutez la page.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image5.png)

Entrez un ID, un titre, un nombre de crédits, puis sélectionnez un service, puis cliquez sur **Insérer**.

Exécutez la page *courses. aspx* et sélectionnez le même service pour afficher le nouveau cours.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-5/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image7.png)

## <a name="working-with-many-to-many-relationships"></a>Utilisation des relations plusieurs-à-plusieurs

La relation entre le jeu d’entités `Courses` et le jeu d’entités `People` est une relation plusieurs-à-plusieurs. Une entité de `Course` a une propriété de navigation nommée `People` qui peut contenir zéro, une ou plusieurs entités de `Person` associées (représentant les instructeurs affectés pour enseigner ce cours). Et une entité `Person` a une propriété de navigation nommée `Courses` qui peut contenir zéro, une ou plusieurs entités `Course` associées (représentant des cours que l’instructeur est affecté à Teach). Un formateur peut enseigner plusieurs cours et un cours peut être enseigné par plusieurs instructeurs. Dans cette section de la procédure pas à pas, vous allez ajouter et supprimer des relations entre les entités `Person` et `Course` en mettant à jour les propriétés de navigation des entités associées.

Créez une nouvelle page Web nommée *InstructorsCourses. aspx* qui utilise la page maître *site. Master* , puis ajoutez le balisage suivant au contrôle `Content` nommé `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample5.aspx)]

Ce balisage crée un contrôle `EntityDataSource` qui récupère le nom et le `PersonID` des entités `Person` pour les formateurs. Un contrôle `DropDrownList` est lié au contrôle `EntityDataSource`. Le contrôle `DropDownList` spécifie un gestionnaire pour l’événement `DataBound`. Vous allez utiliser ce gestionnaire pour lier les deux listes déroulantes qui affichent des cours.

Le balisage crée également le groupe de contrôles suivant à utiliser pour assigner un cours à l’instructeur sélectionné :

- Contrôle de `DropDownList` pour la sélection d’un cours à assigner. Ce contrôle sera rempli avec les cours qui ne sont pas actuellement attribués à l’instructeur sélectionné.
- Contrôle `Button` pour initialiser l’assignation.
- Contrôle `Label` pour afficher un message d’erreur en cas d’échec de l’assignation.

Enfin, le balisage crée également un groupe de contrôles à utiliser pour supprimer un cours de l’instructeur sélectionné.

Dans *InstructorsCourses.aspx.cs*, ajoutez une instruction using :

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample6.cs)]

Ajoutez une méthode pour remplir les deux listes déroulantes qui affichent les cours :

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample7.cs)]

Ce code permet d’obtenir tous les cours du jeu d’entités `Courses` et d’obtenir les cours de la `Courses` propriété de navigation de l’entité `Person` pour l’instructeur sélectionné. Il détermine ensuite les cours qui sont affectés à cet instructeur et remplit les listes déroulantes en conséquence.

Ajoutez un gestionnaire pour l’événement `Click` du bouton `Assign` :

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample8.cs)]

Ce code obtient l’entité `Person` pour l’instructeur sélectionné, obtient l’entité `Course` pour le cours sélectionné et ajoute le cours sélectionné à la propriété de navigation `Courses` de l’entité `Person` de l’instructeur. Il enregistre ensuite les modifications dans la base de données et remplit à nouveau les listes déroulantes afin que les résultats puissent être consultés immédiatement.

Ajoutez un gestionnaire pour l’événement `Click` du bouton `Remove` :

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample9.cs)]

Ce code obtient l’entité `Person` pour l’instructeur sélectionné, obtient l’entité `Course` pour le cours sélectionné et supprime le cours sélectionné de la propriété de navigation `Courses` de l’entité `Person`. Il enregistre ensuite les modifications dans la base de données et remplit à nouveau les listes déroulantes afin que les résultats puissent être consultés immédiatement.

Ajoutez du code à la méthode `Page_Load` qui permet de s’assurer que les messages d’erreur ne sont pas visibles lorsqu’il n’y a pas d’erreur à signaler, et ajoutez des gestionnaires pour les événements `DataBound` et `SelectedIndexChanged` de la liste déroulante des instructeurs pour remplir les listes déroulantes des cours :

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample10.cs)]

Exécutez la page.

[![image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image9.png)

Sélectionnez un formateur. La liste déroulante <strong>attribuer un cours</strong> affiche les cours que l’instructeur n’enseigne pas, et la liste déroulante <strong>supprimer un cours</strong> affiche les cours auxquels l’instructeur est déjà affecté. Dans la section <strong>affecter un cours</strong> , sélectionnez un cours, puis cliquez sur <strong>attribuer</strong>. Le cours passe à la liste déroulante <strong>supprimer un cours</strong> . Sélectionnez un cours dans la section <strong>supprimer un cours</strong> , puis cliquez sur <strong>supprimer</strong><em>.</em> Le cours passe à la liste déroulante <strong>affecter un cours</strong> .

Vous avez maintenant vu d’autres façons d’utiliser les données associées. Dans le didacticiel suivant, vous allez apprendre à utiliser l’héritage dans le modèle de données pour améliorer la maintenabilité de votre application.

> [!div class="step-by-step"]
> [Précédent](the-entity-framework-and-aspnet-getting-started-part-4.md)
> [Suivant](the-entity-framework-and-aspnet-getting-started-part-6.md)
