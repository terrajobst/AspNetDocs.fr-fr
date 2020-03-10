---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'Didacticiel : personnaliser l’affichage des Database First EF avec l’application MVC ASP.NET'
description: Ce didacticiel se concentre sur la modification des vues générées automatiquement pour améliorer la présentation.
author: Rick-Anderson
ms.author: riande
ms.date: 01/24/2019
ms.topic: tutorial
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 89b8a0eb84b6e287c45bc141c68a2c76e63b0e41
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583593"
---
# <a name="tutorial-customize-view-for-ef-database-first-with-aspnet-mvc-app"></a>Didacticiel : personnaliser l’affichage des Database First EF avec l’application MVC ASP.NET

À l’aide de la génération de modèles automatique MVC, Entity Framework et ASP.NET, vous pouvez créer une application Web qui fournit une interface à une base de données existante. Cette série de didacticiels vous montre comment générer automatiquement du code qui permet aux utilisateurs d’afficher, de modifier, de créer et de supprimer des données qui se trouvent dans une table de base de données. Le code généré correspond aux colonnes dans la table de base de données.

Ce didacticiel se concentre sur la modification des vues générées automatiquement pour améliorer la présentation.

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Ajouter des cours à la page de détails de l’étudiant
> * Confirmer l’ajout des cours à la page

## <a name="prerequisites"></a>Conditions préalables requises

* [Modifier la base de données](changing-the-database.md)

## <a name="add-courses-to-student-detail"></a>Ajouter des cours aux détails de l’étudiant

Le code généré fournit un bon point de départ pour votre application, mais elle ne fournit pas nécessairement toutes les fonctionnalités dont vous avez besoin dans votre application. Vous pouvez personnaliser le code pour répondre aux besoins particuliers de votre application. Actuellement, votre application n’affiche pas les cours inscrits pour l’étudiant sélectionné. Dans cette section, vous allez ajouter les cours inscrits pour chaque étudiant à la vue **Détails** de l’étudiant.

Ouvrez **vues** > **étudiants** > *Details. cshtml*. Sous la dernière balise &lt;/DL&gt;, mais avant la balise de fin &lt;/div&gt;, ajoutez le code suivant.

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

Ce code crée une table qui affiche une ligne pour chaque enregistrement de la table d’inscription pour l’étudiant sélectionné. La méthode **Display** restitue le code HTML de l’objet (ModelItem) qui représente l’expression. Vous utilisez la méthode d’affichage (au lieu d’incorporer simplement la valeur de la propriété dans le code) pour vous assurer que la valeur est correctement mise en forme en fonction de son type et du modèle pour ce type. Dans cet exemple, chaque expression retourne une propriété unique à partir de l’enregistrement en cours dans la boucle, et les valeurs sont des types primitifs qui sont rendus sous forme de texte.

## <a name="confirm-courses-are-added"></a>Confirmer l’ajout des cours

Exécutez la solution. Cliquez sur **liste des élèves** et sélectionnez les **Détails** de l’un des étudiants. Vous verrez que les cours inscrits ont été inclus dans la vue.

![étudiant avec inscription](customizing-a-view/_static/image1.png)

## <a name="next-steps"></a>Étapes suivantes
Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Ajout de cours à la page de détails de l’étudiant
> * Confirmation de l’ajout des cours à la page

Passez au didacticiel suivant pour apprendre à ajouter des annotations de données pour spécifier les exigences de validation et la mise en forme d’affichage.
> [!div class="nextstepaction"]
> [Améliorer la validation des données](enhancing-data-validation.md)
