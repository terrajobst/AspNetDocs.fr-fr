---
uid: web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb
title: Encapsulation des modifications de base de données dans une transaction (VB) | Microsoft Docs
author: rick-anderson
description: Ce didacticiel est le premier d’une série de quatre qui recherchent la mise à jour, la suppression et l’insertion de lots de données. Dans ce didacticiel, nous allons découvrir comment les transactions de base de données le permettent...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: 7d821db5-6cbb-4b38-af14-198f9155fc82
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb
msc.type: authoredcontent
ms.openlocfilehash: dee95ee2789a69aac5aa79b8358e58e3ee99e1b2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78588689"
---
# <a name="wrapping-database-modifications-within-a-transaction-vb"></a>Inclusion de modifications d’une base de données dans une transaction (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_63_VB.zip) ou [Télécharger le PDF](wrapping-database-modifications-within-a-transaction-vb/_static/datatutorial63vb1.pdf)

> Ce didacticiel est le premier d’une série de quatre qui recherchent la mise à jour, la suppression et l’insertion de lots de données. Dans ce didacticiel, nous allons apprendre comment les transactions de base de données permettent d’effectuer des modifications de lot sous la forme d’une opération atomique, ce qui garantit que toutes les étapes réussissent ou que toutes les étapes échouent.

## <a name="introduction"></a>Introduction

Comme nous l’avons vu dans le didacticiel sur l' [insertion, la mise à jour et la suppression des données](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) , le contrôle GridView offre une prise en charge intégrée de la modification et de la suppression au niveau des lignes. En quelques clics de souris, il est possible de créer une interface de modification de données enrichie sans écrire une ligne de code, à condition que vous soyez contenu en modifiant et en supprimant par ligne. Toutefois, dans certains scénarios, cela est insuffisant et nous devons fournir aux utilisateurs la possibilité de modifier ou de supprimer un lot d’enregistrements.

Par exemple, la plupart des clients de messagerie Web utilisent une grille pour répertorier chaque message où chaque ligne contient une case à cocher avec les informations sur les courriers électroniques (objet, expéditeur, etc.). Cette interface permet à l’utilisateur de supprimer plusieurs messages en les vérifiant, puis en cliquant sur un bouton supprimer les messages sélectionnés. Une interface de modification par lot est idéale dans les situations où les utilisateurs modifient généralement un grand nombre d’enregistrements différents. Plutôt que de forcer l’utilisateur à cliquer sur modifier, apporter des modifications, puis cliquer sur mettre à jour pour chaque enregistrement qui doit être modifié, une interface de modification par lot effectue le rendu de chaque ligne avec son interface de modification. L’utilisateur peut modifier rapidement l’ensemble de lignes qui doivent être modifiées, puis enregistrer ces modifications en cliquant sur un bouton mettre à jour tout. Dans cet ensemble de didacticiels, nous allons examiner comment créer des interfaces pour l’insertion, la modification et la suppression de lots de données.

Lorsque vous effectuez des opérations de traitement par lots, il est important de déterminer s’il est possible que certaines opérations du lot aboutissent alors que d’autres échouent. Prenons l’exemple d’une interface de suppression de lot, ce qui doit se produire si le premier enregistrement sélectionné est correctement supprimé, mais le deuxième échoue, par exemple en raison d’une violation de contrainte de clé étrangère ? Le premier enregistrement doit-il être restauré ou est-il acceptable pour que le premier enregistrement reste supprimé ?

