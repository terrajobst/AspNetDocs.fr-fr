---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
title: Prise en main avec Entity Framework 4,0 Database First et ASP.NET 4 Web Forms-part 3 | Microsoft Docs
author: tdykstra
description: L’exemple d’application Web Contoso University montre comment créer des applications de Web Forms ASP.NET à l’aide de l’Entity Framework. L’exemple d’application est...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: ccdc3f8c-2568-40a7-8f8b-3c23d2e05388
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
msc.type: authoredcontent
ms.openlocfilehash: 88debb11a9157dce9ff000b1cb459b876dbceaf3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78643247"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-3"></a>Prise en main avec Entity Framework 4,0 Database First et ASP.NET 4 Web Forms-part 3

par [Tom Dykstra](https://github.com/tdykstra)

> L’exemple d’application Web Contoso University montre comment créer des applications ASP.NET Web Forms à l’aide de la Entity Framework 4,0 et de Visual Studio 2010. Pour plus d’informations sur la série de didacticiels, consultez [le premier didacticiel de la série](the-entity-framework-and-aspnet-getting-started-part-1.md)

## <a name="filtering-ordering-and-grouping-data"></a>Filtrage, classement et regroupement des données

Dans le didacticiel précédent, vous avez utilisé le contrôle `EntityDataSource` pour afficher et modifier les données. Dans ce didacticiel, vous allez filtrer, classer et regrouper des données. Quand vous faites cela en définissant les propriétés du contrôle `EntityDataSource`, la syntaxe est différente de celle des autres contrôles de source de données. Comme vous le verrez, toutefois, vous pouvez utiliser le contrôle `QueryExtender` pour réduire ces différences.

Vous allez modifier la page *students. aspx* pour filtrer les élèves, les trier par nom et rechercher le nom. Vous allez également modifier la page *courses. aspx* pour afficher les cours du département sélectionné et rechercher les cours par nom. Enfin, vous ajouterez les statistiques des étudiants à la page *about. aspx* .

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image1.png)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image3.png)

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image5.png)

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image7.png)

## <a name="using-the-entitydatasource-where-property-to-filter-data"></a>Utilisation de la propriété « Where » EntityDataSource pour filtrer les données

Ouvrez la page *students. aspx* que vous avez créée dans le didacticiel précédent. Comme actuellement configuré, le contrôle `GridView` dans la page affiche tous les noms du jeu d’entités `People`. Toutefois, vous souhaitez afficher uniquement les élèves, que vous pouvez trouver en sélectionnant `Person` entités qui ont des dates d’inscription non null.

Basculez en mode **conception** et sélectionnez le contrôle `EntityDataSource`. Dans la fenêtre **Propriétés** , définissez la propriété `Where` sur `it.EnrollmentDate is not null`.

[![image01](the-entity-framework-and-aspnet-getting-started-part-3/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image9.png)

La syntaxe que vous utilisez dans la propriété `Where` du contrôle `EntityDataSource` est Entity SQL. Entity SQL est semblable à Transact-SQL, mais il est personnalisé pour une utilisation avec des entités plutôt que des objets de base de données. Dans l' `it.EnrollmentDate is not null`d’expression, le mot `it` représente une référence à l’entité retournée par la requête. Par conséquent, `it.EnrollmentDate` fait référence à la propriété `EnrollmentDate` de l’entité `Person` que le contrôle `EntityDataSource` retourne.

Exécutez la page. La liste des étudiants contient désormais uniquement les élèves. (Aucune ligne n’est affichée lorsqu’il n’y a aucune date d’inscription.)

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image11.png)

## <a name="using-the-entitydatasource-orderby-property-to-order-data"></a>Utilisation de la propriété « OrderBy » EntityDataSource pour trier les données

