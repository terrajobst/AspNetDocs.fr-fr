---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
title: Prise en main avec Entity Framework 4,0 Database First et ASP.NET 4 Web Forms-part 7 | Microsoft Docs
author: tdykstra
description: L’exemple d’application Web Contoso University montre comment créer des applications de Web Forms ASP.NET à l’aide de l’Entity Framework. L’exemple d’application est...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: f8afb245-b705-419c-8790-0b295e90d5e2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
msc.type: authoredcontent
ms.openlocfilehash: 18d4b44c5e23fd6942c3adf48a33a5602e6df6d0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78603431"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-7"></a>Prise en main avec Entity Framework 4,0 Database First et ASP.NET 4 Web Forms-part 7

par [Tom Dykstra](https://github.com/tdykstra)

> L’exemple d’application Web Contoso University montre comment créer des applications ASP.NET Web Forms à l’aide de la Entity Framework 4,0 et de Visual Studio 2010. Pour plus d’informations sur la série de didacticiels, consultez [le premier didacticiel de la série](the-entity-framework-and-aspnet-getting-started-part-1.md)

## <a name="using-stored-procedures"></a>Utilisation des procédures stockées

Dans le didacticiel précédent, vous avez implémenté un modèle d’héritage TPH (table par hiérarchie). Ce didacticiel vous montre comment utiliser des procédures stockées pour mieux contrôler l’accès aux bases de données.

Le Entity Framework vous permet de spécifier qu’il doit utiliser des procédures stockées pour l’accès aux bases de données. Pour tout type d’entité, vous pouvez spécifier une procédure stockée à utiliser pour créer, mettre à jour ou supprimer des entités de ce type. Ensuite, dans le modèle de données, vous pouvez ajouter des références à des procédures stockées que vous pouvez utiliser pour effectuer des tâches telles que la récupération de jeux d’entités.

L’utilisation de procédures stockées est une exigence courante pour l’accès aux bases de données. Dans certains cas, un administrateur de base de données peut exiger que tous les accès à la base de données passent par des procédures stockées pour des raisons de sécurité. Dans d’autres cas, vous pouvez créer une logique métier dans certains processus que le Entity Framework utilise lorsqu’il met à jour la base de données. Par exemple, chaque fois qu’une entité est supprimée, vous souhaiterez peut-être la copier dans une base de données d’archivage. Ou lorsqu’une ligne est mise à jour, vous souhaiterez peut-être écrire une ligne dans une table de journalisation qui enregistre la personne qui a apporté la modification. Vous pouvez effectuer ces genres de tâches dans une procédure stockée appelée chaque fois que le Entity Framework supprime une entité ou met à jour une entité.

Comme dans le didacticiel précédent, vous ne créez pas de nouvelles pages. Au lieu de cela, vous allez modifier la manière dont le Entity Framework accède à la base de données pour certaines des pages que vous avez déjà créées.

Dans ce didacticiel, vous allez créer des procédures stockées dans la base de données pour insérer des entités `Student` et `Instructor`. Vous allez les ajouter au modèle de données et spécifier que le Entity Framework doit les utiliser pour ajouter des entités `Student` et `Instructor` à la base de données. Vous allez également créer une procédure stockée que vous pouvez utiliser pour récupérer `Course` entités.

## <a name="creating-stored-procedures-in-the-database"></a>Création de procédures stockées dans la base de données

(Si vous utilisez le fichier *School. mdf* du projet disponible en téléchargement dans ce didacticiel, vous pouvez ignorer cette section, car les procédures stockées existent déjà.)

Dans **Explorateur de serveurs**, développez *School. mdf*, cliquez avec le bouton droit sur **procédures stockées**, puis sélectionnez **Ajouter une nouvelle procédure stockée**.

[![image15](the-entity-framework-and-aspnet-getting-started-part-7/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image1.png)

Copiez les instructions SQL suivantes et collez-les dans la fenêtre procédure stockée, en remplaçant la procédure stockée squelette.

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample1.sql)]