Si vous souhaitez que l’opération de traitement par lots soit traitée comme une [opération atomique](http://en.wikipedia.org/wiki/Atomic_operation), lorsque toutes les étapes réussissent ou que toutes les étapes échouent, la couche d’accès aux données doit être augmentée pour inclure la prise en charge des [transactions de base de données](http://en.wikipedia.org/wiki/Database_transaction). Les transactions de base de données garantissent l’atomicité pour l’ensemble des instructions `INSERT`, `UPDATE`et `DELETE` exécutées dans le cadre de la transaction et sont une fonctionnalité prise en charge par la plupart des systèmes de base de données modernes.

Dans ce didacticiel, nous allons examiner comment étendre la couche DAL afin d’utiliser des transactions de base de données. Les didacticiels suivants examinent la mise en œuvre des pages Web pour l’insertion, la mise à jour et la suppression d’interfaces par lots. Commençons !

> [!NOTE]
> Lorsque vous modifiez des données dans une transaction par lot, l’atomicité n’est pas toujours nécessaire. Dans certains scénarios, il peut être acceptable de faire en sorte que certaines modifications de données réussissent et que d’autres dans le même lot échouent, par exemple lors de la suppression d’un ensemble de messages d’un client de messagerie Web. Si une erreur de base de données survient au milieu du processus de suppression, il est probable que ces enregistrements traités sans erreur restent supprimés. Dans ce cas, la couche DAL n’a pas besoin d’être modifiée pour prendre en charge les transactions de base de données. Toutefois, il existe d’autres scénarios d’opération de traitement par lots où l’atomicité est vitale. Lorsqu’un client déplace ses fonds d’un compte bancaire vers un autre, deux opérations doivent être effectuées : les fonds doivent être déduits du premier compte, puis ajoutés à la seconde. Même si la Banque n’a pas l’esprit que la première étape réussit, mais que la deuxième étape échoue, ses clients peuvent être incorrects. Je vous encourage à suivre ce didacticiel et à implémenter les améliorations apportées à la couche DAL pour prendre en charge les transactions de base de données, même si vous n’envisagez pas de les utiliser dans les trois didacticiels d’insertion, de mise à jour et de suppression de lots.

## <a name="an-overview-of-transactions"></a>Vue d’ensemble des transactions

La plupart des bases de données incluent la prise en charge des *transactions*, qui permettent de regrouper plusieurs commandes de base de données en une seule unité logique de travail. Les commandes de base de données qui composent une transaction sont garanties atomiques, ce qui signifie que toutes les commandes échouent ou que tout fonctionnera correctement.

En général, les transactions sont implémentées via des instructions SQL à l’aide du modèle suivant :

1. Indique le début d’une transaction.
2. Exécutez les instructions SQL qui composent la transaction.
3. En cas d’erreur dans l’une des instructions de l’étape 2, restaurez la transaction.
4. Si toutes les instructions de l’étape 2 s’exécutent sans erreur, validez la transaction.

Les instructions SQL utilisées pour créer, valider et restaurer la transaction peuvent être entrées manuellement lors de l’écriture de scripts SQL ou de la création de procédures stockées, ou par programmation à l’aide de ADO.NET ou des classes de l' [espace de noms`System.Transactions`](https://msdn.microsoft.com/library/system.transactions.aspx). Dans ce didacticiel, nous allons examiner uniquement la gestion des transactions à l’aide de ADO.NET. Dans un prochain didacticiel, nous allons examiner comment utiliser des procédures stockées dans la couche d’accès aux données. à présent, nous allons explorer les instructions SQL pour créer, restaurer et valider des transactions. En attendant, consultez [gestion des transactions dans SQL Server procédures stockées](http://www.4guysfromrolla.com/webtech/080305-1.shtml) pour plus d’informations.

> [!NOTE]
> La [classe`TransactionScope`](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) de l’espace de noms `System.Transactions` permet aux développeurs d’encapsuler par programmation une série d’instructions dans l’étendue d’une transaction et inclut la prise en charge des transactions complexes qui impliquent plusieurs sources, telles que deux bases de données différentes ou même des types hétérogènes de magasins de données, tels qu’une base de données Microsoft SQL Server, une base de données Oracle et un service Web. J’ai décidé d’utiliser des transactions ADO.NET pour ce didacticiel au lieu de la classe `TransactionScope`, car ADO.NET est plus spécifique pour les transactions de base de données et, dans de nombreux cas, est beaucoup moins gourmand en ressources. En outre, dans certains scénarios, la classe `TransactionScope` utilise Microsoft Distributed Transaction Coordinator (MSDTC). La configuration, la mise en œuvre et les problèmes de performances liés à MSDTC en font un sujet plutôt spécialisé et avancé et dépassant le cadre de ces didacticiels.

Lorsque vous utilisez le fournisseur SqlClient dans ADO.NET, les transactions sont initiées via un appel à la méthode [`SqlConnection` Class](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) s [`BeginTransaction`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.begintransaction.aspx), qui retourne un [objet`SqlTransaction`](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.aspx). Les instructions de modification de données qui composent la transaction sont placées dans un bloc `try...catch`. Si une erreur se produit dans une instruction du bloc `try`, l’exécution est transférée vers le bloc `catch` dans lequel la transaction peut être restaurée à l’aide de la [méthode`Rollback`](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.rollback.aspx)`SqlTransaction` objet. Si toutes les instructions se terminent correctement, un appel à la méthode `SqlTransaction` objet s [`Commit`](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.commit.aspx) à la fin du bloc `try` valide la transaction. L’extrait de code suivant illustre ce modèle. Consultez [maintenance de la cohérence de la base de données avec des transactions](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx) pour obtenir une syntaxe supplémentaire et des exemples d’utilisation des transactions avec ADO.net.

[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample1.vb)]

