---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
title: Utilisation de procédures stockées existantes pour les TableAdapters du DataSet typé (VB) | Microsoft Docs
author: rick-anderson
description: Dans le didacticiel précédent, nous avons appris à utiliser l’Assistant TableAdapter pour générer de nouvelles procédures stockées. Dans ce didacticiel, nous allons découvrir comment le même TableAdapter...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: 2da25f6a-757e-4e7b-a812-1575288d8f7a
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
msc.type: authoredcontent
ms.openlocfilehash: e35c3d6a98516a07f6119e6cb9dbeb99bc28fe33
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78531800"
---
# <a name="using-existing-stored-procedures-for-the-typed-datasets-tableadapters-vb"></a>Utilisation de procédures stockées existantes pour les TableAdapters de dataset typé (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_68_VB.zip) ou [Télécharger le PDF](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/datatutorial68vb1.pdf)

> Dans le didacticiel précédent, nous avons appris à utiliser l’Assistant TableAdapter pour générer de nouvelles procédures stockées. Dans ce didacticiel, nous allons découvrir comment le même Assistant TableAdapter peut fonctionner avec les procédures stockées existantes. Nous apprenons également à ajouter manuellement de nouvelles procédures stockées à notre base de données.

## <a name="introduction"></a>Introduction

Dans le [didacticiel précédent](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) , nous avons vu comment les TableAdapters des datasets typés pouvaient être configurés pour utiliser des procédures stockées pour accéder aux données plutôt qu’à des instructions SQL ad hoc. Nous avons notamment examiné comment faire en sorte que l’Assistant TableAdapter crée automatiquement ces procédures stockées. Lors du Portage d’une application héritée vers ASP.NET 2,0 ou lors de la création d’un site Web ASP.NET 2,0 autour d’un modèle de données existant, il est probable que la base de données contienne déjà les procédures stockées dont nous avons besoin. Vous pouvez également créer vos procédures stockées manuellement ou à l’aide d’un autre outil que l’Assistant TableAdapter qui génère automatiquement vos procédures stockées.

Dans ce didacticiel, nous allons examiner comment configurer le TableAdapter pour utiliser des procédures stockées existantes. Étant donné que la base de données Northwind ne possède qu’un petit ensemble de procédures stockées intégrées, nous examinerons également les étapes nécessaires pour ajouter manuellement de nouvelles procédures stockées à la base de données par le biais de l’environnement Visual Studio. Commençons !

> [!NOTE]
> Dans le didacticiel sur les [modifications de base de données encapsulées dans une transaction](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) , nous avons ajouté des méthodes au TableAdapter pour prendre en charge les transactions (`BeginTransaction`, `CommitTransaction`, etc.). Les transactions peuvent également être entièrement gérées dans une procédure stockée, ce qui ne nécessite aucune modification du code de la couche d’accès aux données. Dans ce didacticiel, nous allons explorer les commandes T-SQL utilisées pour exécuter les instructions des procédures stockées dans l’étendue d’une transaction.

## <a name="step-1-adding-stored-procedures-to-the-northwind-database"></a>Étape 1 : ajout de procédures stockées à la base de données Northwind

Visual Studio facilite l’ajout de nouvelles procédures stockées à une base de données. Ajoutez une nouvelle procédure stockée à la base de données Northwind qui retourne toutes les colonnes de la table `Products` pour celles qui ont une valeur de `CategoryID` particulière. Dans la fenêtre Explorateur de serveurs, développez la base de données Northwind afin d’afficher ses dossiers (schémas de base de données, tables, vues, etc.). Comme nous l’avons vu dans le didacticiel précédent, le dossier procédures stockées contient les procédures stockées existantes de la base de données. Pour ajouter une nouvelle procédure stockée, il vous suffit de cliquer avec le bouton droit sur le dossier procédures stockées et de choisir l’option Ajouter une nouvelle procédure stockée dans le menu contextuel.