[![image14](the-entity-framework-and-aspnet-getting-started-part-7/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image3.png)

les entités `Student` ont quatre propriétés : `PersonID`, `LastName`, `FirstName`et `EnrollmentDate`. La base de données génère automatiquement la valeur d’ID, et la procédure stockée accepte des paramètres pour les trois autres. La procédure stockée retourne la valeur de la clé d’enregistrement de la nouvelle ligne afin que le Entity Framework puisse suivre cela dans la version de l’entité qu’elle conserve en mémoire.

Enregistrez et fermez la fenêtre procédure stockée.

Créez une procédure stockée `InsertInstructor` de la même manière, à l’aide des instructions SQL suivantes :

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample2.sql)]

Créez également des procédures stockées `Update` pour les entités `Student` et `Instructor`. (La base de données a déjà une procédure stockée `DeletePerson` qui fonctionne pour les entités `Instructor` et `Student`.)

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample3.sql)]

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample4.sql)]

Dans ce didacticiel, vous allez mapper les trois fonctions (Insert, Update et Delete) pour chaque type d’entité. Le Entity Framework version 4 vous permet de mapper uniquement une ou deux de ces fonctions aux procédures stockées sans mapper les autres, à une exception près : Si vous mappez la fonction de mise à jour mais pas la fonction de suppression, la Entity Framework lèvera une exception lorsque vous tentative de suppression d’une entité. Dans le Entity Framework version 3,5, vous ne disposiez pas de cette flexibilité dans le mappage des procédures stockées : Si vous avez mappé une fonction que vous deviez mapper les trois.

Pour créer une procédure stockée qui lit plutôt que met à jour les données, créez-en une qui sélectionne toutes les entités `Course`, à l’aide des instructions SQL suivantes :

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample5.sql)]

## <a name="adding-the-stored-procedures-to-the-data-model"></a>Ajout de procédures stockées au modèle de données

Les procédures stockées sont maintenant définies dans la base de données, mais elles doivent être ajoutées au modèle de données pour les mettre à disposition du Entity Framework. Ouvrez *SchoolModel. edmx*, cliquez avec le bouton droit sur l’aire de conception, puis sélectionnez **mettre à jour le modèle à partir de la base de données**. Dans l’onglet **Ajouter** de la boîte **de dialogue choisir vos objets de base de données** , développez **procédures stockées**, sélectionnez les procédures stockées nouvellement créées et la procédure stockée `DeletePerson`, puis cliquez sur **Terminer**.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-7/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image5.png)

## <a name="mapping-the-stored-procedures"></a>Mappage des procédures stockées

Dans le concepteur de modèle de données, cliquez avec le bouton droit sur l’entité `Student`, puis sélectionnez **mappage de procédure stockée**.

[![image21](the-entity-framework-and-aspnet-getting-started-part-7/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image7.png)

La fenêtre **Détails de mappage** s’affiche, dans laquelle vous pouvez spécifier des procédures stockées que le Entity Framework doit utiliser pour l’insertion, la mise à jour et la suppression d’entités de ce type.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-7/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image9.png)

Définissez la fonction **Insert** sur **InsertStudent**. La fenêtre affiche une liste de paramètres de procédure stockée, chacun d’entre eux devant être mappé à une propriété d’entité. Deux d’entre elles sont mappées automatiquement, car les noms sont identiques. Il n’existe aucune propriété d’entité nommée `FirstName`. vous devez donc sélectionner manuellement `FirstMidName` dans une liste déroulante qui affiche les propriétés d’entité disponibles. (Cela est dû au fait que vous avez remplacé le nom de la propriété `FirstName` par `FirstMidName` dans le premier didacticiel.)

