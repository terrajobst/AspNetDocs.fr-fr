---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Didacticiel : utiliser des procédures stockées et Async avec EF dans une application ASP.NET MVC'
description: Dans ce didacticiel, vous allez découvrir comment implémenter le modèle de programmation asynchrone et comment utiliser des procédures stockées.
author: tdykstra
ms.author: riande
ms.date: 01/18/2019
ms.topic: tutorial
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 5612f2f25d06feb904a205505ed8f048d2263266
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583439"
---
# <a name="tutorial-use-async-and-stored-procedures-with-ef-in-an-aspnet-mvc-app"></a>Didacticiel : utiliser des procédures stockées et Async avec EF dans une application ASP.NET MVC

Dans les didacticiels précédents, vous avez appris à lire et à mettre à jour les données à l’aide du modèle de programmation synchrone. Dans ce didacticiel, vous allez découvrir comment implémenter le modèle de programmation asynchrone. Le code asynchrone peut aider une application à fonctionner mieux, car elle permet une meilleure utilisation des ressources du serveur.

Dans ce didacticiel, vous allez également apprendre à utiliser des procédures stockées pour les opérations d’insertion, de mise à jour et de suppression sur une entité.

Enfin, vous redéployez l’application sur Azure, ainsi que toutes les modifications que vous avez apportées à la base de données que vous avez implémentées depuis la première fois que vous avez déployé.

Les illustrations suivantes montrent quelques-unes des pages que vous allez utiliser.

