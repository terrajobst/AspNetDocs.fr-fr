---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'Didacticiel : améliorer la validation des données pour EF Database First avec l’application MVC ASP.NET'
description: Ce didacticiel se concentre sur l’ajout d’annotations de données au modèle de données pour spécifier les exigences de validation et la mise en forme d’affichage.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 897cd7c6a40445e2a4abede50d81e101372d3233
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616276"
---
# <a name="tutorial-enhance-data-validation-for-ef-database-first-with-aspnet-mvc-app"></a>Didacticiel : améliorer la validation des données pour EF Database First avec l’application MVC ASP.NET

À l’aide de la génération de modèles automatique MVC, Entity Framework et ASP.NET, vous pouvez créer une application Web qui fournit une interface à une base de données existante. Cette série de didacticiels vous montre comment générer automatiquement du code qui permet aux utilisateurs d’afficher, de modifier, de créer et de supprimer des données qui se trouvent dans une table de base de données. Le code généré correspond aux colonnes dans la table de base de données.

Ce didacticiel se concentre sur l’ajout d’annotations de données au modèle de données pour spécifier les exigences de validation et la mise en forme d’affichage. Il a été amélioré en fonction des commentaires des utilisateurs figurant dans la section commentaires.

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Ajouter des annotations de données
> * Ajouter des classes de métadonnées

## <a name="prerequisites"></a>Conditions préalables requises

* [Personnaliser un affichage](customizing-a-view.md)

## <a name="add-data-annotations"></a>Ajouter des annotations de données

Comme vous l’avez vu dans une rubrique précédente, certaines règles de validation des données sont automatiquement appliquées à l’entrée utilisateur. Par exemple, vous pouvez uniquement fournir un nombre pour la propriété de niveau. Pour spécifier d’autres règles de validation des données, vous pouvez ajouter des annotations de données à votre classe de modèle. Ces annotations sont appliquées dans l’ensemble de votre application Web pour la propriété spécifiée. Vous pouvez également appliquer des attributs de mise en forme qui modifient la façon dont les propriétés sont affichées. par exemple, la modification de la valeur utilisée pour les étiquettes de texte.

Dans ce didacticiel, vous allez ajouter des annotations de données pour limiter la longueur des valeurs fournies pour les propriétés FirstName, LastName et MiddleName. Dans la base de données, ces valeurs sont limitées à 50 caractères ; Toutefois, dans votre application Web, cette limite de caractères n’est pas appliquée actuellement. Si un utilisateur fournit plus de 50 caractères pour l’une de ces valeurs, la page se bloque lors d’une tentative d’enregistrement de la valeur dans la base de données. Vous allez également limiter les valeurs de niveau aux valeurs comprises entre 0 et 4.

Sélectionnez **modèles** > **ContosoModel. edmx** > **ContosoModel.tt** et ouvrez le fichier *Student.cs* . Ajoutez le code en surbrillance suivant à la classe.

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

Ouvrez *Enrollment.cs* et ajoutez le code en surbrillance suivant.

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

Générez la solution.

Cliquez sur **liste des élèves** , puis sélectionnez **modifier**. Si vous tentez d’entrer plus de 50 caractères, un message d’erreur s’affiche.

![afficher le message d’erreur](enhancing-data-validation/_static/image1.png)

Revenez à la page d’hébergement. Cliquez sur **liste des inscriptions** et sélectionnez **modifier**. Essayez de fournir un niveau supérieur à 4. Vous recevez cette erreur : *le champ grade doit être compris entre 0 et 4.*

## <a name="add-metadata-classes"></a>Ajouter des classes de métadonnées

L’ajout des attributs de validation directement à la classe de modèle fonctionne lorsque vous ne vous attendez pas à ce que la base de données change. Toutefois, si votre base de données est modifiée et que vous devez régénérer la classe de modèle, vous perdez tous les attributs que vous avez appliqués à la classe de modèle. Cette approche peut être très inefficace et sujette à la perte de règles de validation importantes.