Vous souhaitez également que cette liste soit dans l’ordre des noms lorsqu’elle est affichée pour la première fois. Lorsque la page *students. aspx* est toujours ouverte en mode **conception** et que le contrôle `EntityDataSource` est toujours sélectionné, dans la fenêtre **Propriétés** , définissez la propriété **orderby** sur `it.LastName`.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-3/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image13.png)

Exécutez la page. La liste des étudiants est désormais triée par nom de famille.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-3/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image15.png)

## <a name="using-a-control-parameter-to-set-the-where-property"></a>Utilisation d’un paramètre de contrôle pour définir la propriété « Where »

Comme pour les autres contrôles de source de données, vous pouvez passer des valeurs de paramètre à la propriété `Where`. Sur la page *courses. aspx* que vous avez créée dans la partie 2 du didacticiel, vous pouvez utiliser cette méthode pour afficher les cours qui sont associés au service qu’un utilisateur sélectionne dans la liste déroulante.

Ouvrez *courses. aspx* et basculez en mode **Design** . Ajoutez un deuxième contrôle `EntityDataSource` à la page, puis nommez-le `CoursesEntityDataSource`. Connectez-le au modèle `SchoolEntities`, puis sélectionnez `Courses` comme valeur **EntitySetName** .

Dans la fenêtre **Propriétés** , cliquez sur le bouton de sélection dans la zone de propriété **Where** . (Assurez-vous que le contrôle `CoursesEntityDataSource` est toujours sélectionné avant d’utiliser la fenêtre **Propriétés** .)

[![Image06](the-entity-framework-and-aspnet-getting-started-part-3/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image17.png)

La boîte de dialogue **éditeur d’expressions** s’affiche. Dans cette boîte de dialogue, sélectionnez **générer automatiquement l’expression WHERE en fonction des paramètres fournis**, puis cliquez sur **Ajouter un paramètre**. Nommez le paramètre `DepartmentID`, sélectionnez **Control** comme valeur **source du paramètre** , puis **DepartmentsDropDownList** comme valeur **ControlID** .

[![Image07](the-entity-framework-and-aspnet-getting-started-part-3/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image19.png)

Cliquez sur **afficher les propriétés avancées**, puis, dans la fenêtre **Propriétés** de la boîte de dialogue **éditeur d’expressions** , remplacez la valeur de la propriété `Type` par `Int32`.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-3/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image21.png)

Quand vous avez terminé, cliquez sur **OK**.

Sous la liste déroulante, ajoutez un contrôle `GridView` à la page, puis nommez-le `CoursesGridView`. Connectez-le au `CoursesEntityDataSource` contrôle de source de données, cliquez sur **Actualiser le schéma**, puis sur **modifier les colonnes**et supprimez la colonne `DepartmentID`. Le balisage de contrôle `GridView` ressemble à l’exemple suivant.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample1.aspx)]

Lorsque l’utilisateur modifie le service sélectionné dans la liste déroulante, vous souhaitez que la liste des cours associés change automatiquement. Pour ce faire, sélectionnez la liste déroulante, puis, dans la fenêtre **Propriétés** , affectez à la propriété `AutoPostBack` la valeur `True`.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-3/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image23.png)

Maintenant que vous avez terminé d’utiliser le concepteur, passez en mode **source** et remplacez les propriétés `ConnectionString` et `DefaultContainer` Name du contrôle `CoursesEntityDataSource` par l’attribut `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"`. Lorsque vous avez terminé, le balisage du contrôle doit ressembler à l’exemple suivant.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample2.aspx)]

Exécutez la page et utilisez la liste déroulante pour sélectionner différents services. Seuls les cours proposés par le service sélectionné s’affichent dans le contrôle `GridView`.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-3/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image25.png)

## <a name="using-the-entitydatasource-groupby-property-to-group-data"></a>Utilisation de la propriété « GroupBy » EntityDataSource pour regrouper des données

Supposons que Contoso University souhaite placer des statistiques sur la page about de l’étudiant. Plus précisément, il souhaite afficher une répartition du nombre d’élèves par date d’inscription.