Par défaut, les TableAdapters dans un DataSet typé n’utilisent pas de transactions. Pour assurer la prise en charge des transactions, nous devons augmenter les classes TableAdapter pour inclure des méthodes supplémentaires qui utilisent le modèle ci-dessus pour exécuter une série d’instructions de modification de données dans l’étendue d’une transaction. À l’étape 2, nous verrons comment utiliser des classes partielles pour ajouter ces méthodes.

## <a name="step-1-creating-the-working-with-batched-data-web-pages"></a>Étape 1 : création du travail avec des pages Web de données par lot

Avant de commencer à explorer l’augmentation de la couche DAL pour prendre en charge les transactions de base de données, commençons par prendre un moment pour créer les pages Web ASP.NET dont nous aurons besoin pour ce didacticiel et les trois qui suivent. Commencez par ajouter un nouveau dossier nommé `BatchData` puis ajoutez les pages ASP.NET suivantes, en associant chaque page à la page maître `Site.master`.

- `Default.aspx`
- `Transactions.aspx`
- `BatchUpdate.aspx`
- `BatchDelete.aspx`
- `BatchInsert.aspx`

![Ajouter les pages ASP.NET pour les didacticiels liés à SqlDataSource](wrapping-database-modifications-within-a-transaction-vb/_static/image1.gif)

**Figure 1**: ajouter les pages ASP.net pour les didacticiels liés à SqlDataSource

Comme pour les autres dossiers, `Default.aspx` utilise le contrôle utilisateur `SectionLevelTutorialListing.ascx` pour répertorier les didacticiels dans sa section. Par conséquent, ajoutez ce contrôle utilisateur à `Default.aspx` en le faisant glisser de l’Explorateur de solutions sur la page s Mode Création.