![Page services](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Créer un service](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * En savoir plus sur le code asynchrone
> * Créer un contrôleur de service
> * Utiliser des procédures stockées
> * Déployer sur Azure

## <a name="prerequisites"></a>Conditions préalables requises

* [Mise à jour de données associées](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="why-use-asynchronous-code"></a>Pourquoi utiliser du code asynchrone ?

Un serveur web a un nombre limité de threads disponibles et, dans les situations de forte charge, tous les threads disponibles peuvent être utilisés. Quand cela se produit, le serveur ne peut pas traiter de nouvelle requête tant que les threads ne sont pas libérés. Avec le code synchrone, plusieurs threads peuvent être bloqués alors qu’ils n’effectuent en fait aucun travail, car ils attendent que des E/S se terminent. Avec le code asynchrone, quand un processus attend que des E/S se terminent, son thread est libéré afin d’être utilisé par le serveur pour traiter d’autres demandes. Par conséquent, le code asynchrone permet d’utiliser plus efficacement les ressources du serveur, et le serveur est activé pour gérer davantage de trafic sans retard.

Dans les versions antérieures de .NET, l’écriture et le test de code asynchrone étaient complexes, sujettes aux erreurs et difficiles à déboguer. Dans .NET 4,5, l’écriture, le test et le débogage de code asynchrone sont tellement plus faciles que vous devez généralement écrire du code asynchrone à moins que vous n’ayez une raison de ne pas le faire. Le code asynchrone introduit une petite quantité de surcharge, mais pour les situations de faible trafic, l’impact sur les performances est négligeable, tandis que pour les situations de trafic élevé, l’amélioration potentielle des performances est importante.

Pour plus d’informations sur la programmation asynchrone, consultez [utiliser la prise en charge asynchrone de .net 4.5 pour éviter les appels de blocage](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).

## <a name="create-department-controller"></a>Créer un contrôleur de service

Créez un contrôleur de service de la même façon que vous le faisiez avec les contrôleurs précédents, sauf cette fois, activez la case à cocher **utiliser les actions du contrôleur Async** .

Les points suivants montrent ce qui a été ajouté au code synchrone pour la méthode `Index` pour le rendre asynchrone :

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

Quatre modifications ont été appliquées pour permettre à la requête de base de données Entity Framework de s’exécuter de façon asynchrone :

- La méthode est marquée avec le mot clé `async`, qui indique au compilateur de générer des rappels pour les parties du corps de la méthode et de créer automatiquement l’objet `Task<ActionResult>` qui est retourné.
- Le type de retour a été modifié de `ActionResult` à `Task<ActionResult>`. Le type de `Task<T>` représente un travail en cours avec un résultat de type `T`.
- Le mot clé `await` a été appliqué à l’appel de service Web. Lorsque le compilateur voit ce mot clé, en arrière-plan, il fractionne la méthode en deux parties. La première partie se termine par l’opération qui est démarrée de manière asynchrone. La deuxième partie est placée dans une méthode de rappel qui est appelée lorsque l’opération se termine.
- La version asynchrone de la méthode d’extension `ToList` a été appelée.

Pourquoi l’instruction `departments.ToList` est-elle modifiée, mais pas l’instruction `departments = db.Departments` ? En effet, seules les instructions qui provoquent l’envoi de requêtes ou de commandes à la base de données sont exécutées de façon asynchrone. L’instruction `departments = db.Departments` configure une requête, mais la requête n’est pas exécutée tant que la méthode `ToList` n’est pas appelée. Par conséquent, seule la méthode `ToList` est exécutée de façon asynchrone.

Dans la méthode `Details` et les méthodes `HttpGet` `Edit` et `Delete`, la méthode `Find` est celle qui entraîne l’envoi d’une requête à la base de données. c’est la méthode qui est exécutée de façon asynchrone :

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

Dans les méthodes `Create`, `HttpPost Edit`et `DeleteConfirmed`, il s’agit de l’appel de méthode `SaveChanges` qui provoque l’exécution d’une commande, et non des instructions telles que `db.Departments.Add(department)` qui entraînent uniquement la modification des entités en mémoire.

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

Ouvrez *Views\Department\Index.cshtml*et remplacez le code du modèle par le code suivant :

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

Ce code modifie le titre de index en Departments, déplace le nom de l’administrateur vers la droite et fournit le nom complet de l’administrateur.

Dans les affichages créer, supprimer, détails et modifier, remplacez la légende du champ `InstructorID` par « administrateur » de la même façon que vous avez modifié le champ nom du service en « département » dans les vues du cours.

Dans les vues Create et Edit, utilisez le code suivant :

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

Dans les vues Delete et Details, utilisez le code suivant :

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Exécutez l’application, puis cliquez sur l’onglet **services** .

Tout fonctionne de la même manière que dans les autres contrôleurs, mais dans ce contrôleur, toutes les requêtes SQL s’exécutent de façon asynchrone.

Voici quelques éléments à connaître lorsque vous utilisez la programmation asynchrone avec l’Entity Framework :

- Le code Async n’est pas thread-safe. En d’autres termes, en d’autres termes, n’essayez pas d’effectuer plusieurs opérations en parallèle à l’aide de la même instance de contexte.
- Si vous souhaitez tirer parti des avantages de performances du code asynchrone, assurez-vous que n’importe quel package de librairie que vous utilisez (telles que pour la pagination), utilise également async si elles appellent n'importe quelle méthode Entity Framework qui provoque l'envoi de requêtes à la base de données.

## <a name="use-stored-procedures"></a>Utiliser des procédures stockées

Certains développeurs et administrateurs de base de données préfèrent utiliser des procédures stockées pour l’accès aux bases de données. Dans les versions antérieures de Entity Framework vous pouvez récupérer des données à l’aide d’une procédure stockée en [exécutant une requête SQL brute](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), mais vous ne pouvez pas indiquer à EF d’utiliser des procédures stockées pour les opérations de mise à jour. Dans EF 6, il est facile de configurer Code First pour utiliser des procédures stockées.

1. Dans *DAL\SchoolContext.cs*, ajoutez le code en surbrillance à la méthode `OnModelCreating`.

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    Ce code indique à Entity Framework d’utiliser des procédures stockées pour les opérations d’insertion, de mise à jour et de suppression sur l’entité `Department`.
2. Dans la console de gestion de package, entrez la commande suivante :

    `add-migration DepartmentSP`

    Ouvrez *migrations\\&lt;timestamp&gt;\_DepartmentSP.cs* pour voir le code dans la méthode `Up` qui crée des procédures stockées INSERT, Update et DELETE :

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
3. Dans la console de gestion de package, entrez la commande suivante :

     `update-database`
4. Exécutez l’application en mode débogage, cliquez sur l’onglet **services** , puis cliquez sur **créer**.
5. Entrez les données d’un nouveau service, puis cliquez sur **créer**.

6. Dans Visual Studio, examinez les journaux dans la fenêtre **sortie** pour voir qu’une procédure stockée a été utilisée pour insérer la nouvelle ligne Department.

     ![Service d’insertion SP](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Code First crée des noms de procédure stockée par défaut. Si vous utilisez une base de données existante, vous devrez peut-être personnaliser les noms des procédures stockées pour pouvoir utiliser des procédures stockées déjà définies dans la base de données. Pour plus d’informations sur la procédure à suivre, consultez [Entity Framework code First insertion/mise à jour/suppression de procédures stockées](https://msdn.microsoft.com/data/dn468673).

Si vous souhaitez personnaliser les procédures stockées générées, vous pouvez modifier le code de génération de modèles automatique pour la méthode de migration `Up` qui crée la procédure stockée. De cette façon, vos modifications sont reflétées chaque fois que la migration est exécutée et sont appliquées à votre base de données de production lorsque les migrations s’exécutent automatiquement en production après le déploiement.

Si vous souhaitez modifier une procédure stockée existante qui a été créée dans une migration précédente, vous pouvez utiliser la commande Add-migration pour générer une migration vierge, puis écrire manuellement le code qui appelle la méthode [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) .

## <a name="deploy-to-azure"></a>Déployer sur Azure

Cette section nécessite que vous ayez suivi la section facultative **déploiement de l’application dans Azure** du didacticiel [migrations et déploiement](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) de cette série. Si vous aviez des erreurs de migration que vous avez résolues en supprimant la base de données dans votre projet local, ignorez cette section.

1. Dans **l’Explorateur de solutions** de Visual Studio, cliquez avec le bouton droit sur le projet, puis dans le menu contextuel, sélectionnez **Publier**.
2. Cliquez sur **Publier**.

    Visual Studio déploie l’application sur Azure, et l’application s’ouvre dans votre navigateur par défaut, en cours d’exécution dans Azure.
3. Testez l’application pour vérifier qu’elle fonctionne.

    La première fois que vous exécutez une page qui accède à la base de données, le Entity Framework exécute toutes les migrations `Up` méthodes nécessaires pour mettre à jour la base de données avec le modèle de données actuel. Vous pouvez désormais utiliser toutes les pages Web que vous avez ajoutées depuis la dernière fois que vous avez déployé, y compris les pages de service que vous avez ajoutées dans ce didacticiel.

## <a name="get-the-code"></a>Obtenir le code

[Télécharger le projet terminé](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Ressources supplémentaires

Vous trouverez des liens vers d’autres ressources de Entity Framework dans l' [ASP.net accès aux données-ressources recommandées](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Appris à propos du code asynchrone
> * Création d’un contrôleur de service
> * Procédures stockées utilisées
> * Déployé sur Azure

Passez à l’article suivant pour apprendre à gérer les conflits quand plusieurs utilisateurs mettent à jour la même entité en même temps.
> [!div class="nextstepaction"]
> [Gestion de l’accès concurrentiel](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
