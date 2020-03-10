---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'Didacticiel : générer des affichages pour EF Database First avec l’application MVC ASP.NET'
description: Ce didacticiel se concentre sur l’utilisation de la génération de modèles automatique ASP.NET pour générer les contrôleurs et les vues.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: e71e13e22d8a72e1699cfc70d4d93af603edba5b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616206"
---
# <a name="tutorial-generate-views-for-ef-database-first-with-aspnet-mvc-app"></a>Didacticiel : générer des affichages pour EF Database First avec l’application MVC ASP.NET

À l’aide de la génération de modèles automatique MVC, Entity Framework et ASP.NET, vous pouvez créer une application Web qui fournit une interface à une base de données existante. Cette série de didacticiels vous montre comment générer automatiquement du code qui permet aux utilisateurs d’afficher, de modifier, de créer et de supprimer des données qui se trouvent dans une table de base de données. Le code généré correspond aux colonnes dans la table de base de données.

Ce didacticiel se concentre sur l’utilisation de la génération de modèles automatique ASP.NET pour générer les contrôleurs et les vues.

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Ajouter une structure
> * Ajouter des liens vers de nouvelles vues
> * Afficher les vues des élèves
> * Afficher les affichages d’inscription

## <a name="prerequisite"></a>Condition préalable

* [Créer l’application Web et les modèles de données](creating-the-web-application.md)

## <a name="add-scaffold"></a>Ajouter une structure

Vous êtes prêt à générer du code qui fournira des opérations de données standard pour les classes de modèle. Vous ajoutez le code en ajoutant un élément de génération de modèles automatique. Il existe de nombreuses options pour le type de génération de modèles automatique que vous pouvez ajouter. dans ce didacticiel, l’échafaudage inclut un contrôleur et des vues qui correspondent aux modèles d’étudiant et d’inscription que vous avez créés dans la section précédente.

Pour maintenir la cohérence dans votre projet, vous allez ajouter le nouveau contrôleur au dossier **contrôleurs** existants. Cliquez avec le bouton droit sur le dossier **Controllers** , puis sélectionnez **Ajouter** > **nouvel élément de génération de modèles**automatique.

Sélectionnez le **contrôleur MVC 5 avec affichages, à l’aide de l’option Entity Framework** . Cette option génère le contrôleur et les vues pour la mise à jour, la suppression, la création et l’affichage des données dans votre modèle.

![Ajouter un contrôleur MVC](generating-views/_static/image2.png)

Sélectionnez **Student (ContosoSite. Models)** pour la classe de modèle et sélectionnez **ContosoUniversityDataEntities (ContosoSite. Models)** pour la classe de contexte. Conservez le nom du contrôleur en tant que **StudentsController**.

Cliquez sur **Ajouter**.

Si vous recevez une erreur, cela peut être dû au fait que vous n’avez pas généré le projet dans la section précédente. Si c’est le cas, essayez de générer le projet, puis ajoutez de nouveau l’élément de génération de modèles automatique.

Une fois le processus de génération de code terminé, vous verrez un nouveau contrôleur et des vues dans les **contrôleurs** et les **vues** de votre **projet > dossiers** Student.

Effectuez à nouveau les mêmes étapes, mais ajoutez une structure pour la classe d' **inscription** . Lorsque vous avez terminé, vous disposez d’un fichier **EnrollmentsController.cs** et d’un dossier sous **affichages** nommé **inscriptions** avec les vues créer, supprimer, détails, modifier et index.

## <a name="add-links-to-new-views"></a>Ajouter des liens vers de nouvelles vues

Pour faciliter la navigation vers vos nouveaux affichages, vous pouvez ajouter quelques liens hypertexte aux vues d’index pour les élèves et les inscriptions. Ouvrez le fichier dans **affichages** >  > d' *index. cshtml*, qui est la page **d’hébergement de** votre site. Ajoutez le code suivant sous Jumbotron.

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

Pour la méthode ActionLink, le premier paramètre est le texte à afficher dans le lien. Le deuxième paramètre est l’action et le troisième paramètre est le nom du contrôleur. Par exemple, le premier lien pointe vers l’action d’index dans StudentsController. Le lien hypertexte réel est construit à partir de ces valeurs. Le premier lien prend finalement les utilisateurs dans le fichier **index. cshtml** dans le dossier **views/** Student.

## <a name="display-student-views"></a>Afficher les vues des élèves

Vous allez vérifier que le code ajouté à votre projet affiche correctement une liste des étudiants et permet aux utilisateurs de modifier, de créer ou de supprimer les enregistrements des étudiants dans la base de données.

Cliquez avec le bouton droit sur les **vues** > fichier d’accès à **distance** > d' *index. cshtml* , puis sélectionnez **afficher dans le navigateur**. Sur la page d’hébergement de l’application, sélectionnez **liste des élèves**.

![](generating-views/_static/image6.png)

Dans la page **index** , notez la liste des étudiants et des liens pour modifier ces données. Sélectionnez le lien **créer un nouveau** et fournissez des valeurs pour un nouvel étudiant. Cliquez sur **créer**, et notez que le nouvel étudiant est ajouté à votre liste.

De retour sur la page **index** , sélectionnez le lien **modifier** , puis modifiez certaines des valeurs d’un étudiant. Cliquez sur **Enregistrer**et notez que l’enregistrement de l’étudiant a été modifié.

Enfin, sélectionnez le lien **supprimer** et confirmez que vous souhaitez supprimer l’enregistrement en cliquant sur le bouton **supprimer** .

Sans écrire de code, vous avez ajouté des vues qui effectuent des opérations courantes sur les données de la table Student.

Vous avez peut-être remarqué que l’étiquette de texte d’un champ est basée sur la propriété de base de données (par exemple **LastName**) qui n’est pas nécessairement ce que vous souhaitez afficher sur la page Web. Par exemple, vous préférerez peut-être l’étiquette comme **nom de famille**. Vous allez résoudre ce problème d’affichage plus tard dans le didacticiel.

## <a name="display-enrollment-views"></a>Afficher les affichages d’inscription

Votre base de données comprend une relation un-à-plusieurs entre les tables des étudiants et les inscriptions, et une relation un-à-plusieurs entre les tables de cours et d’inscription. Les vues de l’inscription gèrent correctement ces relations. Accédez à la page d’hébergement de votre site, puis sélectionnez le lien **liste des inscriptions** , puis **créer un** lien.

La vue affiche un formulaire pour la création d’un nouvel enregistrement d’inscription. En particulier, Notez que le formulaire contient une liste déroulante **CourseID** et une liste déroulante **StudentID** . Les deux sont renseignés avec les valeurs des tables associées.

En outre, la validation des valeurs fournies est appliquée automatiquement en fonction du type de données du champ. La **qualité** requiert un nombre. par conséquent, un message d’erreur s’affiche si vous essayez de fournir une valeur incompatible : *le champ grade doit être un nombre.*

Vous avez vérifié que les vues générées automatiquement permettent aux utilisateurs de travailler avec les données de la base de données. Dans le didacticiel suivant de cette série, vous allez mettre à jour la base de données et effectuer les modifications correspondantes dans l’application Web.

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Structure ajoutée
> * Ajout de liens vers de nouvelles vues
> * Affichages des élèves affichés
> * Affichages d’inscription affichés

Passez au didacticiel suivant pour apprendre à modifier la base de données.
> [!div class="nextstepaction"]
> [Modifier la base de données](changing-the-database.md)