[![cliquez avec le bouton droit sur le dossier procédures stockées et ajoutez une nouvelle procédure stockée](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image2.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image1.png)

**Figure 1**: cliquez avec le bouton droit sur le dossier procédures stockées et ajoutez une nouvelle procédure stockée ([cliquez pour afficher l’image en taille réelle](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image3.png))

Comme le montre la figure 1, la sélection de l’option Ajouter une nouvelle procédure stockée ouvre une fenêtre de script dans Visual Studio avec le contour du script SQL nécessaire à la création de la procédure stockée. Nous sommes notre tâche de étoffer ce script et de l’exécuter, à partir de laquelle la procédure stockée sera ajoutée à la base de données.

Entrez le script suivant :

[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample1.sql)]

Ce script, lorsqu’il est exécuté, ajoute une nouvelle procédure stockée à la base de données Northwind nommée `Products_SelectByCategoryID`. Cette procédure stockée accepte un paramètre d’entrée unique (`@CategoryID`, de type `int`) et retourne tous les champs pour ces produits avec une valeur de `CategoryID` correspondante.

Pour exécuter cette `CREATE PROCEDURE` script et ajouter la procédure stockée à la base de données, cliquez sur l’icône Enregistrer dans la barre d’outils ou appuyez sur CTRL + S. Après cela, le dossier procédures stockées est actualisé, en présentant la procédure stockée nouvellement créée. En outre, le script dans la fenêtre change la subtilité de `CREATE PROCEDURE dbo.Products_SelectProductByCategoryID` à `ALTER PROCEDURE` `dbo.Products_SelectProductByCategoryID`. `CREATE PROCEDURE` ajoute une nouvelle procédure stockée à la base de données, tandis que `ALTER PROCEDURE` met à jour une procédure existante. Étant donné que le début du script est passé à `ALTER PROCEDURE`, la modification des paramètres d’entrée des procédures stockées ou des instructions SQL et le fait de cliquer sur l’icône Enregistrer met à jour la procédure stockée avec ces modifications.

La figure 2 montre Visual Studio après l’enregistrement de la procédure stockée `Products_SelectByCategoryID`.

