---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'Didacticiel : modifier la base de données pour le Database First EF avec l’application MVC ASP.NET'
description: Ce didacticiel se concentre sur la mise à jour de la structure de la base de données et sur la propagation de cette modification dans l’application Web.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 52cad1120908cf0d4f85770f8e2690f9415c5f56
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616262"
---
# <a name="tutorial-change-the-database-for-ef-database-first-with-aspnet-mvc-app"></a>Didacticiel : modifier la base de données pour le Database First EF avec l’application MVC ASP.NET

À l’aide de la génération de modèles automatique MVC, Entity Framework et ASP.NET, vous pouvez créer une application Web qui fournit une interface à une base de données existante. Cette série de didacticiels vous montre comment générer automatiquement du code qui permet aux utilisateurs d’afficher, de modifier, de créer et de supprimer des données qui se trouvent dans une table de base de données. Le code généré correspond aux colonnes dans la table de base de données.

Ce didacticiel se concentre sur la mise à jour de la structure de la base de données et sur la propagation de cette modification dans l’application Web.

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Ajouter une colonne
> * Ajouter la propriété aux vues

## <a name="prerequisites"></a>Conditions préalables requises

* [Génération de vues](generating-views.md)

## <a name="add-a-column"></a>Ajouter une colonne

Si vous mettez à jour la structure d’une table dans votre base de données, vous devez vous assurer que votre modification est propagée vers le modèle de données, les vues et le contrôleur.

Pour ce didacticiel, vous allez ajouter une nouvelle colonne à la table Student pour enregistrer le deuxième prénom de l’étudiant. Pour ajouter cette colonne, ouvrez le projet de base de données, puis ouvrez le fichier Student. Sql. Par le biais du concepteur ou du code T-SQL, ajoutez une colonne nommée **MiddleName** qui est de type nvarchar (50) et qui autorise les valeurs NULL.

Déployez cette modification dans votre base de données locale en démarrant votre projet de base de données (ou F5). Le nouveau champ est ajouté à la table. Si vous ne le voyez pas dans le Explorateur d’objets SQL Server, cliquez sur le bouton Actualiser dans le volet.

![afficher la nouvelle colonne](changing-the-database/_static/image2.png)

La nouvelle colonne existe dans la table de base de données, mais elle n’existe pas actuellement dans la classe de modèle de données. Vous devez mettre à jour le modèle pour inclure votre nouvelle colonne. Dans le dossier **Models** , ouvrez le fichier **ContosoModel. edmx** pour afficher le diagramme de modèle. Notez que le modèle Student ne contient pas la propriété MiddleName. Cliquez avec le bouton droit n’importe où sur l’aire de conception, puis sélectionnez **mettre à jour le modèle à partir de la base de données**.

Dans l’Assistant Mise à jour, sélectionnez l’onglet **Actualiser** , puis sélectionnez **Tables** > **dbo** > **Student**. Cliquez sur **Finish**.

Une fois le processus de mise à jour terminé, le schéma de base de données comprend la nouvelle propriété **MiddleName** . Enregistrez le fichier **ContosoModel. edmx** . Vous devez enregistrer ce fichier pour la nouvelle propriété à propager vers la classe **Student.cs** . Vous avez maintenant mis à jour la base de données et le modèle.

Générez la solution.

## <a name="add-the-property-to-the-views"></a>Ajouter la propriété aux vues

Malheureusement, les vues ne contiennent toujours pas la nouvelle propriété. Pour mettre à jour les vues, vous avez deux options : vous pouvez soit régénérer les vues en ajoutant une nouvelle fois la génération de modèles automatique pour la classe Student, soit ajouter manuellement la nouvelle propriété à vos vues existantes. Dans ce didacticiel, vous allez rajouter la génération de modèles automatique, car vous n’avez pas apporté de modifications personnalisées aux vues générées automatiquement. Vous pouvez envisager d’ajouter manuellement la propriété lorsque vous avez apporté des modifications aux vues et que vous ne souhaitez pas perdre ces modifications.

Pour vous assurer **que les vues** sont recréées, supprimez le dossier Student sous **views**, puis supprimez **StudentsController**. Ensuite, cliquez avec le bouton droit sur le dossier **Controllers** et ajoutez la génération de modèles automatique pour le modèle **Student** . Là encore, nommez le contrôleur **StudentsController**. Sélectionnez **Ajouter** .

Regénérez la solution. Les vues contiennent maintenant la propriété MiddleName.

![afficher le deuxième prénom](changing-the-database/_static/image5.png)

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Ajout d’une colonne
> * Ajout de la propriété aux vues

Passez au didacticiel suivant pour découvrir comment personnaliser l’affichage pour afficher des détails sur un enregistrement d’étudiant.
> [!div class="nextstepaction"]
> [Personnaliser un affichage](customizing-a-view.md)