[![ajouter le contrôle utilisateur SectionLevelTutorialListing. ascx à default. aspx](wrapping-database-modifications-within-a-transaction-vb/_static/image2.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image1.png)

**Figure 2**: ajouter le contrôle utilisateur `SectionLevelTutorialListing.ascx` à `Default.aspx` ([cliquez pour afficher l’image en taille réelle](wrapping-database-modifications-within-a-transaction-vb/_static/image2.png))

Enfin, ajoutez ces quatre pages en tant qu’entrées au fichier `Web.sitemap`. Plus précisément, ajoutez le balisage suivant après la personnalisation du plan de site `<siteMapNode>`:

[!code-xml[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample2.xml)]

Après avoir mis à jour `Web.sitemap`, prenez un moment pour afficher le site Web des didacticiels via un navigateur. Le menu à gauche contient maintenant des éléments pour les didacticiels utilisation des données par lot.

![Le plan de site comprend désormais des entrées pour l’utilisation des didacticiels de données par lot](wrapping-database-modifications-within-a-transaction-vb/_static/image3.gif)

**Figure 3**: le plan de site contient maintenant des entrées pour les didacticiels utilisation des données par lot

## <a name="step-2-updating-the-data-access-layer-to-support-database-transactions"></a>Étape 2 : mise à jour de la couche d’accès aux données pour prendre en charge les transactions de base de données

Comme nous l’avons vu en arrière dans le premier didacticiel, [création d’une couche d’accès aux données](../introduction/creating-a-data-access-layer-vb.md), le DataSet typé dans notre couche DAL est composé de DataTables et de TableAdapters. Les DataTables contiennent des données tandis que les TableAdapters fournissent les fonctionnalités pour lire les données de la base de données dans les DataTables, pour mettre à jour la base de données avec les modifications apportées aux DataTables, et ainsi de suite. Rappelez-vous que les TableAdapters fournissent deux modèles pour la mise à jour des données, que j’ai désignés sous le terme de mise à jour par lot et de DB-direct. Avec le modèle de mise à jour par lot, le TableAdapter reçoit un DataSet, un DataTable ou une collection de DataRows. Ces données sont énumérées et, pour chaque ligne insérée, modifiée ou supprimée, le `InsertCommand`, `UpdateCommand`ou `DeleteCommand` est exécuté. Avec le modèle DB-direct, le TableAdapter reçoit à la place les valeurs des colonnes nécessaires pour l’insertion, la mise à jour ou la suppression d’un seul enregistrement. La méthode de modèle DB direct utilise ensuite ces valeurs transmises pour exécuter l’instruction `InsertCommand`, `UpdateCommand`ou `DeleteCommand` appropriée.

Quel que soit le modèle de mise à jour utilisé, les méthodes générées automatiquement par les TableAdapters n’utilisent pas de transactions. Par défaut, chaque insertion, mise à jour ou suppression effectuée par le TableAdapter est traitée comme une seule opération discrète. Par exemple, imaginez que le modèle DB-direct est utilisé par du code dans la couche BLL pour insérer dix enregistrements dans la base de données. Ce code appelle la méthode TableAdapter s `Insert` dix fois. Si les cinq premières insertions ont abouti, mais que la sixième a entraîné une exception, les cinq premiers enregistrements insérés resteront dans la base de données. De même, si le modèle de mise à jour par lot est utilisé pour effectuer des insertions, des mises à jour et des suppressions sur les lignes insérées, modifiées et supprimées dans un DataTable, si les premières modifications ont réussi, mais qu’une erreur plus tard a rencontré une erreur, ces modifications antérieures terminé reste dans la base de données.

Dans certains scénarios, nous voulons garantir l’atomicité sur une série de modifications. Pour ce faire, nous devons étendre manuellement le TableAdapter en ajoutant de nouvelles méthodes qui exécutent les `InsertCommand`, `UpdateCommand`et `DeleteCommand` dans le cadre d’une transaction. Lors de la [création d’une couche d’accès aux données](../introduction/creating-a-data-access-layer-vb.md) , nous avons examiné l’utilisation des [classes partielles](http://en.wikipedia.org/wiki/Partial_type) pour étendre les fonctionnalités des DataTables dans le DataSet typé. Cette technique peut également être utilisée avec les TableAdapters.

Le `Northwind.xsd` de DataSet typé se trouve dans le sous-dossier `DAL` `App_Code`. Créez un sous-dossier dans le dossier `DAL` nommé `TransactionSupport` et ajoutez un nouveau fichier de classe nommé `ProductsTableAdapter.TransactionSupport.vb` (voir figure 4). Ce fichier contiendra l’implémentation partielle du `ProductsTableAdapter` qui comprend des méthodes permettant d’effectuer des modifications de données à l’aide d’une transaction.

![Ajoutez un dossier nommé TransactionSupport et un fichier de classe nommé ProductsTableAdapter. TransactionSupport. vb](wrapping-database-modifications-within-a-transaction-vb/_static/image4.gif)

**Figure 4**: ajouter un dossier nommé `TransactionSupport` et un fichier de classe nommé `ProductsTableAdapter.TransactionSupport.vb`

Entrez le code suivant dans le fichier `ProductsTableAdapter.TransactionSupport.vb` :

[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample3.vb)]

Le mot clé `Partial` dans la déclaration de classe indique ici au compilateur que les membres ajoutés dans doivent être ajoutés à la classe `ProductsTableAdapter` dans l’espace de noms `NorthwindTableAdapters`. Notez l’instruction `Imports System.Data.SqlClient` en haut du fichier. Étant donné que le TableAdapter a été configuré pour utiliser le fournisseur SqlClient, il utilise en interne un objet [`SqlDataAdapter`](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter.aspx) pour émettre ses commandes à la base de données. Par conséquent, nous devons utiliser la classe `SqlTransaction` pour commencer la transaction, puis la valider ou la restaurer. Si vous utilisez un magasin de données autre que Microsoft SQL Server, vous devez utiliser le fournisseur approprié.

Ces méthodes fournissent les blocs de construction nécessaires au démarrage, à la restauration et à la validation d’une transaction. Ils sont marqués `Public`, ce qui leur permet d’être utilisés à partir de l' `ProductsTableAdapter`, d’une autre classe de la couche DAL ou d’une autre couche de l’architecture, telle que la couche BLL. `BeginTransaction` ouvre le `SqlConnection` interne du TableAdapter (si nécessaire), commence la transaction et l’assigne à la propriété `Transaction`, puis attache la transaction aux objets internes `SqlDataAdapter` s `SqlCommand`. `CommitTransaction` et `RollbackTransaction` appeler les méthodes `Transaction` `Commit` et `Rollback`, respectivement, avant de fermer l’objet `Connection` interne.

## <a name="step-3-adding-methods-to-update-and-delete-data-under-the-umbrella-of-a-transaction"></a>Étape 3 : ajout de méthodes pour mettre à jour et supprimer des données sous le parapluie d’une transaction

À l’issue de ces méthodes, nous sommes prêts à ajouter des méthodes à `ProductsDataTable` ou à la couche BLL qui exécute une série de commandes sous le parapluie d’une transaction. La méthode suivante utilise le modèle de mise à jour par lot pour mettre à jour une instance de `ProductsDataTable` à l’aide d’une transaction. Il démarre une transaction en appelant la méthode `BeginTransaction`, puis utilise un bloc `Try...Catch` pour émettre les instructions de modification de données. Si l’appel à la méthode `Adapter` objet s `Update` provoque une exception, l’exécution est transférée vers le bloc `catch` où la transaction sera restaurée et l’exception relancée. N’oubliez pas que la méthode `Update` implémente le modèle de mise à jour par lot en énumérant les lignes du `ProductsDataTable` fourni et en effectuant les `InsertCommand`, `UpdateCommand`et `DeleteCommand` nécessaires. Si l’une de ces commandes génère une erreur, la transaction est annulée, ce qui annule les modifications précédentes effectuées pendant la durée de vie de la transaction. Si l’instruction `Update` se termine sans erreur, la transaction est validée dans son intégralité.

[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample4.vb)]