[![la Products_SelectByCategoryID de la procédure stockée a été ajoutée à la base de données](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image5.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image4.png)

**Figure 2**: la procédure stockée `Products_SelectByCategoryID` a été ajoutée à la base de données ([cliquez pour afficher l’image en taille réelle](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image6.png))

## <a name="step-2-configuring-the-tableadapter-to-use-an-existing-stored-procedure"></a>Étape 2 : configuration du TableAdapter pour utiliser une procédure stockée existante

Maintenant que la procédure stockée `Products_SelectByCategoryID` a été ajoutée à la base de données, nous pouvons configurer notre couche d’accès aux données de façon à ce qu’elle utilise cette procédure stockée lorsque l’une de ses méthodes est appelée. En particulier, nous allons ajouter une méthode de `GetProductsByCategoryID(<_i22_>categoryID)<!--_i22_-->` à la `ProductsTableAdapter` dans le DataSet typé `NorthwindWithSprocs` qui appelle la procédure stockée `Products_SelectByCategoryID` que nous venons de créer.

Commencez par ouvrir le jeu de données `NorthwindWithSprocs`. Cliquez avec le bouton droit sur le `ProductsTableAdapter`, puis choisissez Ajouter une requête pour lancer l’Assistant Configuration de requêtes TableAdapter. Dans le [didacticiel précédent](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) , nous avons choisi que le TableAdapter crée une nouvelle procédure stockée pour nous. Pour ce didacticiel, toutefois, nous voulons connecter la nouvelle méthode TableAdapter à la procédure stockée `Products_SelectByCategoryID` existante. Par conséquent, choisissez l’option utiliser une procédure stockée existante dans la première étape de l’Assistant, puis cliquez sur suivant.

[![choisissez l’option utiliser une procédure stockée existante](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image8.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image7.png)

**Figure 3**: choisir l’option utiliser une procédure stockée existante ([cliquez pour afficher l’image en taille réelle](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image9.png))

L’écran suivant fournit une liste déroulante remplie avec les procédures stockées de la base de données. La sélection d’une procédure stockée répertorie ses paramètres d’entrée à gauche et les champs de données retournés (le cas échéant) à droite. Sélectionnez la procédure stockée `Products_SelectByCategoryID` dans la liste et cliquez sur suivant.

[![choisir la procédure stockée Products_SelectByCategoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image11.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image10.png)

**Figure 4**: sélectionner la procédure stockée `Products_SelectByCategoryID` ([cliquez pour afficher l’image en taille réelle](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image12.png))

L’écran suivant nous demande quel type de données est retourné par la procédure stockée, et notre réponse ici détermine le type retourné par la méthode du TableAdapter. Par exemple, si nous indiquons que les données tabulaires sont retournées, la méthode retourne une instance `ProductsDataTable` remplie avec les enregistrements retournés par la procédure stockée. En revanche, si nous indiquons que cette procédure stockée retourne une valeur unique, le TableAdapter retourne une `Object` à laquelle la valeur de la première colonne du premier enregistrement retourné par la procédure stockée est affectée.

Étant donné que la procédure stockée `Products_SelectByCategoryID` retourne tous les produits appartenant à une catégorie particulière, choisissez la première réponse-données tabulaires, puis cliquez sur suivant.

[![indiquer que la procédure stockée retourne des données tabulaires](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image14.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image13.png)

**Figure 5**: indiquer que la procédure stockée retourne des données tabulaires ([cliquez pour afficher l’image en taille réelle](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image15.png))

Il ne reste plus qu’à indiquer les modèles de méthode à utiliser, suivis des noms de ces méthodes. Laissez les options remplir un DataTable et retourner un DataTable activée, mais renommez les méthodes en `FillByCategoryID` et `GetProductsByCategoryID`. Cliquez ensuite sur suivant pour consulter un résumé des tâches que l’Assistant va effectuer. Si tout semble correct, cliquez sur Terminer.

[![nommez les méthodes FillByCategoryID et GetProductsByCategoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image17.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image16.png)

**Figure 6**: nommer les méthodes `FillByCategoryID` et `GetProductsByCategoryID` ([cliquez pour afficher l’image en taille réelle](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image18.png))

> [!NOTE]
> Les méthodes TableAdapter que nous venons de créer, `FillByCategoryID` et `GetProductsByCategoryID`, attendent un paramètre d’entrée de type `Integer`. Cette valeur de paramètre d’entrée est transmise à la procédure stockée via son paramètre `@CategoryID`. Si vous modifiez les paramètres de la procédure stockée `Products_SelectByCategory`, vous devrez également mettre à jour les paramètres de ces méthodes TableAdapter. Comme indiqué dans le [didacticiel précédent](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md), cette opération peut être effectuée de deux manières : en ajoutant ou en supprimant manuellement des paramètres dans la collection Parameters ou en réexécutant l’Assistant TableAdapter.

## <a name="step-3-adding-agetproductsbycategoryidcategoryidmethod-to-the-bll"></a>Étape 3 : ajout d’une méthode`GetProductsByCategoryID(categoryID)`à la couche BLL

Une fois la méthode `GetProductsByCategoryID` DAL terminée, l’étape suivante consiste à fournir l’accès à cette méthode dans la couche de logique métier. Ouvrez le fichier de classe `ProductsBLLWithSprocs` et ajoutez la méthode suivante :

[!code-vb[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample2.vb)]

Cette méthode BLL retourne simplement le `ProductsDataTable` retourné par la méthode `GetProductsByCategoryID` `ProductsTableAdapter`. L’attribut `DataObjectMethodAttribute` fournit les métadonnées utilisées par l’Assistant Configuration de la source de données ObjectDataSource s. En particulier, cette méthode apparaît dans la liste déroulante Sélectionner les onglets.

## <a name="step-4-displaying-products-by-category"></a>Étape 4 : affichage des produits par catégorie

Pour tester la procédure stockée `Products_SelectByCategoryID` nouvellement ajoutée et les méthodes DAL et BLL correspondantes, créez une page ASP.NET contenant un DropDownList et un GridView. Le DropDownList répertorie toutes les catégories dans la base de données, tandis que le contrôle GridView affiche les produits appartenant à la catégorie sélectionnée.

> [!NOTE]
> Nous avons créé des interfaces maître/détail à l’aide de DropDownList dans les didacticiels précédents. Pour plus d’informations sur l’implémentation d’un rapport maître/détail, reportez-vous au didacticiel sur le [filtrage maître/détail avec un contrôle DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) .

Ouvrez la page `ExistingSprocs.aspx` dans le dossier `AdvancedDAL` et faites glisser un contrôle DropDownList de la boîte à outils vers le concepteur. Affectez à la propriété DropDownList s `ID` la valeur `Categories` et à sa propriété `AutoPostBack` la valeur `True`. Ensuite, à partir de sa balise active, liez le DropDownList à un nouvel ObjectDataSource nommé `CategoriesDataSource`. Configurez le ObjectDataSource afin qu’il récupère ses données à partir de la méthode de la classe `CategoriesBLL` s `GetCategories`. Définissez les listes déroulantes dans les onglets mettre à jour, insérer et supprimer sur (aucun).

[![récupérer des données de la méthode GetCategories de la classe CategoriesBLL s](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image20.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image19.png)

**Figure 7**: récupérer des données à partir de la méthode de la classe `CategoriesBLL` s `GetCategories` ([cliquez pour afficher l’image en taille réelle](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image21.png))

[![définir les listes déroulantes dans les onglets mettre à jour, insérer et supprimer sur (aucun)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image23.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image22.png)

**Figure 8**: définir les listes déroulantes des onglets mettre à jour, insérer et supprimer sur (aucun) ([cliquez pour afficher l’image en taille réelle](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image24.png))

Une fois l’exécution de l’Assistant ObjectDataSource terminée, configurez le DropDownList pour afficher le champ de données `CategoryName` et utiliser le champ `CategoryID` comme `Value` pour chaque `ListItem`.

À ce stade, les balises déclaratives DropDownList et ObjectDataSource s doivent ressembler à ce qui suit :

[!code-aspx[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample3.aspx)]

Ensuite, faites glisser un contrôle GridView sur le concepteur, en le plaçant sous le DropDownList. Définissez le `ID` GridView s sur `ProductsByCategory` et, à partir de sa balise active, liez-le à un nouvel ObjectDataSource nommé `ProductsByCategoryDataSource`. Configurez le `ProductsByCategoryDataSource` ObjectDataSource pour utiliser la classe `ProductsBLLWithSprocs`, en le faisant récupérer ses données à l’aide de la méthode `GetProductsByCategoryID(categoryID)`. Étant donné que ce GridView sera utilisé uniquement pour afficher des données, définissez les listes déroulantes dans les onglets mettre à jour, insérer et supprimer sur (aucun), puis cliquez sur suivant.

[![configurer ObjectDataSource pour utiliser la classe ProductsBLLWithSprocs](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image26.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image25.png)

**Figure 9**: configurer ObjectDataSource pour utiliser la classe `ProductsBLLWithSprocs` ([cliquez pour afficher l’image en taille réelle](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image27.png))

[![récupérer des données à partir de la méthode GetProductsByCategoryID (categoryID)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image29.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image28.png)

**Figure 10**: récupérer des données à partir de la méthode `GetProductsByCategoryID(categoryID)` ([cliquez pour afficher l’image en taille réelle](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image30.png))

La méthode choisie dans l’onglet sélectionner attend un paramètre. par conséquent, la dernière étape de l’Assistant nous invite à spécifier la source du paramètre. Définissez la liste déroulante source du paramètre sur Control et choisissez le contrôle `Categories` dans la liste déroulante ControlID. Cliquez sur Terminer pour terminer l'Assistant.

[![utiliser les catégories DropDownList comme source du paramètre categoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image32.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image31.png)

**Figure 11**: utiliser le `Categories` DropDownList comme source du paramètre `categoryID` ([cliquez pour afficher l’image en taille réelle](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image33.png))

Une fois l’exécution de l’Assistant ObjectDataSource terminée, Visual Studio ajoute BoundFields et CheckBoxField pour chacun des champs de données de produit. N’hésitez pas à personnaliser ces champs comme vous le souhaitez.

Accédez à la page via un navigateur. Lorsque vous accédez à la page, la catégorie boissons est sélectionnée et les produits correspondants sont listés dans la grille. La modification de la liste déroulante en une catégorie alternative, comme illustré à la figure 12, provoque une publication (postback) et recharge la grille avec les produits de la catégorie nouvellement sélectionnée.

[![les produits de la catégorie production sont affichés](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image35.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image34.png)

**Figure 12**: affichage des produits de la catégorie production ([cliquez pour afficher l’image en taille réelle](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image36.png))

## <a name="step-5-wrapping-a-stored-procedure-s-statements-within-the-scope-of-a-transaction"></a>Étape 5 : encapsulation d’une instruction de procédure stockée dans l’étendue d’une transaction

Dans le didacticiel sur les [modifications de base de données encapsulées dans une transaction](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) , nous avons abordé les techniques permettant d’effectuer une série d’instructions de modification de base de données dans l’étendue d’une transaction. Rappelez-vous que les modifications effectuées dans le cadre d’une transaction réussissent ou échouent, garantissant l’atomicité. Les techniques d’utilisation des transactions sont les suivantes :

- À l’aide des classes de l’espace de noms `System.Transactions`,
- Faire en sorte que la couche d’accès aux données utilise des classes ADO.NET comme `SqlTransaction`, et
- Ajout des commandes de transaction T-SQL directement dans la procédure stockée

Les *modifications de la base de données d’encapsulation dans un didacticiel de transaction* utilisaient les classes ADO.net dans la couche DAL. Le reste de ce didacticiel explique comment gérer une transaction à l’aide de commandes T-SQL à partir d’une procédure stockée.

Les trois commandes SQL clés pour démarrer, valider et restaurer manuellement une transaction sont `BEGIN TRANSACTION`, `COMMIT TRANSACTION`et `ROLLBACK TRANSACTION`, respectivement. Comme avec l’approche ADO.NET, lorsque vous utilisez des transactions à partir d’une procédure stockée, nous devons appliquer le modèle suivant :

1. Indique le début d’une transaction.
2. Exécutez les instructions SQL qui composent la transaction.
3. En cas d’erreur dans l’une des instructions de l’étape 2, restaurez la transaction.
4. Si toutes les instructions de l’étape 2 s’exécutent sans erreur, validez la transaction.

Ce modèle peut être implémenté dans la syntaxe T-SQL à l’aide du modèle suivant :

[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample4.sql)]

Le modèle commence par définir un bloc `TRY...CATCH`, une construction nouvelle pour SQL Server 2005. Comme avec les blocs `Try...Catch` dans Visual Basic, le bloc `TRY...CATCH` SQL exécute les instructions dans le bloc `TRY`. Si une instruction génère une erreur, le contrôle est immédiatement transféré au bloc `CATCH`.

S’il n’y a pas d’erreurs lors de l’exécution des instructions SQL qui composent la transaction, l’instruction `COMMIT TRANSACTION` valide les modifications et termine la transaction. Toutefois, si l’une des instructions génère une erreur, le `ROLLBACK TRANSACTION` dans le bloc `CATCH` retourne la base de données à son état avant le début de la transaction. La procédure stockée génère également une erreur à l’aide de la [commande RAISERROR](https://msdn.microsoft.com/library/ms178592.aspx), ce qui entraîne le déclenchement d’une `SqlException` dans l’application.

> [!NOTE]
> Étant donné que le bloc `TRY...CATCH` est nouveau dans SQL Server 2005, le modèle ci-dessus ne fonctionnera pas si vous utilisez des versions antérieures de Microsoft SQL Server. Si vous n’utilisez pas SQL Server 2005, consultez [gestion des transactions dans des procédures stockées SQL Server](http://www.4guysfromrolla.com/webtech/080305-1.shtml) pour un modèle qui fonctionne avec d’autres versions de SQL Server.

Observons un exemple concret. Une contrainte de clé étrangère existe entre les tables `Categories` et `Products`, ce qui signifie que chaque champ `CategoryID` dans la table `Products` doit être mappé à une valeur `CategoryID` dans la table `Categories`. Toute action qui violerait cette contrainte, telle que la tentative de suppression d’une catégorie associée à des produits, entraîne une violation de contrainte de clé étrangère. Pour vérifier cela, consultez à présent l’exemple mise à jour et suppression de données binaires existantes dans la section utilisation des données binaires (`~/BinaryData/UpdatingAndDeleting.aspx`). Cette page répertorie chaque catégorie du système, ainsi que les boutons modifier et supprimer (voir figure 13), mais si vous tentez de supprimer une catégorie associée à des produits (tels que boissons), la suppression échoue en raison d’une violation de contrainte de clé étrangère (voir figure 14).

[![chaque catégorie est affichée dans un GridView avec les boutons modifier et supprimer](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image38.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image37.png)

**Figure 13**: chaque catégorie est affichée dans un GridView avec les boutons modifier et supprimer ([cliquez pour afficher l’image en taille réelle](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image39.png))

[![vous ne pouvez pas supprimer une catégorie contenant des produits existants](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image41.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image40.png)

**Figure 14**: vous ne pouvez pas supprimer une catégorie qui contient des produits existants ([cliquez pour afficher l’image en taille réelle](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image42.png))

Imaginez, cependant, que nous souhaitons autoriser la suppression des catégories, qu’elles aient ou non des produits associés. En cas de suppression d’une catégorie avec des produits, imaginez que nous voulons également supprimer ses produits existants (bien qu’une autre option consiste à définir simplement ses produits `CategoryID` valeurs sur `NULL`). Cette fonctionnalité peut être implémentée à l’aide des règles de cascade de la contrainte de clé étrangère. Vous pouvez également créer une procédure stockée qui accepte un paramètre d’entrée `@CategoryID` et, lorsqu’elle est appelée, supprime explicitement tous les produits associés, puis la catégorie spécifiée.

Notre première tentative dans une telle procédure stockée peut se présenter comme suit :

[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample5.sql)]

Bien que cette opération supprime définitivement les produits et la catégorie associés, elle ne le fait pas dans le cadre d’une transaction. Imaginez qu’il existe une autre contrainte de clé étrangère sur `Categories` qui empêche la suppression d’une valeur de `@CategoryID` particulière. Le problème est que, dans ce cas, tous les produits seront supprimés avant d’essayer de supprimer la catégorie. Le résultat net est que pour une telle catégorie, cette procédure stockée supprime tous ses produits pendant que la catégorie est restée, car elle contient toujours des enregistrements associés dans une autre table.

Toutefois, si la procédure stockée a été encapsulée dans l’étendue d’une transaction, les suppressions de la table `Products` seraient annulées en cas d’échec de la suppression sur `Categories`. Le script de procédure stockée suivant utilise une transaction pour garantir l’atomicité entre les deux instructions `DELETE` :

[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample6.sql)]

Prenez un moment pour ajouter la procédure stockée `Categories_Delete` à la base de données Northwind. Reportez-vous à l’étape 1 pour obtenir des instructions sur l’ajout de procédures stockées à une base de données.

## <a name="step-6-updating-thecategoriestableadapter"></a>Étape 6 : mise à jour du`CategoriesTableAdapter`

Pendant que nous avons ajouté la procédure stockée `Categories_Delete` à la base de données, la couche DAL est actuellement configurée pour utiliser des instructions SQL ad hoc pour effectuer la suppression. Nous devons mettre à jour le `CategoriesTableAdapter` et lui demander d’utiliser la procédure stockée `Categories_Delete` à la place.

> [!NOTE]
> Plus haut dans ce didacticiel, nous travaillons avec le jeu de données `NorthwindWithSprocs`. Toutefois, ce jeu de données n’a qu’une seule entité, `ProductsDataTable`et nous devons travailler avec des catégories. Par conséquent, pour le reste de ce didacticiel, lorsque je parle de la couche d’accès aux données, j’ai fait référence au jeu de données `Northwind`, celui que nous avons créé dans le didacticiel sur la [création d’une couche d’accès aux données](../introduction/creating-a-data-access-layer-vb.md) .

Ouvrez le jeu de données Northwind, sélectionnez la `CategoriesTableAdapter`, puis accédez à la Fenêtre Propriétés. Le Fenêtre Propriétés répertorie les `InsertCommand`, `UpdateCommand`, `DeleteCommand`et `SelectCommand` utilisés par le TableAdapter, ainsi que son nom et les informations de connexion. Développez la propriété `DeleteCommand` pour afficher ses détails. Comme le montre la figure 15, la propriété `DeleteCommand` s `CommandType` est définie sur Text, ce qui lui indique d’envoyer le texte de la propriété `CommandText` en tant que requête SQL ad hoc.

![Sélectionnez le CategoriesTableAdapter dans le concepteur pour afficher ses propriétés dans la fenêtre Propriétés.](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image43.png)

**Figure 15**: sélectionner l' `CategoriesTableAdapter` dans le concepteur pour afficher ses propriétés dans la fenêtre Propriétés

Pour modifier ces paramètres, sélectionnez le texte (DeleteCommand) dans le Fenêtre Propriétés et choisissez (nouveau) dans la liste déroulante. Les paramètres des propriétés `CommandText`, `CommandType`et `Parameters` sont effacés. Ensuite, affectez à la propriété `CommandType` la valeur `StoredProcedure`, puis tapez le nom de la procédure stockée pour le `CommandText` (`dbo.Categories_Delete`). Si vous veillez à entrer les propriétés dans cet ordre, en premier lieu, le `CommandType` puis le `CommandText`-Visual Studio remplira automatiquement la collection de paramètres. Si vous n’entrez pas ces propriétés dans cet ordre, vous devez ajouter manuellement les paramètres par le biais de l’éditeur de collections Parameters. Dans les deux cas, il est prudent de cliquer sur les points de suspension dans la propriété Parameters pour afficher l’éditeur de collections Parameters afin de vérifier que les modifications des paramètres corrects ont été apportées (voir figure 16). Si vous ne voyez pas de paramètres dans la boîte de dialogue, ajoutez manuellement le paramètre `@CategoryID` (vous n’avez pas besoin d’ajouter le paramètre `@RETURN_VALUE`).

![Vérifier que les paramètres des paramètres sont corrects](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image44.png)

**Figure 16**: vérifier que les paramètres des paramètres sont corrects

Une fois la couche DAL mise à jour, la suppression d’une catégorie entraîne la suppression automatique de tous ses produits associés et le fait sous le cadre d’une transaction. Pour vérifier cela, revenez à la page mise à jour et suppression de données binaires existantes, puis cliquez sur le bouton Supprimer pour l’une des catégories. En un seul clic de souris, la catégorie et tous les produits associés sont supprimés.

> [!NOTE]
> Avant de tester la procédure stockée `Categories_Delete`, qui supprimera un certain nombre de produits, ainsi que la catégorie sélectionnée, il peut être prudent de faire une copie de sauvegarde de votre base de données. Si vous utilisez la base de données `NORTHWND.MDF` dans `App_Data`, fermez simplement Visual Studio et copiez les fichiers MDF et LDF dans `App_Data` dans un autre dossier. Après avoir testé la fonctionnalité, vous pouvez restaurer la base de données en fermant Visual Studio et en remplaçant les fichiers MDF et LDF actuels dans `App_Data` par les copies de sauvegarde.

## <a name="summary"></a>Récapitulatif

Tandis que l’Assistant TableAdapter s génère automatiquement des procédures stockées pour nous, il arrive que nous ayons déjà créé ces procédures stockées ou que vous souhaitiez les créer manuellement ou à l’aide d’autres outils. Pour prendre en charge de tels scénarios, le TableAdapter peut également être configuré pour pointer vers une procédure stockée existante. Dans ce didacticiel, nous avons vu comment ajouter manuellement des procédures stockées à une base de données par le biais de l’environnement Visual Studio et comment connecter les méthodes TableAdapter s à ces procédures stockées. Nous avons également examiné les commandes T-SQL et le modèle de script utilisés pour démarrer, valider et restaurer des transactions à partir d’une procédure stockée.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel étaient Hilton Geisenow, S Ren Jacob Lauritsen et Teresa Murphy. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
> [Suivant](updating-the-tableadapter-to-use-joins-vb.md)
