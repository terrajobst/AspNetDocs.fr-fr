---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-vb
title: Mise à jour du TableAdapter pour utiliser des JOINTUREs (VB) | Microsoft Docs
author: rick-anderson
description: Lors de l’utilisation d’une base de données, il est courant de demander des données réparties sur plusieurs tables. Pour récupérer des données de deux tables différentes, nous pouvons les utiliser...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: e624a3e0-061b-4efc-8b0e-5877f9ff6714
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-vb
msc.type: authoredcontent
ms.openlocfilehash: 5c94baa99b126cdd24d69afc3d02bfe8b069419b
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74604210"
---
# <a name="updating-the-tableadapter-to-use-joins-vb"></a>Mise à jour du TableAdapter pour l’utilisation de jointures (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_69_VB.zip) ou [Télécharger le PDF](updating-the-tableadapter-to-use-joins-vb/_static/datatutorial69vb1.pdf)

> Lors de l’utilisation d’une base de données, il est courant de demander des données réparties sur plusieurs tables. Pour extraire des données de deux tables différentes, vous pouvez utiliser une sous-requête corrélée ou une opération de jointure. Dans ce didacticiel, nous comparons les sous-requêtes corrélées et la syntaxe de jointure avant d’examiner comment créer un TableAdapter qui comprend une jointure dans sa requête principale.

## <a name="introduction"></a>Introduction

Avec les bases de données relationnelles, les données qui nous intéressent sont souvent réparties sur plusieurs tables. Par exemple, lors de l’affichage des informations sur les produits, nous souhaitons probablement répertorier les noms des catégories et des fournisseurs correspondants pour chaque produit. La table `Products` a des valeurs `CategoryID` et `SupplierID`, mais les noms des catégories et des fournisseurs réels sont respectivement dans les tables `Categories` et `Suppliers`.

Pour récupérer des informations à partir d’une autre table associée, nous pouvons soit utiliser des sous- *requêtes corrélées* , soit `JOIN`*s*. Une sous-requête corrélée est une requête de `SELECT` imbriquée qui fait référence à des colonnes dans la requête externe. Par exemple, dans le didacticiel [création d’une couche d’accès aux données](../introduction/creating-a-data-access-layer-vb.md) , nous avons utilisé deux sous-requêtes corrélées dans la requête principale de `ProductsTableAdapter` s pour retourner les noms des catégories et des fournisseurs pour chaque produit. Une `JOIN` est une construction SQL qui fusionne les lignes associées de deux tables différentes. Nous avons utilisé une `JOIN` dans le didacticiel sur l' [interrogation des données avec le contrôle SqlDataSource](../accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb.md) pour afficher les informations de catégorie à côté de chaque produit.

La raison de l’utilisation de `JOIN` s avec les TableAdapters est due aux limitations de l’Assistant TableAdapter s pour générer automatiquement les instructions `INSERT`, `UPDATE`et `DELETE` correspondantes. Plus précisément, si la requête principale du TableAdapter contient des `JOIN` s, le TableAdapter ne peut pas créer automatiquement les instructions SQL ou les procédures stockées ad hoc pour ses propriétés `InsertCommand`, `UpdateCommand`et `DeleteCommand`.

Dans ce didacticiel, nous allons brièvement comparer et opposer les sous-requêtes corrélées et les `JOIN` s avant d’explorer la création d’un TableAdapter qui comprend `JOIN` s dans sa requête principale.

## <a name="comparing-and-contrasting-correlated-subqueries-andjoin-s"></a>Comparaison et différences entre les sous-requêtes corrélées et les`JOIN` s

Rappelez-vous que le `ProductsTableAdapter` créé dans le premier didacticiel du jeu de données `Northwind` utilise des sous-requêtes corrélées pour rétablir chaque nom de catégorie et de fournisseur correspondant au produit. La requête principale `ProductsTableAdapter` s est indiquée ci-dessous.

[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample1.sql)]