Ajoutez la méthode `UpdateWithTransaction` à la classe `ProductsTableAdapter` par le biais de la classe partielle dans `ProductsTableAdapter.TransactionSupport.vb`. Cette méthode peut également être ajoutée à la classe `ProductsBLL` de la couche de logique métier avec quelques modifications syntaxiques mineures. À savoir, le mot clé `Me` dans `Me.BeginTransaction()`, `Me.CommitTransaction()`et `Me.RollbackTransaction()` doit être remplacé par `Adapter` (Rappelez-vous que `Adapter` est le nom d’une propriété dans `ProductsBLL` de type `ProductsTableAdapter`).

La méthode `UpdateWithTransaction` utilise le modèle de mise à jour par lot, mais une série d’appels directs de base de code peut également être utilisée dans l’étendue d’une transaction, comme le montre la méthode suivante. La méthode `DeleteProductsWithTransaction` accepte comme entrée un `List(Of T)` de type `Integer`, qui sont les `ProductID` s à supprimer. La méthode lance la transaction via un appel à `BeginTransaction` puis, dans le bloc `Try`, itère au sein de la liste fournie appelant la méthode DB-direct pattern `Delete` pour chaque valeur `ProductID`. Si l’un des appels à `Delete` échoue, le contrôle est transféré au bloc `Catch` dans lequel la transaction est restaurée et l’exception est de nouveau levée. Si tous les appels à `Delete` réussie, la transaction est validée. Ajoutez cette méthode à la classe `ProductsBLL`.

[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample5.vb)]

## <a name="applying-transactions-across-multiple-tableadapters"></a>Application de transactions sur plusieurs TableAdapters

Le code lié aux transactions examiné dans ce didacticiel permet de traiter plusieurs instructions sur les `ProductsTableAdapter` comme une opération atomique. Mais que se passe-t-il si plusieurs modifications apportées à différentes tables de base de données doivent être effectuées atomiquement ? Par exemple, lors de la suppression d’une catégorie, nous pourrions d’abord réaffecter ses produits actuels à une autre catégorie. Ces deux étapes qui réassignent les produits et suppriment la catégorie doivent être exécutées en tant qu’opération atomique. Toutefois, le `ProductsTableAdapter` comprend uniquement des méthodes pour modifier la table `Products` et le `CategoriesTableAdapter` comprend uniquement des méthodes de modification de la table `Categories`. Comment une transaction peut-elle englober les deux TableAdapters ?

L’une des options consiste à ajouter une méthode au `CategoriesTableAdapter` nommé `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` et à faire en sorte que cette méthode appelle une procédure stockée qui réaffecte les produits et supprime la catégorie dans l’étendue d’une transaction définie dans la procédure stockée. Nous allons voir comment commencer, valider et restaurer des transactions dans des procédures stockées dans un prochain didacticiel.