Ouvrez *about. aspx*et, en mode **source** , remplacez le contenu existant du contrôle `BodyContent` par « statistiques du corps de l’étudiant » entre les balises `h2` :

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample3.aspx)]

Après l’en-tête, ajoutez un contrôle `EntityDataSource` et nommez-le `StudentStatisticsEntityDataSource`. Connectez-le à `SchoolEntities`, sélectionnez le jeu d’entités `People` et laissez la zone **Sélectionner** dans l’Assistant inchangée. Définissez les propriétés suivantes dans la fenêtre **Propriétés** :

- Pour filtrer uniquement pour les élèves, affectez à la propriété `Where` la valeur `it.EnrollmentDate is not null`.
- Pour regrouper les résultats selon la date d’inscription, affectez à la propriété `GroupBy` la valeur `it.EnrollmentDate`.
- Pour sélectionner la date d’inscription et le nombre d’élèves, affectez à la propriété `Select` la valeur `it.EnrollmentDate, Count(it.EnrollmentDate) AS NumberOfStudents`.
- Pour classer les résultats par date d’inscription, affectez à la propriété `OrderBy` la valeur `it.EnrollmentDate`.

En mode **source** , remplacez les propriétés `ConnectionString` et `DefaultContainer` nom par une propriété `ContextTypeName`. Le balisage de contrôle `EntityDataSource` ressemble maintenant à l’exemple suivant.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample4.aspx)]

La syntaxe des propriétés `Select`, `GroupBy`et `Where` ressemble à Transact-SQL, à l’exception du mot clé `it` qui spécifie l’entité actuelle.

Ajoutez le balisage suivant pour créer un contrôle de `GridView` pour afficher les données.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample5.aspx)]

Exécutez la page pour afficher une liste qui indique le nombre d’élèves par date d’inscription.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image27.png)

## <a name="using-the-queryextender-control-for-filtering-and-ordering"></a>Utilisation du contrôle QueryExtender pour le filtrage et le tri

Le contrôle `QueryExtender` permet de spécifier le filtrage et le tri dans le balisage. La syntaxe est indépendante du système de gestion de base de données (SGBD) que vous utilisez. Il est également généralement indépendant du Entity Framework, à l’exception que la syntaxe que vous utilisez pour les propriétés de navigation est propre à la Entity Framework.

Dans cette partie du didacticiel, vous utiliserez un contrôle de `QueryExtender` pour filtrer et trier les données, et l’un des champs order-by sera une propriété de navigation.

(Si vous préférez utiliser le code au lieu du balisage pour étendre les requêtes qui sont générées automatiquement par le contrôle `EntityDataSource`, vous pouvez le faire en gérant l’événement `QueryCreated`. C’est ainsi que le contrôle `QueryExtender` étend également `EntityDataSource` des requêtes de contrôle.)

Ouvrez la page *courses. aspx* et, sous le balisage que vous avez ajouté précédemment, insérez le balisage suivant pour créer un titre, une zone de texte pour entrer des chaînes de recherche, un bouton de recherche et un contrôle de `EntityDataSource` lié au jeu d’entités `Courses`.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample6.aspx)]

Notez que la propriété `Include` du contrôle `EntityDataSource` a la valeur `Department`. Dans la base de données, la table `Course` ne contient pas le nom du service ; Il contient une colonne de clé étrangère `DepartmentID`. Si vous interrogez la base de données directement, pour obtenir le nom du service ainsi que les données de cours, vous devez joindre les tables `Course` et `Department`. En affectant à la propriété `Include` la valeur `Department`, vous spécifiez que le Entity Framework doit effectuer le travail d’obtention de l’entité `Department` associée lorsqu’elle obtient une entité `Course`. L’entité `Department` est ensuite stockée dans la propriété de navigation `Department` de l’entité `Course`. (Par défaut, la classe `SchoolEntities` qui a été générée par le concepteur de modèle de données récupère les données associées lorsqu’elles sont nécessaires, et que vous avez lié le contrôle de source de données à cette classe, la définition de la propriété `Include` n’est donc pas nécessaire. Toutefois, la définition de cette valeur améliore les performances de la page, car dans le cas contraire, le Entity Framework effectuerait des appels distincts à la base de données pour récupérer des données pour les entités `Course` et pour les entités `Department` associées.)