Les deux sous-requêtes corrélées-`(SELECT CategoryName FROM Categories WHERE Categories.CategoryID = Products.CategoryID)` et `(SELECT CompanyName FROM Suppliers WHERE Suppliers.SupplierID = Products.SupplierID)` sont des requêtes `SELECT` qui renvoient une valeur unique par produit sous la forme d’une colonne supplémentaire dans la liste des colonnes de l’instruction de `SELECT` externe.

Une `JOIN` peut également être utilisée pour retourner chaque fournisseur et nom de catégorie du produit. La requête suivante retourne la même sortie que la sortie ci-dessus, mais utilise `JOIN` s à la place des sous-requêtes :

[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample2.sql)]

Un `JOIN` fusionne les enregistrements d’une table avec les enregistrements d’une autre table en fonction de certains critères. Dans la requête ci-dessus, par exemple, le `LEFT JOIN Categories ON Categories.CategoryID = Products.CategoryID` indique à SQL Server de fusionner chaque enregistrement de produit avec l’enregistrement de catégorie dont la valeur `CategoryID` correspond à la valeur `CategoryID` du produit. Le résultat fusionné nous permet de travailler avec les champs de catégorie correspondants pour chaque produit (par exemple, `CategoryName`).

> [!NOTE]
> les `JOIN` s sont couramment utilisés lors de l’interrogation de données à partir de bases de données relationnelles. Si vous ne connaissez pas la syntaxe de la `JOIN` ou si vous avez besoin de monter un peu sur son utilisation, j’ai recommandé le didacticiel sur la [jointure SQL](http://www.w3schools.com/sql/sql_join.asp) dans [W3 Schools](http://www.w3schools.com/). Il est également utile de lire les sections notions de base des [`JOIN`](https://msdn.microsoft.com/library/ms191517.aspx) et [sous-requêtes](https://msdn.microsoft.com/library/ms189575.aspx) de la [documentation en ligne de SQL](https://msdn.microsoft.com/library/ms130214.aspx).

Étant donné que les `JOIN` s et les sous-requêtes corrélées peuvent être utilisées pour récupérer des données associées à partir d’autres tables, de nombreux développeurs n’ont pas oublié leurs têtes et se demandent quelle approche utiliser. Tous les gourous SQL que j’ai parlés ont dit à peu près la même chose, qu’il n’a pas vraiment d’importance pour les performances, car SQL Server produira approximativement des plans d’exécution identiques. Leur avis est ensuite d’utiliser la technique avec laquelle vous et votre équipe êtes le plus à l’aise. Il est de plus en plus à noter qu’après avoir fait part de ces conseils, ces experts expriment immédiatement leur préférence de `JOIN` s par rapport à des sous-requêtes corrélées.

Lorsque vous créez une couche d’accès aux données à l’aide de DataSets typés, les outils fonctionnent mieux lorsque vous utilisez des sous-requêtes. En particulier, l’Assistant TableAdapter s ne génère pas automatiquement les instructions `INSERT`, `UPDATE`et `DELETE` correspondantes si la requête principale contient des `JOIN` s, mais génère automatiquement ces instructions lorsque des sous-requêtes corrélées sont utilisées.

Pour explorer cette lacune, créez un jeu de données temporaire typé dans le dossier `~/App_Code/DAL`. Pendant l’Assistant Configuration de TableAdapter, choisissez d’utiliser des instructions SQL ad hoc et entrez la requête `SELECT` suivante (voir la figure 1) :

[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample3.sql)]

[![entrez une requête principale qui contient des JOINTUREs](updating-the-tableadapter-to-use-joins-vb/_static/image2.png)](updating-the-tableadapter-to-use-joins-vb/_static/image1.png)

**Figure 1**: entrer une requête principale contenant `JOIN` s ([cliquez pour afficher l’image en taille réelle](updating-the-tableadapter-to-use-joins-vb/_static/image3.png))

Par défaut, le TableAdapter crée automatiquement les instructions `INSERT`, `UPDATE`et `DELETE` en fonction de la requête principale. Si vous cliquez sur le bouton avancé, vous pouvez voir que cette fonctionnalité est activée. Malgré ce paramètre, le TableAdapter ne peut pas créer les instructions `INSERT`, `UPDATE`et `DELETE`, car la requête principale contient un `JOIN`.