Une autre option consiste à créer une classe d’assistance dans la couche DAL qui contient la méthode `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)`. Cette méthode crée une instance du `CategoriesTableAdapter` et le `ProductsTableAdapter` puis définit ces deux TableAdapters `Connection` propriétés sur la même instance de `SqlConnection`. À ce stade, l’un des deux TableAdapters initie la transaction avec un appel à `BeginTransaction`. Les méthodes TableAdapters pour réassigner les produits et supprimer la catégorie sont appelées dans un bloc de `Try...Catch` avec la transaction validée ou restaurée en fonction des besoins.

## <a name="step-4-adding-theupdatewithtransactionmethod-to-the-business-logic-layer"></a>Étape 4 : ajout de la méthode`UpdateWithTransaction`à la couche de logique métier

À l’étape 3, nous avons ajouté une méthode `UpdateWithTransaction` à la `ProductsTableAdapter` dans la couche DAL. Nous devons ajouter une méthode correspondante à la couche BLL. Alors que la couche de présentation peut appeler directement la couche DAL pour appeler la méthode `UpdateWithTransaction`, ces didacticiels ont pour objet de définir une architecture en couches qui isole la couche DAL de la couche de présentation. Par conséquent, il est nécessaire de poursuivre cette approche.

Ouvrez le fichier de classe `ProductsBLL` et ajoutez une méthode nommée `UpdateWithTransaction` qui appelle simplement la méthode DAL correspondante. Il doit maintenant y avoir deux nouvelles méthodes dans `ProductsBLL`: `UpdateWithTransaction`, que vous venez d’ajouter, et `DeleteProductsWithTransaction`, qui a été ajouté à l’étape 3.

[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample6.vb)]

> [!NOTE]
> Ces méthodes n’incluent pas l’attribut `DataObjectMethodAttribute` affecté à la plupart des autres méthodes de la classe `ProductsBLL`, car nous appellerons ces méthodes directement à partir des classes code-behind des pages ASP.NET. Rappelez-vous que `DataObjectMethodAttribute` est utilisé pour marquer les méthodes qui doivent s’afficher dans l’Assistant ObjectDataSource s configurer la source de données et sous quel onglet (SELECT, UPDATE, INSERT ou DELETE). Étant donné que le GridView ne dispose pas d’une prise en charge intégrée pour la modification ou la suppression de lots, nous devrons appeler ces méthodes par programmation plutôt que d’utiliser l’approche déclarative sans code.

## <a name="step-5-atomically-updating-database-data-from-the-presentation-layer"></a>Étape 5 : mise à jour atomique des données de la base de données à partir de la couche de présentation

Pour illustrer l’effet de la transaction lors de la mise à jour d’un lot d’enregistrements, créez une interface utilisateur qui répertorie tous les produits d’un GridView et inclut un contrôle Web Button qui, lorsque vous cliquez dessus, réassigne les valeurs de `CategoryID` des produits. En particulier, la réaffectation de la catégorie progresse, de sorte que les premiers produits reçoivent une valeur de `CategoryID` valide alors que d’autres reçoivent intentionnellement une valeur de `CategoryID` inexistante. Si nous tentons de mettre à jour la base de données avec un produit dont le `CategoryID` ne correspond pas à une catégorie existante de `CategoryID`, une violation de contrainte de clé étrangère se produit et une exception est levée. Ce que nous verrons dans cet exemple, c’est que lorsque vous utilisez une transaction, l’exception levée à partir de la violation de contrainte de clé étrangère entraîne la restauration des modifications de `CategoryID` valides précédentes. Toutefois, lorsque vous n’utilisez pas de transaction, les modifications apportées aux catégories initiales sont conservées.

Commencez par ouvrir la page `Transactions.aspx` dans le dossier `BatchData` et faites glisser un contrôle GridView de la boîte à outils vers le concepteur. Définissez ses `ID` sur `Products` et, à partir de sa balise active, liez-le à un nouvel ObjectDataSource nommé `ProductsDataSource`. Configurez le ObjectDataSource pour extraire ses données de la méthode de `GetProducts` de la classe `ProductsBLL`. Il s’agit d’un GridView en lecture seule. par conséquent, définissez les listes déroulantes dans les onglets mettre à jour, insérer et supprimer sur (aucun), puis cliquez sur Terminer.

