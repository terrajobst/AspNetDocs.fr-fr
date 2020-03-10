---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Didacticiel : implémenter l’héritage avec EF dans une application ASP.NET MVC 5'
description: Ce didacticiel vous indiquera comment implémenter l’héritage dans le modèle de données.
author: tdykstra
ms.author: riande
ms.date: 01/21/2019
ms.topic: tutorial
ms.assetid: 08834147-77ec-454a-bb7a-d931d2a40dab
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 73a01ed47b0935a1a9734c197377470defb1fe36
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583061"
---
# <a name="tutorial-implement-inheritance-with-ef-in-an-aspnet-mvc-5-app"></a>Didacticiel : implémenter l’héritage avec EF dans une application ASP.NET MVC 5

Dans le didacticiel précédent, vous avez géré les exceptions d’accès concurrentiel. Ce didacticiel vous indiquera comment implémenter l’héritage dans le modèle de données.

Dans la programmation orientée objet, vous pouvez utiliser [l’héritage](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) pour faciliter la [réutilisation du code](http://en.wikipedia.org/wiki/Code_reuse). Dans ce didacticiel, vous allez modifier les classes `Instructor` et `Student` afin qu’elles dérivent d’une classe de base `Person` qui contient des propriétés telles que `LastName`, communes aux formateurs et aux étudiants. Vous n’ajouterez ni ne modifierez aucune page web, mais vous modifierez une partie du code et ces modifications seront automatiquement répercutées dans la base de données.

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Apprendre à mapper l’héritage à la base de données
> * Créer la classe Person
> * Mettre à jour Student et Instructor
> * Ajouter une personne au modèle
> * Créer et mettre à jour des migrations
> * Tester l’implémentation
> * Déployer sur Azure

## <a name="prerequisites"></a>Conditions préalables requises

* [Gestion des accès concurrentiels](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="map-inheritance-to-database"></a>Mapper l’héritage à la base de données

Les classes `Instructor` et `Student` dans le modèle de données `School` ont plusieurs propriétés qui sont identiques :

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

Supposons que vous souhaitez éliminer le code redondant pour les propriétés partagées par les entités `Instructor` et `Student`. Ou vous souhaitez écrire un service capable de mettre en forme les noms sans se soucier du fait que le nom provienne d’un formateur ou d’un étudiant. Vous pouvez créer une classe de base `Person` qui contient uniquement les propriétés partagées, puis faire en sorte que les entités `Instructor` et `Student` héritent de cette classe de base, comme le montre l’illustration suivante :

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Il existe plusieurs façons de représenter cette structure d’héritage dans la base de données. Vous pouvez avoir une table `Person` qui contient des informations sur les étudiants et les formateurs dans une seule table. Certaines des colonnes ne peuvent s’appliquer qu’aux formateurs (`HireDate`), certains uniquement aux élèves (`EnrollmentDate`), certains aux deux (`LastName`, `FirstName`). En règle générale, vous avez une colonne de *discriminateur* pour indiquer le type représenté par chaque ligne. Par exemple, la colonne de discriminateur peut avoir « Instructor » pour les formateurs et « Student » pour les étudiants.

![Table par hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Ce modèle de génération d’une structure d’héritage d’entité à partir d’une seule table de base de données est appelé l’héritage TPH ( *table par hiérarchie* ).

Une alternative consiste à faire en sorte que la base de données ressemble plus à la structure d’héritage. Par exemple, vous pouvez avoir uniquement les champs nom dans la table `Person` et avoir des tables `Instructor` et `Student` distinctes avec les champs de date.

![Table par type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Ce modèle de création d’une table de base de données pour chaque classe d’entité est appelé l’héritage de *table par type* (TPT).

Une autre option encore consiste à mapper tous les types non abstraits à des tables individuelles. Toutes les propriétés d’une classe, y compris les propriétés héritées, sont mappées aux colonnes de la table correspondante. Ce modèle porte le nom d’héritage TPC (table par classe concrète). Si vous avez implémenté l’héritage TPC pour les classes `Person`, `Student`et `Instructor` comme indiqué plus haut, les tables `Student` et `Instructor` ne sont pas différentes après l’implémentation de l’héritage.

Les modèles d’héritage TPC et TPH offrent généralement de meilleures performances dans le Entity Framework que les modèles d’héritage TPT, car les modèles TPT peuvent entraîner des requêtes de jointure complexes.

Ce didacticiel montre comment implémenter l’héritage TPH. TPH est le modèle d’héritage par défaut dans le Entity Framework. il vous suffit de créer une classe `Person`, de modifier les classes `Instructor` et `Student` pour qu’elles dérivent de `Person`, d’ajouter la nouvelle classe à la `DbContext`et de créer une migration. (Pour plus d’informations sur la façon d’implémenter les autres modèles d’héritage, consultez [mappage de l’héritage TPT (table par type)](https://msdn.microsoft.com/data/jj591617#2.5) et mappage de l’héritage table [par classe (TPC)](https://msdn.microsoft.com/data/jj591617#2.6) dans la documentation MSDN Entity Framework.)

## <a name="create-the-person-class"></a>Créer la classe Person

Dans le dossier *Models* , créez *Person.cs* et remplacez le code du modèle par le code suivant :

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="update-instructor-and-student"></a>Mettre à jour Student et Instructor

À présent, mettez à jour les *Instructor.cs* et *Student.cs* pour qu’ils héritent des valeurs de *Person.SC*.

Dans *Instructor.cs*, dérivez la classe `Instructor` de la classe `Person` et supprimez les champs clé et nom. Le code ressemblera à l’exemple suivant :

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Apportez des modifications similaires à *Student.cs*. La classe `Student` se présente comme dans l’exemple suivant :

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-person-to-the-model"></a>Ajouter une personne au modèle

Dans *SchoolContext.cs*, ajoutez une propriété `DbSet` pour le type d’entité `Person` :

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

C’est là tout ce dont Entity Framework a besoin pour configurer l’héritage TPH (table par hiérarchie). Comme vous le verrez, lorsque la base de données est mise à jour, elle aura une table `Person` à la place des tables `Student` et `Instructor`.

## <a name="create-and-update-migrations"></a>Créer et mettre à jour des migrations

Dans la console du gestionnaire de package (PMC), entrez la commande suivante :

`Add-Migration Inheritance`

Exécutez la commande `Update-Database` dans le PMC. La commande échoue à ce stade, car nous avons des données existantes que les migrations ne savent pas gérer. Vous recevez un message d’erreur semblable à celui-ci :

> *Impossible de supprimer l’objet « dbo ». Instructor', car il est référencé par une contrainte de clé étrangère.*

Ouvrez *migrations\&lt ; timestamp&gt;\_Inheritance.cs* et remplacez la méthode `Up` par le code suivant :

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

Ce code prend en charge les tâches de mise à jour de base de données suivantes :

- Supprime les contraintes de clé étrangère et les index qui pointent vers la table Student.
- Renomme la table Instructor en Person et apporte les modifications nécessaires pour qu’elle stocke les données des étudiants :

    - Ajoute une EnrollmentDate nullable pour les étudiants.
    - Ajoute la colonne Discriminator pour indiquer si une ligne est pour un étudiant ou un formateur.
    - Rend HireDate nullable étant donné que les lignes d’étudiant n’ont pas de dates d’embauche.
    - Ajoute un champ temporaire qui sera utilisé pour mettre à jour les clés étrangères qui pointent vers les étudiants. Lorsque vous copiez des élèves dans la table Person, ils obtiennent de nouvelles valeurs de clé primaire.
- Copie des données à partir de la table Student dans la table Person. Cela entraîne l’affectation de nouvelles valeurs de clés primaires aux étudiants.
- Corrige les valeurs de clés étrangères qui pointent vers les étudiants.
- Crée de nouveau les index et les contraintes de clé étrangère, désormais pointées vers la table Person.

(Si vous aviez utilisé un GUID à la place d’un entier comme type de clé primaire, les valeurs des clés primaires des étudiants n’auraient pas changé, et plusieurs de ces étapes auraient pu être omises.)

Exécutez de nouveau la commande `update-database`.

(Dans un système de production, vous apporterez des modifications à la méthode en baisse au cas où vous auriez dû l’utiliser pour revenir à la version précédente de la base de données. Pour ce didacticiel, vous n’allez pas utiliser la méthode OFF.)

> [!NOTE]
> Il est possible d’établir d’autres erreurs lors de la migration des données et d’apporter des modifications au schéma. Si vous recevez des erreurs de migration que vous ne pouvez pas résoudre, vous pouvez poursuivre le didacticiel en modifiant la chaîne de connexion dans le fichier *Web. config* ou en supprimant la base de données. L’approche la plus simple consiste à renommer la base de données dans le fichier *Web. config* . Par exemple, remplacez le nom de la base de données par par contosouniversity2 comme indiqué dans l’exemple suivant :
>
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
>
> Avec une nouvelle base de données, il n’y a pas de données à migrer et la commande `update-database` est bien plus susceptible de s’exécuter sans erreur. Pour obtenir des instructions sur la suppression de la base de données, consultez Suppression d' [une base de données de Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). Si vous adoptez cette approche pour poursuivre le didacticiel, ignorez l’étape de déploiement à la fin de ce didacticiel ou déployez-la sur un nouveau site et une nouvelle base de données. Si vous déployez une mise à jour sur le même site que celui sur lequel vous avez déjà déployé, EF obtiendra la même erreur lors de l’exécution automatique des migrations. Si vous souhaitez résoudre les problèmes liés à une erreur de migration, la meilleure ressource est l’un des Entity Framework forums ou StackOverflow.com.

## <a name="test-the-implementation"></a>Tester l’implémentation

Exécutez le site et essayez différentes pages. Tout fonctionne comme avant.

Dans **Explorateur de serveurs,** développez **Data Connections\SchoolContext** , puis **tables**, et vous constatez que les tables **Student** et **Instructor** ont été remplacées par une table **Person** . Développez la table **Person** . vous constatez qu’elle contient toutes les colonnes qui étaient utilisées dans les tables **Student** et **Instructor** .

Cliquez avec le bouton droit sur la table Person, puis cliquez sur **Afficher les données de la table** pour voir la colonne de discriminateur.

Le diagramme suivant illustre la structure de la nouvelle base de données School :

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a>Déployer sur Azure

Cette section nécessite que vous ayez suivi la section facultative **déploiement de l’application dans Azure** dans la [partie 3, tri, filtrage et pagination](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) de cette série de didacticiels. Si vous aviez des erreurs de migration que vous avez résolues en supprimant la base de données dans votre projet local, ignorez cette étape. ou créez un nouveau site et une nouvelle base de données, et déployez-le dans le nouvel environnement.

1. Dans **l’Explorateur de solutions** de Visual Studio, cliquez avec le bouton droit sur le projet, puis dans le menu contextuel, sélectionnez **Publier**.

2. Cliquez sur **Publier**.

    L’application Web s’ouvre dans votre navigateur par défaut.

3. Testez l’application pour vérifier qu’elle fonctionne.

    La première fois que vous exécutez une page qui accède à la base de données, le Entity Framework exécute toutes les migrations `Up` méthodes nécessaires pour mettre à jour la base de données avec le modèle de données actuel.

## <a name="get-the-code"></a>Obtenir le code

[Télécharger le projet terminé](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Ressources supplémentaires

Vous trouverez des liens vers d’autres ressources de Entity Framework dans l' [ASP.net accès aux données-ressources recommandées](../../../../whitepapers/aspnet-data-access-content-map.md).

Pour plus d’informations sur cette structure d’héritage et les autres, consultez modèle d’héritage [TPT](https://msdn.microsoft.com/data/jj618293) et [modèle d’héritage TPH](https://msdn.microsoft.com/data/jj618292) sur MSDN. Dans le prochain didacticiel, vous allez apprendre à gérer divers scénarios Entity Framework relativement avancés.

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez effectué les actions suivantes :

> [!div class="checklist"]
> * Appris à mapper l’héritage à la base de données
> * Créez la classe Person
> * Mettez à jour Student et Instructor
> * Ajout d’une personne au modèle
> * Migrations créées et mises à jour
> * Testez l’implémentation
> * Déployé sur Azure

Passez à l’article suivant pour en savoir plus sur les sujets qui sont utiles lorsque vous allez au-delà des principes de base du développement d’applications Web ASP.NET qui utilisent Entity Framework Code First.
> [!div class="nextstepaction"]
> [Scénarios Entity Framework avancés](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
