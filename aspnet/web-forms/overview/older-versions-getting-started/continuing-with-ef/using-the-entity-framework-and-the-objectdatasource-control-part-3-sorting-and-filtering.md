---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 'Utilisation de l’Entity Framework 4,0 et du contrôle ObjectDataSource, partie 3 : tri et filtrage | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels s’appuie sur l’application Web Contoso University qui est créée par le Prise en main avec la série de didacticiels Entity Framework 4,0. ...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: 603120864528b9a5ff81214270eb9a7f1b68b347
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78631669"
---
# <a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a>Utilisation de l’Entity Framework 4,0 et du contrôle ObjectDataSource, partie 3 : tri et filtrage

par [Tom Dykstra](https://github.com/tdykstra)

> Cette série de didacticiels s’appuie sur l’application Web Contoso University qui est créée par le [prise en main avec la](https://asp.net/entity-framework/tutorials#Getting%20Started) série de didacticiels Entity Framework 4,0. Si vous n’avez pas terminé les didacticiels précédents, comme point de départ pour ce didacticiel, vous pouvez [Télécharger l’application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que vous auriez créée. Vous pouvez également [Télécharger l’application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) qui est créée par la série de didacticiels complète. Si vous avez des questions sur les didacticiels, vous pouvez les poster sur le [Forum de Entity Framework ASP.net](https://forums.asp.net/1227.aspx).

Dans le didacticiel précédent, vous avez implémenté le modèle de référentiel dans une application Web multiniveau qui utilise les Entity Framework et le contrôle `ObjectDataSource`. Ce didacticiel montre comment trier et filtrer et gérer les scénarios maître/détail. Vous allez ajouter les améliorations suivantes à la page *Departments. aspx* :

- Une zone de texte pour permettre aux utilisateurs de sélectionner des services par nom.
- Liste des cours pour chaque service affiché dans la grille.
- Possibilité de trier en cliquant sur les en-têtes de colonne.

[![image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)

## <a name="adding-the-ability-to-sort-gridview-columns"></a>Ajout de la possibilité de trier des colonnes GridView

Ouvrez la page *Departments. aspx* et ajoutez un attribut `SortParameterName="sortExpression"` au contrôle `ObjectDataSource` nommé `DepartmentsObjectDataSource`. (Par la suite, vous créerez une méthode `GetDepartments` qui accepte un paramètre nommé `sortExpression`.) Le balisage de la balise d’ouverture du contrôle ressemble maintenant à l’exemple suivant.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

Ajoutez l’attribut `AllowSorting="true"` à la balise d’ouverture du contrôle `GridView`. Le balisage de la balise d’ouverture du contrôle ressemble maintenant à l’exemple suivant.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

Dans *Departments.aspx.cs*, définissez l’ordre de tri par défaut en appelant la méthode `Sort` du contrôle `GridView` à partir de la méthode `Page_Load` :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

Vous pouvez ajouter du code qui effectue un tri ou un filtrage dans la classe de logique métier ou le référentiel. Si vous le faites dans la classe de logique métier, le tri ou le filtrage est effectué une fois les données récupérées de la base de données, car la classe de logique métier utilise un objet `IEnumerable` retourné par le référentiel. Si vous ajoutez du code de tri et de filtrage dans la classe de référentiel et que vous le faites avant qu’une expression LINQ ou une requête d’objet ait été convertie en objet `IEnumerable`, vos commandes sont transmises à la base de données à des fins de traitement, ce qui est généralement plus efficace. Dans ce didacticiel, vous allez implémenter le tri et le filtrage de manière à ce que la base de données effectue le traitement, c’est-à-dire dans le référentiel.

Pour ajouter une fonctionnalité de tri, vous devez ajouter une nouvelle méthode à l’interface de référentiel et aux classes de référentiel, ainsi qu’à la classe de logique métier. Dans le fichier *ISchoolRepository.cs* , ajoutez une nouvelle méthode `GetDepartments` qui accepte un paramètre `sortExpression` qui sera utilisé pour trier la liste des services renvoyés :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

Le paramètre `sortExpression` spécifie la colonne à utiliser pour le tri et le sens du tri.

Ajoutez le code de la nouvelle méthode au fichier *SchoolRepository.cs* :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

Modifiez la méthode `GetDepartments` sans paramètre existante pour appeler la nouvelle méthode :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

Dans le projet de test, ajoutez la nouvelle méthode suivante à *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

Si vous souhaitez créer des tests unitaires qui dépendent de cette méthode retournant une liste triée, vous devez trier la liste avant de la retourner. Comme vous ne créez pas de tests comme celui de ce didacticiel, la méthode peut simplement retourner la liste non triée des services.

Dans le fichier *SchoolBL.cs* , ajoutez la nouvelle méthode suivante à la classe de logique métier :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

Ce code transmet le paramètre de tri à la méthode de référentiel.

Exécutez la page *Departments. aspx* .

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)

Vous pouvez maintenant cliquer sur n’importe quel en-tête de colonne pour effectuer un tri sur cette colonne. Si la colonne est déjà triée, le fait de cliquer sur l’en-tête inverse le sens de tri.

## <a name="adding-a-search-box"></a>Ajout d’une zone de recherche

Dans cette section, vous allez ajouter une zone de texte de recherche, la lier au contrôle `ObjectDataSource` à l’aide d’un paramètre de contrôle et ajouter une méthode à la classe de logique métier pour prendre en charge le filtrage.

Ouvrez la page *Departments. aspx* et ajoutez le balisage suivant entre le titre et le premier contrôle `ObjectDataSource` :

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

Dans le contrôle `ObjectDataSource` nommé `DepartmentsObjectDataSource`, procédez comme suit :

- Ajoutez un élément `SelectParameters` pour un paramètre nommé `nameSearchString` qui obtient la valeur entrée dans le contrôle `SearchTextBox`.
- Remplacez la valeur de l’attribut `SelectMethod` par `GetDepartmentsByName`. (Vous allez créer cette méthode ultérieurement.)

Le balisage du contrôle `ObjectDataSource` ressemble maintenant à l’exemple suivant :

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

Dans *ISchoolRepository.cs*, ajoutez une méthode `GetDepartmentsByName` qui accepte à la fois les paramètres `sortExpression` et `nameSearchString` :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

Dans *SchoolRepository.cs*, ajoutez la nouvelle méthode suivante :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

Ce code utilise une méthode `Where` pour sélectionner les éléments qui contiennent la chaîne recherchée. Si la chaîne de recherche est vide, tous les enregistrements sont sélectionnés. Notez que lorsque vous spécifiez des appels de méthode ensemble dans une instruction comme celle-ci (`Include`, `OrderBy`, puis `Where`), la méthode `Where` doit toujours être la dernière.

Modifiez la méthode `GetDepartments` existante qui accepte un paramètre `sortExpression` pour appeler la nouvelle méthode :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

Dans *MockSchoolRepository.cs* dans le projet de test, ajoutez la nouvelle méthode suivante :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

Dans *SchoolBL.cs*, ajoutez la nouvelle méthode suivante :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

Exécutez la page *Departments. aspx* et entrez une chaîne de recherche pour vous assurer que la logique de sélection fonctionne. Laissez la zone de texte vide et essayez d’effectuer une recherche pour vous assurer que tous les enregistrements sont retournés.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)

## <a name="adding-a-details-column-for-each-grid-row"></a>Ajout d’une colonne de détails pour chaque ligne de grille

Ensuite, vous souhaitez afficher tous les cours pour chaque département affiché dans la cellule de droite de la grille. Pour ce faire, vous allez utiliser un contrôle de `GridView` imbriqué et le lier aux données à partir de la propriété de navigation `Courses` de l’entité `Department`.

Ouvrez *Departments. aspx* et dans le balisage pour le contrôle `GridView`, spécifiez un gestionnaire pour l’événement `RowDataBound`. Le balisage de la balise d’ouverture du contrôle ressemble maintenant à l’exemple suivant.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

Ajoutez un nouvel élément `TemplateField` après le champ modèle `Administrator` :

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

Ce balisage crée un contrôle de `GridView` imbriqué qui affiche le numéro de cours et le titre d’une liste de cours. Elle ne spécifie pas de source de données, car vous allez la lier au code dans le gestionnaire de `RowDataBound`.

Ouvrez *Departments.aspx.cs* et ajoutez le gestionnaire suivant pour l’événement `RowDataBound` :

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

Ce code obtient l’entité `Department` à partir des arguments d’événement, convertit la propriété de navigation `Courses` en collection `List` et établit le `GridView` imbriqué à la collection.

Ouvrez le fichier *SchoolRepository.cs* et spécifiez le chargement hâtif pour la propriété de navigation `Courses` en appelant la méthode `Include` dans la requête d’objet que vous créez dans la méthode `GetDepartmentsByName`. L’instruction `return` dans la méthode `GetDepartmentsByName` ressemble désormais à l’exemple suivant.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

Exécutez la page. En plus de la fonctionnalité de tri et de filtrage que vous avez ajoutée précédemment, le contrôle GridView affiche désormais des détails de cours imbriqués pour chaque service.

[![image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)

Cela termine l’introduction aux scénarios de tri, de filtrage et de maître/détail. Dans le didacticiel suivant, vous verrez comment gérer l’accès concurrentiel.

> [!div class="step-by-step"]
> [Précédent](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [Suivant](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