[![configurer ObjectDataSource pour utiliser la méthode ProductsBLL de la classe s GetProducts](wrapping-database-modifications-within-a-transaction-vb/_static/image5.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image3.png)

**Figure 5**: configurer ObjectDataSource pour utiliser la méthode `ProductsBLL` classe s `GetProducts` ([cliquez pour afficher l’image en taille réelle](wrapping-database-modifications-within-a-transaction-vb/_static/image4.png))

[![définir les listes déroulantes dans les onglets mettre à jour, insérer et supprimer sur (aucun)](wrapping-database-modifications-within-a-transaction-vb/_static/image6.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image5.png)

**Figure 6**: définir les listes déroulantes des onglets mettre à jour, insérer et supprimer sur (aucun) ([cliquez pour afficher l’image en taille réelle](wrapping-database-modifications-within-a-transaction-vb/_static/image6.png))

À l’issue de l’exécution de l’Assistant Configuration de la source de données, Visual Studio crée BoundFields et CheckBoxField pour les champs de données de produit. Supprimez tous ces champs à l’exception de `ProductID`, `ProductName`, `CategoryID`et `CategoryName` et renommez les propriétés `ProductName` et `CategoryName` BoundFields `HeaderText` en Product et Category, respectivement. À partir de la balise active, activez l’option Activer la pagination. Une fois ces modifications apportées, les balises déclaratives GridView et ObjectDataSource doivent ressembler à ce qui suit :

[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample7.aspx)]

Ensuite, ajoutez trois contrôles Web Button au-dessus de GridView. Définissez la propriété Text du premier bouton sur Actualiser la grille, la seconde pour modifier les catégories (avec TRANSACTION) et la troisième pour modifier les catégories (sans TRANSACTION).

[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample8.aspx)]

À ce stade, le Mode Création dans Visual Studio doit ressembler à la capture d’écran illustrée à la figure 7.

[![la page contient un contrôle GridView et trois contrôles Web Button](wrapping-database-modifications-within-a-transaction-vb/_static/image7.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image7.png)

**Figure 7**: la page contient un GridView et trois contrôles Web Button ([cliquez pour afficher l’image en taille réelle](wrapping-database-modifications-within-a-transaction-vb/_static/image8.png))

Créez des gestionnaires d’événements pour chacun des trois événements Button s `Click` et utilisez le code suivant :

[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample9.vb)]

Les boutons actualiser le gestionnaire d’événements `Click` relient simplement les données au contrôle GridView en appelant la méthode `Products` GridView s `DataBind`.

Le deuxième gestionnaire d’événements réaffecte les produits `CategoryID` s et utilise la nouvelle méthode de transaction de la couche BLL pour effectuer les mises à jour de la base de données sous le parapluie d’une transaction. Notez que chaque `CategoryID` produit est défini arbitrairement sur la même valeur que son `ProductID`. Cela fonctionnera correctement pour les premiers produits, car ces produits ont des valeurs de `ProductID` qui se mappent à des `CategoryID` s valides. Toutefois, une fois que les `ProductID` s commencent à être trop volumineux, ce chevauchement des `ProductID` s et `CategoryID` s ne s’applique plus.

Le troisième `Click` gestionnaire d’événements met à jour les produits `CategoryID` s de la même manière, mais envoie la mise à jour à la base de données à l’aide de la méthode de `Update` par défaut de `ProductsTableAdapter`. Cette méthode de `Update` n’encapsule pas la série de commandes dans une transaction ; par conséquent, ces modifications sont apportées avant la première erreur de violation de contrainte de clé étrangère.

Pour illustrer ce comportement, visitez cette page via un navigateur. Au départ, vous devriez voir la première page de données, comme illustré à la figure 8. Ensuite, cliquez sur le bouton modifier les catégories (avec TRANSACTION). Cela entraîne une publication (postback) et tente de mettre à jour tous les produits `CategoryID` valeurs, mais entraîne une violation de contrainte de clé étrangère (voir figure 9).