![Entrer une requête principale qui contient des JOINTUREs](updating-the-tableadapter-to-use-joins-vb/_static/image4.png)

**Figure 2**: entrer une requête principale contenant `JOIN` s

Cliquez sur Terminer pour terminer l'Assistant. À ce stade, le concepteur de votre jeu de données inclut un TableAdapter unique avec un DataTable avec des colonnes pour chacun des champs retournés dans la liste de colonnes `SELECT` requête s. Cela comprend les `CategoryName` et les `SupplierName`, comme illustré dans la figure 3.

![Le DataTable contient une colonne pour chaque champ retourné dans la liste des colonnes](updating-the-tableadapter-to-use-joins-vb/_static/image5.png)

**Figure 3**: le DataTable contient une colonne pour chaque champ retourné dans la liste des colonnes

Tandis que le DataTable contient les colonnes appropriées, le TableAdapter n’a pas de valeurs pour ses propriétés `InsertCommand`, `UpdateCommand`et `DeleteCommand`. Pour confirmer cela, cliquez sur le TableAdapter dans le concepteur, puis accédez au Fenêtre Propriétés. Vous verrez que les propriétés `InsertCommand`, `UpdateCommand`et `DeleteCommand` sont définies sur (aucune).

[![les propriétés InsertCommand, UpdateCommand et DeleteCommand ont la valeur (None)](updating-the-tableadapter-to-use-joins-vb/_static/image7.png)](updating-the-tableadapter-to-use-joins-vb/_static/image6.png)

**Figure 4**: les propriétés `InsertCommand`, `UpdateCommand`et `DeleteCommand` sont définies sur (aucune) ([cliquez pour afficher l’image en taille réelle](updating-the-tableadapter-to-use-joins-vb/_static/image8.png))

Pour contourner ce manque, nous pouvons fournir manuellement les instructions et les paramètres SQL pour les propriétés `InsertCommand`, `UpdateCommand`et `DeleteCommand` via le Fenêtre Propriétés. Vous pouvez également commencer par configurer la requête principale du TableAdapter pour qu’il *n'* inclut aucun `JOIN` s. Cela permet aux instructions `INSERT`, `UPDATE`et `DELETE` d’être générées automatiquement pour nous. À la fin de l’Assistant, nous pourrions ensuite mettre à jour manuellement les `SelectCommand` du TableAdapter à partir de la Fenêtre Propriétés afin qu’il inclue la syntaxe de `JOIN`.

Bien que cette approche fonctionne, elle est très fragile quand vous utilisez des requêtes SQL ad hoc, car chaque fois que la requête principale du TableAdapter est reconfigurée par le biais de l’Assistant, les instructions générées automatiquement `INSERT`, `UPDATE`et `DELETE` sont recréées. Cela signifie que toutes les personnalisations que nous avons effectuées par la suite seraient perdues si vous cliquez avec le bouton droit sur le TableAdapter, que vous avez choisi configurer dans le menu contextuel et que vous avez reterminé l’Assistant.

La fragilité des instructions générées automatiquement `INSERT`, `UPDATE`et `DELETE` du TableAdapter est, heureusement, limitée aux instructions SQL ad hoc. Si votre TableAdapter utilise des procédures stockées, vous pouvez personnaliser les procédures stockées `SelectCommand`, `InsertCommand`, `UpdateCommand`ou `DeleteCommand`, puis réexécuter l’Assistant Configuration de TableAdapter sans craindre que les procédures stockées soient modifiées.

Au cours des différentes étapes suivantes, nous allons créer un TableAdapter qui, à l’origine, utilise une requête principale qui omet tout `JOIN` s afin que les procédures stockées d’insertion, de mise à jour et de suppression correspondantes soient générées automatiquement. Nous allons ensuite mettre à jour le `SelectCommand` afin que utilise un `JOIN` qui retourne des colonnes supplémentaires à partir de tables associées. Enfin, nous allons créer une classe de couche de logique métier correspondante et illustrer l’utilisation du TableAdapter dans une page Web ASP.NET.