[![image23](the-entity-framework-and-aspnet-getting-started-part-7/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image11.png)

Dans la même fenêtre **Détails de mappage** , mappez la fonction `Update` à la procédure stockée `UpdateStudent` (veillez à spécifier `FirstMidName` comme valeur de paramètre pour `FirstName`, comme vous l’avez fait pour la procédure stockée `Insert`) et la fonction `Delete` à la procédure stockée `DeletePerson`.

[![image01](the-entity-framework-and-aspnet-getting-started-part-7/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image13.png)

Suivez la même procédure pour mapper les procédures stockées INSERT, Update et Delete pour les formateurs à l’entité `Instructor`.

[![image02](the-entity-framework-and-aspnet-getting-started-part-7/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image15.png)

Pour les procédures stockées qui lisent plutôt que les données de mise à jour, vous utilisez la fenêtre **Explorateur de modèles** pour mapper la procédure stockée au type d’entité qu’elle retourne. Dans le concepteur de modèle de données, cliquez avec le bouton droit sur l’aire de conception, puis sélectionnez **Explorateur de modèles**. Ouvrez le nœud **SchoolModel. Store** , puis ouvrez le nœud **procédures stockées** . Cliquez ensuite avec le bouton droit sur la procédure stockée `GetCourses`, puis sélectionnez **Ajouter une importation de fonction**.

[![image24](the-entity-framework-and-aspnet-getting-started-part-7/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image17.png)

Dans la boîte de dialogue **Ajouter une importation de fonction** , sous **retourne une collection d'** **entités**Select, puis sélectionnez `Course` comme type d’entité retourné. Quand vous avez terminé, cliquez sur **OK**. Enregistrez et fermez le fichier *. edmx* .

[![image25](the-entity-framework-and-aspnet-getting-started-part-7/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image19.png)

## <a name="using-insert-update-and-delete-stored-procedures"></a>Utilisation des procédures stockées INSERT, Update et Delete

Les procédures stockées pour insérer, mettre à jour et supprimer des données sont utilisées par le Entity Framework automatiquement une fois que vous les avez ajoutées au modèle de données et les avez mappées aux entités appropriées. Vous pouvez maintenant exécuter la page *StudentsAdd. aspx* , et chaque fois que vous créez un nouvel étudiant, le Entity Framework utilisera la procédure stockée `InsertStudent` pour ajouter la nouvelle ligne à la table `Student`.

[![image03](the-entity-framework-and-aspnet-getting-started-part-7/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image21.png)

Exécutez la page *students. aspx* et le nouvel étudiant apparaît dans la liste.

[![image04](the-entity-framework-and-aspnet-getting-started-part-7/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image23.png)

Modifiez le nom pour vérifier que la fonction de mise à jour fonctionne, puis supprimez l’étudiant pour vérifier que la fonction de suppression fonctionne.

[![image05](the-entity-framework-and-aspnet-getting-started-part-7/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image25.png)

## <a name="using-select-stored-procedures"></a>Utilisation des procédures stockées Select

Le Entity Framework n’exécute pas automatiquement les procédures stockées telles que `GetCourses`, et vous ne pouvez pas les utiliser avec le contrôle `EntityDataSource`. Pour les utiliser, vous les appelez à partir du code.

Ouvrez le fichier *InstructorsCourses.aspx.cs* . La méthode `PopulateDropDownLists` utilise une requête LINQ-to-Entities pour récupérer toutes les entités course afin qu’elle puisse parcourir la liste et déterminer à qui un formateur est assigné et quels sont ceux qui ne sont pas assignés :

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample6.cs)]

Remplacez ceci par le code suivant :

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample7.cs)]

La page utilise à présent la procédure stockée `GetCourses` pour récupérer la liste de tous les cours. Exécutez la page pour vérifier qu’elle fonctionne comme avant.

(Les propriétés de navigation des entités récupérées par une procédure stockée peuvent ne pas être renseignées automatiquement avec les données associées à ces entités, selon `ObjectContext` paramètres par défaut. Pour plus d’informations, consultez [chargement d’objets connexes](https://msdn.microsoft.com/library/bb896272.aspx) dans la bibliothèque MSDN.)

Dans le didacticiel suivant, vous apprendrez à utiliser Dynamic Data fonctionnalité pour faciliter la programmation et le test des règles de mise en forme et de validation des données. Au lieu de spécifier sur chaque règle de page Web, comme les chaînes de format de données, et si un champ est obligatoire ou non, vous pouvez spécifier ces règles dans les métadonnées du modèle de données et elles sont automatiquement appliquées à chaque page.

> [!div class="step-by-step"]
> [Précédent](the-entity-framework-and-aspnet-getting-started-part-6.md)
> [Suivant](the-entity-framework-and-aspnet-getting-started-part-8.md)
