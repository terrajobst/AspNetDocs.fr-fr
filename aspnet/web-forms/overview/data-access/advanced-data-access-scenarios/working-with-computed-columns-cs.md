---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-cs
title: Utilisation des colonnes calculées (C#) | Microsoft Docs
author: rick-anderson
description: Lors de la création d’une table de base de données, Microsoft SQL Server vous permet de définir une colonne calculée dont la valeur est calculée à partir d’une expression qui fait généralement référence à...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: 57459065-ed7c-4dfe-ac9c-54c093abc261
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-cs
msc.type: authoredcontent
ms.openlocfilehash: ad6a96f2721510c2478f707c8eed018ae797f27a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74603257"
---
# <a name="working-with-computed-columns-c"></a>Utilisation de colonnes calculées (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_71_CS.zip) ou [Télécharger le PDF](working-with-computed-columns-cs/_static/datatutorial71cs1.pdf)

> Lors de la création d’une table de base de données, Microsoft SQL Server vous permet de définir une colonne calculée dont la valeur est calculée à partir d’une expression qui fait généralement référence à d’autres valeurs dans le même enregistrement de base de données. Ces valeurs sont en lecture seule dans la base de données, ce qui nécessite des considérations particulières lors de l’utilisation de TableAdapters. Dans ce didacticiel, nous allons apprendre à répondre aux défis posés par les colonnes calculées.

## <a name="introduction"></a>Introduction

Microsoft SQL Server permet les *[colonnes calculées](https://msdn.microsoft.com/library/ms191250.aspx)* , qui sont des colonnes dont les valeurs sont calculées à partir d’une expression qui fait généralement référence aux valeurs d’autres colonnes de la même table. Par exemple, un modèle de données de suivi temporel peut avoir une table nommée `ServiceLog` avec des colonnes incluant `ServicePerformed`, `EmployeeID`, `Rate`et `Duration`, entre autres. Bien que le montant dû par article de service (soit le taux multiplié par la durée) puisse être calculé par le biais d’une page Web ou d’une autre interface de programmation, il peut être pratique d’inclure une colonne dans la table `ServiceLog` nommée `AmountDue` qui a signalé ces informations. Cette colonne peut être créée en tant que colonne normale, mais elle doit être mise à jour chaque fois que la `Rate` ou `Duration` valeurs de colonne modifiées. Une meilleure approche consisterait à transformer la colonne `AmountDue` en colonne calculée à l’aide de l’expression `Rate * Duration`. En procédant ainsi, SQL Server calcule automatiquement la valeur de colonne `AmountDue` chaque fois qu’elle a été référencée dans une requête.

Dans la mesure où une valeur s de colonne calculée est déterminée par une expression, ces colonnes sont en lecture seule et ne peuvent donc pas avoir de valeurs qui leur sont affectées dans les instructions `INSERT` ou `UPDATE`. Toutefois, lorsque des colonnes calculées font partie de la requête principale pour un TableAdapter qui utilise des instructions SQL ad hoc, elles sont automatiquement incluses dans les instructions `INSERT` et `UPDATE` générées automatiquement. Par conséquent, les `INSERT` TableAdapter s et `UPDATE` et les propriétés `InsertCommand` et `UpdateCommand` doivent être mises à jour pour supprimer les références aux colonnes calculées.

L’un des défis liés à l’utilisation de colonnes calculées avec un TableAdapter qui utilise des instructions SQL ad hoc est que les requêtes TableAdapter `INSERT` et `UPDATE` sont automatiquement régénérées à chaque fois que l’Assistant Configuration de TableAdapter est terminé. Par conséquent, les colonnes calculées manuellement retirées du `INSERT` et `UPDATE` requêtes s’affichent à nouveau si l’Assistant est réexécuté. Bien que les TableAdapters qui utilisent des procédures stockées ne souffrent pas de cette fragilité, ils ont leurs propres excentriques que nous traiterons à l’étape 3.

Dans ce didacticiel, nous allons ajouter une colonne calculée à la table `Suppliers` de la base de données Northwind, puis créer un TableAdapter correspondant pour travailler avec cette table et sa colonne calculée. Notre TableAdapter utilise des procédures stockées plutôt que des instructions SQL ad hoc afin que nos personnalisations ne soient pas perdues lors de l’utilisation de l’Assistant Configuration de TableAdapter.

Commençons !

## <a name="step-1-adding-a-computed-column-to-thesupplierstable"></a>Étape 1 : ajout d’une colonne calculée à la table`Suppliers`

La base de données Northwind n’a pas de colonnes calculées. nous devons donc en ajouter une. Pour ce didacticiel, ajoutez une colonne calculée à la table `Suppliers` appelée `FullContactName` qui renvoie le nom du contact, le titre et la société pour laquelle il travaille dans le format suivant : `ContactName` (`ContactTitle`, `CompanyName`). Cette colonne calculée peut être utilisée dans les rapports lors de l’affichage d’informations sur les fournisseurs.

Commencez par ouvrir la définition de la table `Suppliers` en cliquant avec le bouton droit sur la table `Suppliers` dans la Explorateur de serveurs et en choisissant ouvrir la définition de table dans le menu contextuel. Cette opération affiche les colonnes de la table et leurs propriétés, telles que leur type de données, si elles autorisent `NULL`, etc. Pour ajouter une colonne calculée, commencez par taper le nom de la colonne dans la définition de la table. Ensuite, entrez l’expression dans la zone de texte (formule) sous la section Spécification de la colonne calculée de la colonne Fenêtre Propriétés (voir figure 1). Nommez la colonne calculée `FullContactName` et utilisez l’expression suivante :

[!code-sql[Main](working-with-computed-columns-cs/samples/sample1.sql)]

Notez que les chaînes peuvent être concaténées en SQL à l’aide de l’opérateur `+`. L’instruction `CASE` peut être utilisée comme une condition conditionnelle dans un langage de programmation traditionnel. Dans l’expression ci-dessus, l’instruction `CASE` peut être lue comme suit : si `ContactTitle` n’est pas `NULL` puis qu’elle génère la valeur `ContactTitle` concaténée avec une virgule, sinon, n’émette rien. Pour plus d’informations sur l’utilité de l’instruction `CASE`, consultez les instructions relatives à la [puissance des instructions SQL `CASE`](http://www.4guysfromrolla.com/webtech/102704-1.shtml).

> [!NOTE]
> Au lieu d’utiliser une instruction `CASE` ici, nous pourrions également utiliser `ISNULL(ContactTitle, '')`. [`ISNULL(checkExpression, replacementValue)`](https://msdn.microsoft.com/library/ms184325.aspx) retourne *checkExpression* si la valeur n’est pas NULL ; sinon, elle retourne *replacementValue*. Bien que `ISNULL` ou `CASE` fonctionneront dans cette instance, il existe des scénarios plus complexes dans lesquels la flexibilité de l’instruction `CASE` ne peut pas être mise en correspondance par `ISNULL`.

Après avoir ajouté cette colonne calculée, votre écran doit ressembler à la capture d’écran de la figure 1.

[![ajouter une colonne calculée nommée FullContactName à la table Suppliers](working-with-computed-columns-cs/_static/image2.png)](working-with-computed-columns-cs/_static/image1.png)

**Figure 1**: ajouter une colonne calculée nommée `FullContactName` à la table `Suppliers` ([cliquez pour afficher l’image en taille réelle](working-with-computed-columns-cs/_static/image3.png))

Après avoir nommé la colonne calculée et entré son expression, enregistrez les modifications apportées à la table en cliquant sur l’icône Enregistrer dans la barre d’outils, en appuyant sur CTRL + S, ou en accédant au menu fichier et en choisissant enregistrer `Suppliers`.

L’enregistrement de la table doit actualiser la Explorateur de serveurs, y compris la colonne qui vient d’être ajoutée dans la liste des colonnes de `Suppliers` table s. En outre, l’expression entrée dans la zone de texte (formule) s’ajuste automatiquement à une expression équivalente qui supprime les espaces superflus, entoure les noms de colonnes de crochets (`[]`) et comprend des parenthèses pour afficher plus explicitement l’ordre des opérations :

[!code-sql[Main](working-with-computed-columns-cs/samples/sample2.sql)]

Pour plus d’informations sur les colonnes calculées dans Microsoft SQL Server, reportez-vous à la [documentation technique](https://msdn.microsoft.com/library/ms191250.aspx). Découvrez également [Comment : spécifier des colonnes calculées](https://msdn.microsoft.com/library/ms188300.aspx) pour une procédure pas à pas de création de colonnes calculées.

> [!NOTE]
> Par défaut, les colonnes calculées ne sont pas physiquement stockées dans la table, mais elles sont recalculées à chaque fois qu’elles sont référencées dans une requête. En cochant la case est persistante, vous pouvez demander à SQL Server de stocker physiquement la colonne calculée dans la table. Cela permet de créer un index sur la colonne calculée, ce qui peut améliorer les performances des requêtes qui utilisent la valeur de colonne calculée dans leurs clauses de `WHERE`. Pour plus d’informations, consultez [création d’index sur des colonnes calculées](https://msdn.microsoft.com/library/ms189292.aspx) .

## <a name="step-2-viewing-the-computed-column-s-values"></a>Étape 2 : affichage des valeurs des colonnes calculées

Avant de commencer à travailler sur la couche d’accès aux données, attendez quelques minutes pour voir les valeurs de `FullContactName`. Dans le Explorateur de serveurs, cliquez avec le bouton droit sur le nom de la table `Suppliers` et choisissez nouvelle requête dans le menu contextuel. Une fenêtre de requête s’affiche pour vous inviter à choisir les tables à inclure dans la requête. Ajoutez la table `Suppliers`, puis cliquez sur Fermer. Ensuite, vérifiez les colonnes `CompanyName`, `ContactName`, `ContactTitle`et `FullContactName` de la table Suppliers. Enfin, cliquez sur l’icône de point d’exclamation rouge dans la barre d’outils pour exécuter la requête et afficher les résultats.

Comme le montre la figure 2, les résultats incluent `FullContactName`, qui répertorie les colonnes `CompanyName`, `ContactName`et `ContactTitle` à l’aide du format ldquo ;`ContactName` (`ContactTitle`, `CompanyName`).

[![FullContactName utilise le format ContactName (ContactTitle, CompanyName)](working-with-computed-columns-cs/_static/image5.png)](working-with-computed-columns-cs/_static/image4.png)

**Figure 2**: le `FullContactName` utilise le format `ContactName` (`ContactTitle`, `CompanyName`) ([cliquez pour afficher l’image en taille réelle](working-with-computed-columns-cs/_static/image6.png))

## <a name="step-3-adding-thesupplierstableadapterto-the-data-access-layer"></a>Étape 3 : ajout de la`SuppliersTableAdapter`à la couche d’accès aux données

Pour pouvoir utiliser les informations sur les fournisseurs dans notre application, nous devons d’abord créer un TableAdapter et un DataTable dans notre couche DAL. Dans l’idéal, cette opération est accomplie à l’aide des mêmes étapes simples que celles étudiées dans les didacticiels précédents. Toutefois, l’utilisation de colonnes calculées présente quelques plis qui méritent une discussion.

Si vous utilisez un TableAdapter qui utilise des instructions SQL ad hoc, vous pouvez simplement inclure la colonne calculée dans la requête principale du TableAdapter à l’aide de l’Assistant Configuration de TableAdapter. Toutefois, il génère automatiquement les instructions `INSERT` et `UPDATE` qui incluent la colonne calculée. Si vous essayez d’exécuter l’une de ces méthodes, une `SqlException` avec le message la colonne *ColumnName* ne peut pas être modifiée, car il s’agit d’une colonne calculée ou le résultat d’un opérateur Union est levé. Tandis que l’instruction `INSERT` et `UPDATE` peuvent être ajustées manuellement par le biais des propriétés `InsertCommand` et `UpdateCommand` du TableAdapter, ces personnalisations sont perdues à chaque réexécution de l’Assistant Configuration de TableAdapter.

En raison de la fragilité des TableAdapters qui utilisent des instructions SQL ad hoc, nous vous recommandons d’utiliser des procédures stockées lors de l’utilisation de colonnes calculées. Si vous utilisez des procédures stockées existantes, il vous suffit de configurer le TableAdapter comme indiqué dans le didacticiel [utilisation de procédures stockées existantes pour les TableAdapters de DataSet typé](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) . Toutefois, si l’Assistant TableAdapter crée les procédures stockées pour vous, il est important d’omettre initialement les colonnes calculées de la requête principale. Si vous incluez une colonne calculée dans la requête principale, l’Assistant Configuration de TableAdapter vous informe, à l’achèvement, qu’il ne peut pas créer les procédures stockées correspondantes. En résumé, nous devons tout d’abord configurer le TableAdapter à l’aide d’une requête principale sans colonne calculée, puis mettre à jour manuellement la procédure stockée correspondante et les `SelectCommand` TableAdapter pour inclure la colonne calculée. Cette approche est similaire à celle utilisée dans le didacticiel [mise à jour du TableAdapter pour utiliser](updating-the-tableadapter-to-use-joins-cs.md)`JOIN`*s* .

Pour ce didacticiel, nous allons ajouter un nouveau TableAdapter et le faire créer automatiquement les procédures stockées pour nous. Par conséquent, nous devons initialement omettre la `FullContactName` colonne calculée de la requête principale.

Commencez par ouvrir le jeu de données `NorthwindWithSprocs` dans le dossier `~/App_Code/DAL`. Cliquez avec le bouton droit dans le concepteur et, dans le menu contextuel, choisissez d’ajouter un nouveau TableAdapter. L’Assistant Configuration de TableAdapter est lancé. Spécifiez la base de données à partir de laquelle interroger les données (`NORTHWNDConnectionString` `Web.config`), puis cliquez sur suivant. Étant donné que nous n’avons pas encore créé de procédures stockées pour interroger ou modifier la table `Suppliers`, sélectionnez l’option créer de nouvelles procédures stockées afin que l’Assistant les crée pour nous et clique sur suivant.

[![choisissez l’option créer de nouvelles procédures stockées](working-with-computed-columns-cs/_static/image8.png)](working-with-computed-columns-cs/_static/image7.png)

**Figure 3**: choisissez l’option créer de nouvelles procédures stockées ([cliquez pour afficher l’image en taille réelle](working-with-computed-columns-cs/_static/image9.png))

L’étape suivante nous invite à entrer la requête principale. Entrez la requête suivante, qui renvoie les colonnes `SupplierID`, `CompanyName`, `ContactName`et `ContactTitle` pour chaque fournisseur. Notez que cette requête omet intentionnellement la colonne calculée (`FullContactName`); Nous allons mettre à jour la procédure stockée correspondante pour inclure cette colonne à l’étape 4.

[!code-sql[Main](working-with-computed-columns-cs/samples/sample3.sql)]

Après avoir entré la requête principale et cliqué sur suivant, l’Assistant nous permet de nommer les quatre procédures stockées qu’il va générer. Nommez ces procédures stockées `Suppliers_Select`, `Suppliers_Insert`, `Suppliers_Update`et `Suppliers_Delete`, comme illustré dans la figure 4.

[![personnaliser les noms des procédures stockées générées automatiquement](working-with-computed-columns-cs/_static/image11.png)](working-with-computed-columns-cs/_static/image10.png)

**Figure 4**: personnaliser les noms des procédures stockées générées automatiquement ([cliquez pour afficher l’image en taille réelle](working-with-computed-columns-cs/_static/image12.png))

L’étape suivante de l’Assistant nous permet de nommer les méthodes TableAdapter s et de spécifier les modèles utilisés pour accéder aux données et les mettre à jour. Laissez les trois cases à cocher activées, mais renommez la méthode `GetData` pour `GetSuppliers`. Cliquez sur Terminer pour terminer l'Assistant.

[![renommez la méthode GetData en GetSuppliers](working-with-computed-columns-cs/_static/image14.png)](working-with-computed-columns-cs/_static/image13.png)

**Figure 5**: renommer la méthode `GetData` en `GetSuppliers` ([cliquez pour afficher l’image en taille réelle](working-with-computed-columns-cs/_static/image15.png))

Quand vous cliquez sur terminer, l’Assistant crée les quatre procédures stockées et ajoute le TableAdapter et le DataTable correspondant au DataSet typé.

## <a name="step-4-including-the-computed-column-in-the-tableadapter-s-main-query"></a>Étape 4 : inclusion de la colonne calculée dans la requête principale du TableAdapter

Nous devons maintenant mettre à jour le TableAdapter et le DataTable créés à l’étape 3 pour inclure le `FullContactName` colonne calculée. Cette opération s'effectue en deux étapes :

1. Mise à jour de la procédure stockée `Suppliers_Select` pour retourner le `FullContactName` colonne calculée, et
2. Mise à jour du DataTable pour inclure une colonne `FullContactName` correspondante.

Commencez par naviguer jusqu’au Explorateur de serveurs et en explorant le dossier procédures stockées. Ouvrez la procédure stockée `Suppliers_Select` et mettez à jour la requête `SELECT` pour inclure la `FullContactName` colonne calculée :

[!code-sql[Main](working-with-computed-columns-cs/samples/sample4.sql)]

Enregistrez les modifications apportées à la procédure stockée en cliquant sur l’icône Enregistrer dans la barre d’outils, en appuyant sur CTRL + S ou en choisissant l’option Enregistrer `Suppliers_Select` dans le menu fichier.

Ensuite, revenez au concepteur de DataSet, cliquez avec le bouton droit sur le `SuppliersTableAdapter`, puis choisissez configurer dans le menu contextuel. Notez que la colonne `Suppliers_Select` comprend maintenant la colonne `FullContactName` dans sa collection de colonnes de données.

[![exécuter l’Assistant Configuration de TableAdapter pour mettre à jour les colonnes du DataTable s](working-with-computed-columns-cs/_static/image17.png)](working-with-computed-columns-cs/_static/image16.png)

**Figure 6**: exécuter l’Assistant Configuration de TableAdapter pour mettre à jour les colonnes de DataTable s ([cliquez pour afficher l’image en taille réelle](working-with-computed-columns-cs/_static/image18.png))

Cliquez sur Terminer pour terminer l'Assistant. Cette opération ajoute automatiquement une colonne correspondante au `SuppliersDataTable`. L’Assistant TableAdapter est suffisamment intelligent pour détecter que la colonne `FullContactName` est une colonne calculée et donc en lecture seule. Par conséquent, il définit la propriété de la colonne s `ReadOnly` sur `true`. Pour vérifier cela, sélectionnez la colonne dans la `SuppliersDataTable` puis accédez à la Fenêtre Propriétés (voir figure 7). Notez que les propriétés `FullContactName` Column s `DataType` et `MaxLength` sont également définies en conséquence.

[![la colonne FullContactName est marquée en lecture seule](working-with-computed-columns-cs/_static/image20.png)](working-with-computed-columns-cs/_static/image19.png)

**Figure 7**: la colonne `FullContactName` est marquée en lecture seule ([cliquez pour afficher l’image en taille réelle](working-with-computed-columns-cs/_static/image21.png))

## <a name="step-5-adding-agetsupplierbysupplieridmethod-to-the-tableadapter"></a>Étape 5 : ajout d’une méthode`GetSupplierBySupplierID`au TableAdapter

Pour ce didacticiel, nous allons créer une page ASP.NET qui affiche les fournisseurs dans une grille modifiable. Dans les didacticiels précédents, nous avons mis à jour un enregistrement unique à partir de la couche de logique métier en extrayant cet enregistrement spécifique de la couche DAL en tant que DataTable fortement typé, en mettant à jour ses propriétés, puis en renvoyant le DataTable mis à jour à la couche DAL pour propager les modifications à base de données. Pour effectuer cette première étape, récupérez l’enregistrement en cours de mise à jour à partir de la couche DAL. nous devons d’abord ajouter une méthode de `GetSupplierBySupplierID(supplierID)` à la couche DAL.

Cliquez avec le bouton droit sur la `SuppliersTableAdapter` dans la conception du jeu de données et choisissez l’option Ajouter une requête dans le menu contextuel. Comme nous l’avons fait à l’étape 3, laissez l’Assistant générer une nouvelle procédure stockée pour nous en sélectionnant l’option créer une nouvelle procédure stockée (reportez-vous à la figure 3 pour obtenir une capture d’écran de cette étape de l’Assistant). Étant donné que cette méthode retourne un enregistrement avec plusieurs colonnes, indiquez que nous souhaitons utiliser une requête SQL qui est une SELECT qui retourne des lignes, puis cliquez sur suivant.

[![choisissez l’option Sélectionner les lignes à retourner](working-with-computed-columns-cs/_static/image23.png)](working-with-computed-columns-cs/_static/image22.png)

**Figure 8**: choisissez l’option Sélectionner les lignes à retourner ([cliquez pour afficher l’image en taille réelle](working-with-computed-columns-cs/_static/image24.png))

L’étape suivante nous invite à entrer la requête à utiliser pour cette méthode. Entrez la commande suivante, qui retourne les mêmes champs de données que la requête principale, mais pour un fournisseur particulier.

[!code-sql[Main](working-with-computed-columns-cs/samples/sample5.sql)]

L’écran suivant nous demande de nommer la procédure stockée qui sera générée automatiquement. Nommez cette procédure stockée `Suppliers_SelectBySupplierID`, puis cliquez sur suivant.

[![nom de la procédure stockée Suppliers_SelectBySupplierID](working-with-computed-columns-cs/_static/image26.png)](working-with-computed-columns-cs/_static/image25.png)

**Figure 9**: nommer la procédure stockée `Suppliers_SelectBySupplierID` ([cliquez pour afficher l’image en taille réelle](working-with-computed-columns-cs/_static/image27.png))

Enfin, l’Assistant nous invite à entrer les modèles d’accès aux données et les noms de méthode à utiliser pour le TableAdapter. Laissez les cases à cocher activées, mais renommez les méthodes `FillBy` et `GetDataBy` en `FillBySupplierID` et `GetSupplierBySupplierID`, respectivement.

[![nommez les méthodes TableAdapter FillBySupplierID et GetSupplierBySupplierID](working-with-computed-columns-cs/_static/image29.png)](working-with-computed-columns-cs/_static/image28.png)

**Figure 10**: nommer les méthodes TableAdapter `FillBySupplierID` et `GetSupplierBySupplierID` ([cliquez pour afficher l’image en taille réelle](working-with-computed-columns-cs/_static/image30.png))

Cliquez sur Terminer pour terminer l'Assistant.

## <a name="step-6-creating-the-business-logic-layer"></a>Étape 6 : création de la couche de logique métier

Avant de créer une page ASP.NET qui utilise la colonne calculée créée à l’étape 1, nous devons d’abord ajouter les méthodes correspondantes dans la couche BLL. Notre page ASP.NET, que nous allons créer à l’étape 7, permettra aux utilisateurs d’afficher et de modifier des fournisseurs. Par conséquent, nous avons besoin de notre BLL pour fournir, au minimum, une méthode pour obtenir tous les fournisseurs et un autre pour mettre à jour un fournisseur particulier.

Créez un nouveau fichier de classe nommé `SuppliersBLLWithSprocs` dans le dossier `~/App_Code/BLL` et ajoutez le code suivant :

[!code-csharp[Main](working-with-computed-columns-cs/samples/sample6.cs)]

À l’instar des autres classes BLL, `SuppliersBLLWithSprocs` a une propriété `protected` `Adapter` qui retourne une instance de la classe `SuppliersTableAdapter`, ainsi que deux méthodes `public` : `GetSuppliers` et `UpdateSupplier`. La méthode `GetSuppliers` appelle et retourne le `SuppliersDataTable` retourné par la méthode `GetSupplier` correspondante dans la couche d’accès aux données. La méthode `UpdateSupplier` récupère des informations sur le fournisseur particulier qui est mis à jour via un appel à la méthode de `GetSupplierBySupplierID(supplierID)` DAL s. Il met ensuite à jour les propriétés `CategoryName`, `ContactName`et `ContactTitle` et valide ces modifications dans la base de données en appelant la méthode `Update` de la couche d’accès aux données, en passant l’objet `SuppliersRow` modifié.

> [!NOTE]
> À l’exception de `SupplierID` et `CompanyName`, toutes les colonnes de la table Suppliers autorisent les valeurs `NULL`. Par conséquent, si les paramètres `contactName` ou `contactTitle` transmis sont `null` il est nécessaire de définir les propriétés `ContactName` et `ContactTitle` correspondantes sur une valeur de base de données `NULL` à l’aide des méthodes `SetContactNameNull` et `SetContactTitleNull`, respectivement.

## <a name="step-7-working-with-the-computed-column-from-the-presentation-layer"></a>Étape 7 : utilisation de la colonne calculée de la couche de présentation

Avec la colonne calculée ajoutée à la table `Suppliers` et la couche DAL et la couche BLL mises à jour en conséquence, nous sommes prêts à créer une page ASP.NET qui fonctionne avec la colonne calculée `FullContactName`. Commencez par ouvrir la page `ComputedColumns.aspx` dans le dossier `AdvancedDAL` et faites glisser un contrôle GridView de la boîte à outils vers le concepteur. Définissez la propriété GridView s `ID` sur `Suppliers` et, à partir de sa balise active, liez-la à un nouvel ObjectDataSource nommé `SuppliersDataSource`. Configurez le ObjectDataSource pour utiliser la classe `SuppliersBLLWithSprocs` que nous avons rajoutée à l’étape 6, puis cliquez sur suivant.

[![configurer ObjectDataSource pour utiliser la classe SuppliersBLLWithSprocs](working-with-computed-columns-cs/_static/image32.png)](working-with-computed-columns-cs/_static/image31.png)

**Figure 11**: configurer ObjectDataSource pour utiliser la classe `SuppliersBLLWithSprocs` ([cliquez pour afficher l’image en taille réelle](working-with-computed-columns-cs/_static/image33.png))

Il n’existe que deux méthodes définies dans la classe `SuppliersBLLWithSprocs` : `GetSuppliers` et `UpdateSupplier`. Assurez-vous que ces deux méthodes sont spécifiées respectivement dans les onglets sélectionner et mettre à jour, puis cliquez sur Terminer pour terminer la configuration de l’ObjectDataSource.

À la fin de l’Assistant Configuration de source de données, Visual Studio ajoute un BoundField pour chacun des champs de données retournés. Supprimez le `SupplierID` BoundField et modifiez les propriétés de `HeaderText` des `CompanyName`, `ContactName`, `ContactTitle`et `FullContactName` BoundFields en société, nom de contact, titre et nom de contact complet, respectivement. À partir de la balise active, activez la case à cocher Activer la modification pour activer les fonctionnalités d’édition intégrées de GridView.

Outre l’ajout de BoundFields à GridView, l’exécution de l’Assistant source de données permet également à Visual Studio de définir la propriété ObjectDataSource s `OldValuesParameterFormatString` sur la valeur d’origine\_{0}. Rétablissez la valeur par défaut de ce paramètre, {0}.

Une fois ces modifications apportées aux contrôles GridView et ObjectDataSource, leur balisage déclaratif doit ressembler à ce qui suit :

[!code-aspx[Main](working-with-computed-columns-cs/samples/sample7.aspx)]

Ensuite, accédez à cette page via un navigateur. Comme le montre la figure 12, chaque fournisseur est listé dans une grille qui comprend la colonne `FullContactName`, dont la valeur est simplement la concaténation des trois autres colonnes mises en forme en tant que `ContactName` (`ContactTitle`, `CompanyName`).

[![chaque fournisseur est listé dans la grille](working-with-computed-columns-cs/_static/image35.png)](working-with-computed-columns-cs/_static/image34.png)

**Figure 12**: chaque fournisseur est listé dans la grille ([cliquez pour afficher l’image en taille réelle](working-with-computed-columns-cs/_static/image36.png))

Le fait de cliquer sur le bouton modifier pour un fournisseur particulier entraîne une publication et affiche cette ligne dans son interface de modification (voir figure 13). Les trois premières colonnes s’affichent dans leur interface de modification par défaut, un contrôle TextBox dont la propriété `Text` est définie sur la valeur du champ de données. Toutefois, la colonne `FullContactName` reste sous forme de texte. Lorsque les BoundFields ont été ajoutés au contrôle GridView à la fin de l’Assistant Configuration de source de données, la propriété `FullContactName` BoundField s `ReadOnly` a été définie sur `true`, car la propriété `SuppliersDataTable` de la colonne `FullContactName` correspondante de la `ReadOnly` est définie sur `true`. Comme indiqué à l’étape 4, la propriété `FullContactName` s `ReadOnly` a été définie sur `true` car le TableAdapter a détecté que la colonne était une colonne calculée.

[![la colonne FullContactName n’est pas modifiable](working-with-computed-columns-cs/_static/image38.png)](working-with-computed-columns-cs/_static/image37.png)

**Figure 13**: la colonne `FullContactName` n’est pas modifiable ([cliquez pour afficher l’image en taille réelle](working-with-computed-columns-cs/_static/image39.png))

Continuez et mettez à jour la valeur d’une ou plusieurs des colonnes modifiables, puis cliquez sur mettre à jour. Notez comment la valeur de `FullContactName` s est automatiquement mise à jour pour refléter la modification.

> [!NOTE]
> Le GridView utilise actuellement BoundFields pour les champs modifiables, ce qui se traduit par l’interface de modification par défaut. Étant donné que le champ `CompanyName` est obligatoire, il doit être converti en TemplateField qui comprend un RequiredFieldValidator. Je laisse cela en tant qu’exercice pour le lecteur intéressé. Pour obtenir des instructions pas à pas sur la conversion d’un BoundField en TemplateField et l’ajout de contrôles de validation, consultez le didacticiel [Ajout de contrôles de validation au didacticiel sur les interfaces de modification et d’insertion](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) .

## <a name="summary"></a>Récapitulatif

Lors de la définition du schéma d’une table, Microsoft SQL Server permet l’inclusion de colonnes calculées. Il s’agit de colonnes dont les valeurs sont calculées à partir d’une expression qui fait généralement référence aux valeurs d’autres colonnes dans le même enregistrement. Étant donné que les valeurs des colonnes calculées sont basées sur une expression, elles sont en lecture seule et ne peuvent pas être assignées à une valeur dans une instruction `INSERT` ou `UPDATE`. Cela présente des difficultés lors de l’utilisation d’une colonne calculée dans la requête principale d’un TableAdapter qui tente de générer automatiquement les instructions `INSERT`, `UPDATE`et `DELETE` correspondantes.

Dans ce didacticiel, nous avons abordé les techniques permettant de contourner les problèmes posés par les colonnes calculées. En particulier, nous avons utilisé des procédures stockées dans notre TableAdapter pour surmonter la fragilité inhérente aux TableAdapters qui utilisent des instructions SQL ad hoc. Lorsque l’Assistant TableAdapter crée de nouvelles procédures stockées, il est important que la requête principale omette au départ toutes les colonnes calculées, car leur présence empêche la génération des procédures stockées de modification de données. Une fois que le TableAdapter a été initialement configuré, sa procédure stockée `SelectCommand` peut être redéfinie pour inclure toutes les colonnes calculées.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel étaient Hilton Geisenow et Teresa Murphy. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](adding-additional-datatable-columns-cs.md)
> [Suivant](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs.md)