## <a name="step-1-creating-the-tableadapter-using-a-simplified-main-query"></a>Étape 1 : création du TableAdapter à l’aide d’une requête principale simplifiée

Pour ce didacticiel, nous allons ajouter un TableAdapter et un DataTable fortement typés pour la table `Employees` dans le jeu de données `NorthwindWithSprocs`. La table `Employees` contient un champ `ReportsTo` qui spécifie la `EmployeeID` du gestionnaire des employés. Par exemple, l’employé Anne Dodsworth a une valeur `ReportTo` de 5, qui est la `EmployeeID` de Steven Buchanan. Par conséquent, Anne signale à Steven, son responsable. En plus de signaler chaque valeur de `ReportsTo` d’employé, nous pouvons également récupérer le nom de son responsable. Cela peut être accompli à l’aide d’un `JOIN`. Toutefois, l’utilisation d’un `JOIN` lors de la création initiale du TableAdapter empêche l’Assistant de générer automatiquement les fonctionnalités d’insertion, de mise à jour et de suppression correspondantes. Par conséquent, nous allons commencer par créer un TableAdapter dont la requête principale ne contient aucun `JOIN` s. Ensuite, à l’étape 2, nous allons mettre à jour la procédure stockée principale de la requête pour récupérer le nom du responsable via un `JOIN`.

Commencez par ouvrir le jeu de données `NorthwindWithSprocs` dans le dossier `~/App_Code/DAL`. Cliquez avec le bouton droit sur le concepteur, sélectionnez l’option Ajouter dans le menu contextuel, puis choisissez l’élément de menu TableAdapter. L’Assistant Configuration de TableAdapter est lancé. Comme illustré à la figure 5, utilisez l’Assistant pour créer de nouvelles procédures stockées, puis cliquez sur suivant. Pour obtenir un actualisateur sur la création de nouvelles procédures stockées à partir de l’Assistant TableAdapter, consultez le didacticiel [création de nouvelles procédures stockées pour les TableAdapters de DataSet typé](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) .

[![sélectionnez l’option créer de nouvelles procédures stockées](updating-the-tableadapter-to-use-joins-vb/_static/image10.png)](updating-the-tableadapter-to-use-joins-vb/_static/image9.png)

**Figure 5**: sélectionnez l’option créer de nouvelles procédures stockées ([cliquez pour afficher l’image en taille réelle](updating-the-tableadapter-to-use-joins-vb/_static/image11.png))

Utilisez l’instruction `SELECT` suivante pour la requête principale du TableAdapter :

[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample4.sql)]

Étant donné que cette requête n’inclut aucun `JOIN`, l’Assistant TableAdapter crée automatiquement des procédures stockées avec les instructions `INSERT`, `UPDATE`et `DELETE` correspondantes, ainsi qu’une procédure stockée pour l’exécution de la requête principale.

L’étape suivante nous permet de nommer les procédures stockées du TableAdapter. Utilisez les noms `Employees_Select`, `Employees_Insert`, `Employees_Update`et `Employees_Delete`, comme illustré à la figure 6.

[![nom des procédures stockées du TableAdapter](updating-the-tableadapter-to-use-joins-vb/_static/image13.png)](updating-the-tableadapter-to-use-joins-vb/_static/image12.png)

**Figure 6**: nommer les procédures stockées du TableAdapter ([cliquez pour afficher l’image en taille réelle](updating-the-tableadapter-to-use-joins-vb/_static/image14.png))

L’étape finale nous invite à nommer les méthodes de TableAdapter s. Utilisez `Fill` et `GetEmployees` comme noms de méthode. Veillez également à activer la case à cocher créer des méthodes pour envoyer directement des mises à jour à la base de données (GenerateDBDirectMethods).