Pour éviter ce problème, vous pouvez ajouter une classe de métadonnées qui contient les attributs. Lorsque vous associez la classe de modèle à la classe de métadonnées, ces attributs sont appliqués au modèle. Dans cette approche, la classe de modèle peut être régénérée sans perdre tous les attributs qui ont été appliqués à la classe de métadonnées.

Dans le dossier **Models** , ajoutez une classe nommée *Metadata.cs*.

Remplacez le code dans *Metadata.cs* par le code suivant.

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

Ces classes de métadonnées contiennent tous les attributs de validation que vous aviez précédemment appliqués aux classes de modèle. L’attribut **Display** est utilisé pour modifier la valeur utilisée pour les étiquettes de texte.

À présent, vous devez associer les classes de modèle aux classes de métadonnées.

Dans le dossier **Models** , ajoutez une classe nommée *PartialClasses.cs*.

Remplacez le contenu du fichier par le code suivant.

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

Notez que chaque classe est marquée en tant que classe de `partial`, et que chaque classe correspond au nom et à l’espace de noms en tant que classe générée automatiquement. En appliquant l’attribut de métadonnées à la classe partielle, vous vous assurez que les attributs de validation de données seront appliqués à la classe générée automatiquement. Ces attributs ne seront pas perdus quand vous régénérez les classes de modèle, car l’attribut de métadonnées est appliqué dans les classes partielles qui ne sont pas régénérées.

Pour régénérer les classes générées automatiquement, ouvrez le fichier *ContosoModel. edmx* . Une fois encore, cliquez avec le bouton droit sur l’aire de conception et sélectionnez **mettre à jour le modèle à partir de la base de données**. Même si vous n’avez pas modifié la base de données, ce processus régénère les classes. Sous l’onglet **Actualiser** , sélectionnez **tables** et **Terminer**.

Enregistrez le fichier *ContosoModel. edmx* pour appliquer les modifications.

Ouvrez le fichier *Student.cs* ou le fichier *Enrollment.cs* , et notez que les attributs de validation de données que vous avez appliqués précédemment ne sont plus dans le fichier. Toutefois, exécutez l’application et notez que les règles de validation sont toujours appliquées lorsque vous entrez des données.

## <a name="conclusion"></a>Conclusion

Cette série a fourni un exemple simple de génération de code à partir d’une base de données existante qui permet aux utilisateurs de modifier, de mettre à jour, de créer et de supprimer des données. Il a utilisé ASP.NET MVC 5, Entity Framework et la génération de modèles automatique ASP.NET pour créer le projet. 

Pour obtenir un exemple d’introduction de Code First développement, consultez [prise en main avec ASP.NET MVC 5](../introduction/getting-started.md). 

Pour obtenir un exemple plus avancé, consultez [création d’un modèle de données Entity Framework pour une application ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Notez que l’API DbContext que vous utilisez pour utiliser les données dans Database First est identique à l’API que vous utilisez pour utiliser les données dans Code First. Même si vous envisagez d’utiliser Database First, vous pouvez apprendre à gérer des scénarios plus complexes, tels que la lecture et la mise à jour de données associées, la gestion des conflits d’accès concurrentiel, etc. à partir d’un didacticiel de Code First. La seule différence réside dans la façon dont la base de données, la classe de contexte et les classes d’entité sont créées.

## <a name="additional-resources"></a>Ressources supplémentaires

Pour obtenir la liste complète des annotations de validation des données que vous pouvez appliquer aux propriétés et aux classes, consultez [System. ComponentModel. DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Ajout d’annotations de données
> * Classes de métadonnées ajoutées

Pour savoir comment déployer une application Web et une base de données SQL pour Azure App Service, consultez ce didacticiel :
> [!div class="nextstepaction"]
> [Déployer une application .NET sur Azure App Service](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase/)