[![les produits sont affichés dans un contrôle GridView paginable](wrapping-database-modifications-within-a-transaction-vb/_static/image8.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image9.png)

**Figure 8**: les produits s’affichent dans un contrôle GridView paginable ([cliquez pour afficher l’image en taille réelle](wrapping-database-modifications-within-a-transaction-vb/_static/image10.png))

[![la réassignation des résultats des catégories dans une violation de contrainte de clé étrangère](wrapping-database-modifications-within-a-transaction-vb/_static/image9.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image11.png)

**Figure 9**: réassignation des catégories résultats dans une violation de contrainte de clé étrangère ([cliquez pour afficher l’image en taille réelle](wrapping-database-modifications-within-a-transaction-vb/_static/image12.png))

Maintenant, appuyez sur le bouton précédent de votre navigateur, puis cliquez sur le bouton actualiser la grille. Lors de l’actualisation des données, vous devez voir exactement la même sortie, comme illustré à la figure 8. Autrement dit, bien que certains des produits `CategoryID` s aient été modifiés en valeurs autorisées et mis à jour dans la base de données, ils étaient restaurés lorsque la violation de contrainte de clé étrangère s’est produite.

Maintenant, essayez de cliquer sur le bouton modifier les catégories (sans TRANSACTION). Cela entraînera la même erreur de violation de contrainte de clé étrangère (voir la figure 9), mais cette fois-ci, les produits dont les valeurs de `CategoryID` ont été modifiées en valeur légale ne seront pas restaurés. Appuyez sur le bouton précédent de votre navigateur, puis sur le bouton actualiser la grille. Comme le montre la figure 10, les `CategoryID` des huit premiers produits ont été réaffectées. Par exemple, dans la figure 8, Chang avait une `CategoryID` de 1, mais dans la figure 10, elle a été réaffectée à 2.

[![certaines valeurs CategoryID des produits ont été mises à jour alors que d’autres n’étaient pas](wrapping-database-modifications-within-a-transaction-vb/_static/image10.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image13.png)

**Figure 10**: certains produits `CategoryID` valeurs ont été mis à jour alors que d’autres ne le sont pas ([cliquez pour afficher l’image en taille réelle](wrapping-database-modifications-within-a-transaction-vb/_static/image14.png))

## <a name="summary"></a>Récapitulatif

Par défaut, les méthodes de TableAdapter s n’encapsulent pas les instructions de base de données exécutées dans l’étendue d’une transaction, mais avec un peu de travail, nous pouvons ajouter des méthodes qui créeront, valideront et restaureront une transaction. Dans ce didacticiel, nous avons créé trois méthodes de ce type dans la classe `ProductsTableAdapter` : `BeginTransaction`, `CommitTransaction`et `RollbackTransaction`. Nous avons vu comment utiliser ces méthodes avec un bloc de `Try...Catch` pour rendre atomiques une série d’instructions de modification de données. En particulier, nous avons créé la méthode `UpdateWithTransaction` dans le `ProductsTableAdapter`, qui utilise le modèle de mise à jour par lot pour effectuer les modifications nécessaires aux lignes d’un `ProductsDataTable`fourni. Nous avons également ajouté la méthode `DeleteProductsWithTransaction` à la classe `ProductsBLL` dans la couche BLL, qui accepte une `List` de `ProductID` valeurs comme entrée et appelle la méthode DB-direct pattern `Delete` pour chaque `ProductID`. Les deux méthodes commencent par créer une transaction et à exécuter les instructions de modification de données au sein d’un bloc `Try...Catch`. Si une exception se produit, la transaction est restaurée, sinon elle est validée.

L’étape 5 illustre l’effet des mises à jour transactionnelles par lot par rapport aux mises à jour par lots qui ont négligé d’utiliser une transaction. Dans les trois didacticiels suivants, nous allons nous appuyer sur les fondations de ce didacticiel et créer des interfaces utilisateur pour effectuer des mises à jour, des suppressions et des insertions par lots.

Bonne programmation !

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Maintien de la cohérence de la base de données avec les transactions](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx)
- [Gestion des transactions dans des procédures stockées SQL Server](http://www.4guysfromrolla.com/webtech/080305-1.shtml)
- [Transactions simplifiées : `System.Transactions`](https://blogs.msdn.com/florinlazar/archive/2004/07/23/192239.aspx)
- [TransactionScope et DataAdapters](http://andyclymer.blogspot.com/2007/01/transactionscope-and-dataadapters.html)
- [Utilisation des transactions Oracle Database dans .NET](http://www.oracle.com/technology/pub/articles/price_dbtrans_dotnet.html)

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel étaient Dave Gardner, Hilton Giesenow et Teresa Murphy. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](batch-inserting-cs.md)
> [Suivant](batch-updating-vb.md)