[Nom de l' ![les méthodes Fill et GetEmployees du TableAdapter](updating-the-tableadapter-to-use-joins-vb/_static/image16.png)](updating-the-tableadapter-to-use-joins-vb/_static/image15.png)

**Figure 7**: nommer les méthodes de TableAdapter s `Fill` et `GetEmployees` ([cliquez pour afficher l’image en taille réelle](updating-the-tableadapter-to-use-joins-vb/_static/image17.png))

Après avoir terminé l’Assistant, prenez un moment pour examiner les procédures stockées dans la base de données. Vous devez en voir quatre nouvelles : `Employees_Select`, `Employees_Insert`, `Employees_Update`et `Employees_Delete`. Ensuite, examinez le `EmployeesDataTable` et `EmployeesTableAdapter` venez de créer. Le DataTable contient une colonne pour chaque champ retourné par la requête principale. Cliquez sur le TableAdapter, puis accédez au Fenêtre Propriétés. Vous verrez que les propriétés `InsertCommand`, `UpdateCommand`et `DeleteCommand` sont correctement configurées pour appeler les procédures stockées correspondantes.

[![le TableAdapter comprend des fonctionnalités d’insertion, de mise à jour et de suppression](updating-the-tableadapter-to-use-joins-vb/_static/image19.png)](updating-the-tableadapter-to-use-joins-vb/_static/image18.png)

**Figure 8**: le TableAdapter comprend des fonctionnalités d’insertion, de mise à jour et de suppression ([cliquez pour afficher l’image en taille réelle](updating-the-tableadapter-to-use-joins-vb/_static/image20.png))

Une fois les procédures stockées INSERT, Update et Delete créées automatiquement et les propriétés `InsertCommand`, `UpdateCommand`et `DeleteCommand` correctement configurées, nous sommes prêts à personnaliser la procédure stockée `SelectCommand` s pour qu’elle renvoie des informations supplémentaires sur chaque gestionnaire des employés. En particulier, nous devons mettre à jour la procédure stockée `Employees_Select` pour utiliser une `JOIN` et retourner les valeurs `FirstName` et `LastName` du gestionnaire. Après la mise à jour de la procédure stockée, nous devons mettre à jour le DataTable afin qu’il inclue ces colonnes supplémentaires. Nous allons aborder ces deux tâches aux étapes 2 et 3.

## <a name="step-2-customizing-the-stored-procedure-to-include-ajoin"></a>Étape 2 : personnalisation de la procédure stockée pour inclure une`JOIN`

Commencez par accéder à la Explorateur de serveurs, en explorant le dossier des procédures stockées de la base de données Northwind et en ouvrant la procédure stockée `Employees_Select`. Si vous ne voyez pas cette procédure stockée, cliquez avec le bouton droit sur le dossier procédures stockées, puis choisissez actualiser. Mettez à jour la procédure stockée pour qu’elle utilise une `LEFT JOIN` pour retourner le nom et le prénom du gestionnaire :

[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample5.sql)]

Après avoir mis à jour l’instruction `SELECT`, enregistrez les modifications en accédant au menu fichier, puis en choisissant enregistrer `Employees_Select`. Vous pouvez également cliquer sur l’icône Enregistrer dans la barre d’outils ou appuyer sur CTRL + S. Après avoir enregistré vos modifications, cliquez avec le bouton droit sur la procédure stockée `Employees_Select` dans le Explorateur de serveurs puis sélectionnez Exécuter. Cette opération exécute la procédure stockée et affiche ses résultats dans la fenêtre sortie (voir figure 9).

[![les résultats des procédures stockées s’affichent dans la Fenêtre Sortie](updating-the-tableadapter-to-use-joins-vb/_static/image22.png)](updating-the-tableadapter-to-use-joins-vb/_static/image21.png)

**Figure 9**: les résultats des procédures stockées s’affichent dans la fenêtre sortie ([cliquez pour afficher l’image en taille réelle](updating-the-tableadapter-to-use-joins-vb/_static/image23.png))

## <a name="step-3-updating-the-datatable-s-columns"></a>Étape 3 : mise à jour des colonnes du DataTable s

À ce stade, la procédure stockée `Employees_Select` retourne `ManagerFirstName` et `ManagerLastName` valeurs, mais ces colonnes ne sont pas présentes dans le `EmployeesDataTable`. Ces colonnes manquantes peuvent être ajoutées à l’objet DataTable de l’une des deux manières suivantes :