Après le contrôle `EntityDataSource` que vous venez de créer, insérez le balisage suivant pour créer un contrôle `QueryExtender` lié à ce contrôle `EntityDataSource`.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample7.aspx)]

L’élément `SearchExpression` spécifie que vous souhaitez sélectionner des cours dont les titres correspondent à la valeur entrée dans la zone de texte. Seuls le nombre de caractères entrés dans la zone de texte sont comparés, car la propriété `SearchType` spécifie `StartsWith`.

L’élément `OrderByExpression` spécifie que le jeu de résultats sera classé par titre de cours dans le nom de service. Notez comment le nom du service est spécifié : `Department.Name`. Étant donné que l’association entre l’entité `Course` et l’entité `Department` est un-à-un, la propriété de navigation `Department` contient une entité `Department`. (S’il s’agissait d’une relation un-à-plusieurs, la propriété contiendrait une collection.) Pour récupérer le nom du service, vous devez spécifier la propriété `Name` de l’entité `Department`.

Enfin, ajoutez un contrôle de `GridView` pour afficher la liste des cours :

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample8.aspx)]

La première colonne est un champ de modèle qui affiche le nom du service. L’expression DataBinding spécifie `Department.Name`, comme vous l’avez vu dans le contrôle `QueryExtender`.

Exécutez la page. L’affichage initial affiche une liste de tous les cours par service, puis par titre de cours.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image29.png)

Entrez un « m », puis cliquez sur **Rechercher** pour afficher tous les cours dont les titres commencent par « m » (la recherche ne respecte pas la casse).

[![Image12](the-entity-framework-and-aspnet-getting-started-part-3/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image31.png)

## <a name="using-the-like-operator-to-filter-data"></a>Utilisation de l’opérateur « Like » pour filtrer les données

Vous pouvez obtenir un effet similaire aux types de recherche `StartsWith`, `Contains`et `EndsWith` du contrôle `QueryExtender` à l’aide d’un opérateur `Like` dans la propriété `EntityDataSource` du contrôle `Where`. Dans cette partie du didacticiel, vous allez apprendre à utiliser l’opérateur `Like` pour rechercher un élève par nom.

Ouvrez *students. aspx* en mode **source** . Après le contrôle `GridView`, ajoutez le balisage suivant :

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample9.aspx)]

Ce balisage est similaire à ce que vous avez vu précédemment, à l’exception de la valeur de propriété `Where`. La deuxième partie de l’expression `Where` définit une recherche de sous-chaîne (`LIKE %FirstMidName% or LIKE %LastName%`) qui recherche le prénom et le nom de ce qui est entré dans la zone de texte.

Exécutez la page. Au départ, vous voyez tous les élèves, car la valeur par défaut du paramètre `StudentName` est « % ».

[![Image13](the-entity-framework-and-aspnet-getting-started-part-3/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image33.png)

Entrez la lettre « g » dans la zone de texte, puis cliquez sur **Rechercher**. Vous voyez une liste des étudiants dont le prénom ou le nom contient un « g ».

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image35.png)

Vous pouvez désormais afficher, mettre à jour, filtrer, trier et regrouper des données à partir de tables individuelles. Dans le didacticiel suivant, vous commencerez à travailler avec des données associées (scénarios maître/détail).

> [!div class="step-by-step"]
> [Précédent](the-entity-framework-and-aspnet-getting-started-part-2.md)
> [Suivant](the-entity-framework-and-aspnet-getting-started-part-4.md)