- **Manuellement** , cliquez avec le bouton droit sur le DataTable dans le concepteur de DataSet et, dans le menu Ajouter, choisissez colonne. Vous pouvez ensuite nommer la colonne et définir ses propriétés en conséquence.
- **Automatiquement** : l’Assistant Configuration de TableAdapter met à jour les colonnes DataTable s pour refléter les champs retournés par la procédure stockée `SelectCommand`. Lorsque vous utilisez des instructions SQL ad hoc, l’Assistant supprime également les propriétés `InsertCommand`, `UpdateCommand`et `DeleteCommand`, puisque le `SelectCommand` contient maintenant une `JOIN`. Toutefois, lorsque vous utilisez des procédures stockées, ces propriétés de commande restent intactes.

Nous avons exploré manuellement l’ajout de colonnes de DataTable dans les didacticiels précédents, notamment le [maître/détail à l’aide d’une liste à puces d’enregistrements principaux avec les détails DataList](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) et le [chargement de fichiers](../working-with-binary-files/uploading-files-vb.md), et nous examinerons à nouveau ce processus plus en détail dans le didacticiel suivant. Pour ce didacticiel, toutefois, nous allons utiliser l’approche automatique via l’Assistant Configuration de TableAdapter.

Commencez par cliquer avec le bouton droit sur le `EmployeesTableAdapter` et sélectionnez configurer dans le menu contextuel. L’Assistant Configuration de TableAdapter s’affiche, qui répertorie les procédures stockées utilisées pour la sélection, l’insertion, la mise à jour et la suppression, ainsi que les valeurs de retour et les paramètres (le cas échéant). La figure 10 illustre cet Assistant. Ici, nous pouvons voir que la procédure stockée `Employees_Select` retourne désormais les champs `ManagerFirstName` et `ManagerLastName`.

[![l’Assistant affiche la liste des colonnes mises à jour pour la procédure stockée Employees_Select](updating-the-tableadapter-to-use-joins-vb/_static/image25.png)](updating-the-tableadapter-to-use-joins-vb/_static/image24.png)

**Figure 10**: l’Assistant affiche la liste des colonnes mises à jour pour la procédure stockée `Employees_Select` ([cliquez pour afficher l’image en taille réelle](updating-the-tableadapter-to-use-joins-vb/_static/image26.png))

Terminez l’Assistant en cliquant sur Terminer. Lors du retour au concepteur de DataSet, le `EmployeesDataTable` comprend deux colonnes supplémentaires : `ManagerFirstName` et `ManagerLastName`.

[![EmployeesDataTable contient deux nouvelles colonnes](updating-the-tableadapter-to-use-joins-vb/_static/image28.png)](updating-the-tableadapter-to-use-joins-vb/_static/image27.png)

**Figure 11**: le `EmployeesDataTable` contient deux nouvelles colonnes ([cliquez pour afficher l’image en taille réelle](updating-the-tableadapter-to-use-joins-vb/_static/image29.png))

Pour illustrer que la procédure stockée `Employees_Select` mise à jour est en vigueur et que les fonctionnalités d’insertion, de mise à jour et de suppression du TableAdapter sont toujours fonctionnelles, créez une page Web qui permet aux utilisateurs d’afficher et de supprimer des employés. Avant de créer une telle page, toutefois, nous devons d’abord créer une nouvelle classe dans la couche de logique métier pour travailler avec des employés à partir du jeu de données `NorthwindWithSprocs`. À l’étape 4, nous allons créer une classe `EmployeesBLLWithSprocs`. À l’étape 5, nous utiliserons cette classe à partir d’une page ASP.NET.

## <a name="step-4-implementing-the-business-logic-layer"></a>Étape 4 : implémentation de la couche de logique métier

Créez un nouveau fichier de classe dans le dossier `~/App_Code/BLL` nommé `EmployeesBLLWithSprocs.vb`. Cette classe imite la sémantique de la classe de `EmployeesBLL` existante, seul celle-ci fournit moins de méthodes et utilise le jeu de données `NorthwindWithSprocs` (au lieu du DataSet `Northwind`). Ajoutez le code suivant à la classe `EmployeesBLLWithSprocs` .

[!code-vb[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample6.vb)]

La propriété de la classe `EmployeesBLLWithSprocs` s `Adapter` retourne une instance du `NorthwindWithSprocs` DataSet s `EmployeesTableAdapter`. Cela est utilisé par les méthodes `GetEmployees` et `DeleteEmployee`. La méthode `GetEmployees` appelle la méthode `GetEmployees` correspondante `EmployeesTableAdapter`, qui appelle la procédure stockée `Employees_Select` et remplit ses résultats dans un `EmployeeDataTable`. La méthode `DeleteEmployee` appelle de la même manière la méthode `EmployeesTableAdapter` s `Delete`, qui appelle la procédure stockée `Employees_Delete`.

## <a name="step-5-working-with-the-data-in-the-presentation-layer"></a>Étape 5 : utilisation des données dans la couche de présentation

Une fois la classe `EmployeesBLLWithSprocs` terminée, nous sommes prêts à travailler avec les données des employés via une page ASP.NET. Ouvrez la page `JOINs.aspx` dans le dossier `AdvancedDAL` et faites glisser un contrôle GridView de la boîte à outils vers le concepteur, en affectant à sa propriété `ID` la valeur `Employees`. Ensuite, à partir de la balise active GridView s, liez la grille à un nouveau contrôle ObjectDataSource nommé `EmployeesDataSource`.

Configurez l’ObjectDataSource pour utiliser la classe `EmployeesBLLWithSprocs` et, à partir des onglets sélectionner et supprimer, assurez-vous que les méthodes `GetEmployees` et `DeleteEmployee` sont sélectionnées dans les listes déroulantes. Cliquez sur Terminer pour terminer la configuration de ObjectDataSource s.

[![configurer ObjectDataSource pour utiliser la classe EmployeesBLLWithSprocs](updating-the-tableadapter-to-use-joins-vb/_static/image31.png)](updating-the-tableadapter-to-use-joins-vb/_static/image30.png)

**Figure 12**: configurer ObjectDataSource pour utiliser la classe `EmployeesBLLWithSprocs` ([cliquez pour afficher l’image en taille réelle](updating-the-tableadapter-to-use-joins-vb/_static/image32.png))

[![que ObjectDataSource utilise les méthodes GetEmployees et DeleteEmployee](updating-the-tableadapter-to-use-joins-vb/_static/image34.png)](updating-the-tableadapter-to-use-joins-vb/_static/image33.png)

**Figure 13**: avoir l’ObjectDataSource utiliser les méthodes `GetEmployees` et `DeleteEmployee` ([cliquez pour afficher l’image en taille réelle](updating-the-tableadapter-to-use-joins-vb/_static/image35.png))

Visual Studio ajoute un BoundField au GridView pour chaque colonne `EmployeesDataTable` s. Supprimez tous ces BoundFields, à l’exception de `Title`, `LastName`, `FirstName`, `ManagerFirstName`et `ManagerLastName` et renommez les propriétés `HeaderText` des quatre derniers BoundFields, respectivement, Last Name, First Name, Manager s First Name et Manager Names Name, respectivement.

Pour permettre aux utilisateurs de supprimer des employés de cette page, vous devez effectuer deux opérations. Tout d’abord, demandez au GridView de fournir des fonctionnalités de suppression en activant l’option Activer la suppression de sa balise active. Ensuite, modifiez la propriété ObjectDataSource s `OldValuesParameterFormatString` à partir de la valeur définie par l’Assistant ObjectDataSource (`original_{0}`) sur sa valeur par défaut (`{0}`). Une fois ces modifications effectuées, vos balises déclaratives GridView et ObjectDataSource doivent ressembler à ce qui suit :

[!code-aspx[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample7.aspx)]

Testez la page en la visitant via un navigateur. Comme le montre la figure 14, la page répertorie chaque employé et son nom de responsable (en supposant qu’il en possède un).

[![la jointure dans la procédure stockée Employees_Select renvoie le nom du responsable](updating-the-tableadapter-to-use-joins-vb/_static/image37.png)](updating-the-tableadapter-to-use-joins-vb/_static/image36.png)

**Figure 14**: la `JOIN` dans la procédure stockée `Employees_Select` renvoie le nom du responsable ([cliquez pour afficher l’image en taille réelle](updating-the-tableadapter-to-use-joins-vb/_static/image38.png))

Le fait de cliquer sur le bouton supprimer démarre le workflow de suppression, qui termine l’exécution de la procédure stockée `Employees_Delete`. Toutefois, l’instruction tentée `DELETE` dans la procédure stockée échoue en raison d’une violation de contrainte de clé étrangère (voir figure 15). Plus précisément, chaque employé possède un ou plusieurs enregistrements dans la table `Orders`, ce qui entraîne l’échec de la suppression.

[![la suppression d’un employé qui a des commandes correspondantes entraîne une violation de contrainte de clé étrangère](updating-the-tableadapter-to-use-joins-vb/_static/image40.png)](updating-the-tableadapter-to-use-joins-vb/_static/image39.png)

**Figure 15**: la suppression d’un employé qui a des commandes correspondantes entraîne une violation de contrainte de clé étrangère ([cliquez pour afficher l’image en taille réelle](updating-the-tableadapter-to-use-joins-vb/_static/image41.png))

Pour autoriser la suppression d’un employé, vous pouvez :

- Mettre à jour la contrainte de clé étrangère vers les suppressions en cascade,
- Supprimez manuellement les enregistrements de la table `Orders` pour les employés que vous souhaitez supprimer, ou
- Mettez à jour la procédure stockée `Employees_Delete` pour supprimer d’abord les enregistrements associés de la table `Orders` avant de supprimer l’enregistrement `Employees`. Nous avons abordé cette technique dans le didacticiel [utilisation de procédures stockées existantes pour les TableAdapters de DataSet typé](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) .

Je laisse cela en tant qu’exercice pour le lecteur.

## <a name="summary"></a>Récapitulatif

Lorsque vous utilisez des bases de données relationnelles, il est courant que les requêtes extraient leurs données à partir de plusieurs tables associées. Les sous-requêtes corrélées et les `JOIN` s fournissent deux techniques différentes pour accéder aux données à partir de tables associées dans une requête. Dans les didacticiels précédents, nous utilisons le plus souvent des sous-requêtes corrélées car le TableAdapter ne peut pas générer automatiquement les instructions `INSERT`, `UPDATE`et `DELETE` pour les requêtes impliquant `JOIN` s. Si ces valeurs peuvent être fournies manuellement, lorsque vous utilisez des instructions SQL ad hoc, toutes les personnalisations sont remplacées à la fin de l’Assistant Configuration de TableAdapter.

Heureusement, les TableAdapters créés à l’aide de procédures stockées ne souffrent pas de la même fragilité que celles créées à l’aide d’instructions SQL ad hoc. Par conséquent, il est possible de créer un TableAdapter dont la requête principale utilise un `JOIN` lors de l’utilisation de procédures stockées. Dans ce didacticiel, nous avons vu comment créer un TableAdapter de ce type. Nous avons commencé par utiliser une requête `JOIN``SELECT` pour la requête principale du TableAdapter afin que les procédures stockées d’insertion, de mise à jour et de suppression correspondantes soient créées automatiquement. Une fois la configuration initiale du TableAdapter terminée, nous avons augmenté la `SelectCommand` procédure stockée pour utiliser une `JOIN` et réexécuté l’Assistant Configuration de TableAdapter pour mettre à jour les colonnes `EmployeesDataTable` s.

La nouvelle exécution de l’Assistant Configuration de TableAdapter a mis à jour automatiquement les colonnes de `EmployeesDataTable` pour refléter les champs de données retournés par la procédure stockée `Employees_Select`. Vous pouvez également ajouter ces colonnes manuellement au DataTable. Nous allons explorer l’ajout manuel de colonnes au DataTable dans le prochain didacticiel.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel étaient Hilton Geisenow, David SURU et Teresa Murphy. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
> [Suivant](adding-additional-datatable-columns-vb.md)
