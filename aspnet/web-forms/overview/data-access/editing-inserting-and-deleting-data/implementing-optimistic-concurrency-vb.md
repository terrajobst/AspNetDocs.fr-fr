---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb
title: Implémentation de l’accès concurrentiel optimiste (VB) | Microsoft Docs
author: rick-anderson
description: Pour une application Web qui permet à plusieurs utilisateurs de modifier des données, il existe un risque que deux utilisateurs modifient les mêmes données en même temps. Dans ce...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 2646968c-2826-4418-b1d0-62610ed177e3
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb
msc.type: authoredcontent
ms.openlocfilehash: 28c39fe2a290cc3a5b093fdd09de341630606137
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74628824"
---
# <a name="implementing-optimistic-concurrency-vb"></a>Implémentation de l’accès concurrentiel optimiste (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_21_VB.exe) ou [Télécharger le PDF](implementing-optimistic-concurrency-vb/_static/datatutorial21vb1.pdf)

> Pour une application Web qui permet à plusieurs utilisateurs de modifier des données, il existe un risque que deux utilisateurs modifient les mêmes données en même temps. Dans ce didacticiel, nous allons implémenter le contrôle d’accès concurrentiel optimiste pour gérer ce risque.

## <a name="introduction"></a>Introduction

Pour les applications Web qui permettent uniquement aux utilisateurs d’afficher des données, ou pour ceux qui n’incluent qu’un seul utilisateur qui peut modifier des données, il n’y a aucune menace que deux utilisateurs simultanés remplacent accidentellement les modifications d’une autre. Toutefois, pour les applications Web qui permettent à plusieurs utilisateurs de mettre à jour ou de supprimer des données, il est possible que les modifications d’un utilisateur soient en conflit avec les autres utilisateurs simultanés. Sans aucune stratégie d’accès concurrentiel en place, lorsque deux utilisateurs modifient simultanément un seul enregistrement, l’utilisateur qui valide les modifications en dernier remplace les modifications apportées par le premier.

Par exemple, imaginez que deux utilisateurs, Jisun et Sam, visitaient une page de notre application qui permettait aux visiteurs de mettre à jour et de supprimer les produits via un contrôle GridView. Les deux cliquez sur le bouton modifier dans le GridView en même temps. Jisun remplace le nom de produit par « Chai thé », puis clique sur le bouton mettre à jour. Le résultat net est une instruction `UPDATE` qui est envoyée à la base de données, qui définit *tous* les champs modifiables du produit (même si Jisun a mis à jour un seul champ, `ProductName`). À ce stade, la base de données a les valeurs « Chai thé », les boissons de catégorie, les fournisseurs exotiques, etc. pour ce produit particulier. Toutefois, le contrôle GridView sur l’écran du Sam affiche toujours le nom du produit dans la ligne de GridView modifiable en tant que « Chai ». Quelques secondes après la validation des modifications de Jisun, Sam met à jour la catégorie en condiments et clique sur mettre à jour. Il en résulte une instruction `UPDATE` envoyée à la base de données qui définit le nom du produit sur « Chai », le `CategoryID` à l’ID de catégorie de boissons correspondant, et ainsi de suite. Les modifications apportées au nom du produit par Jisun ont été remplacées. La figure 1 représente graphiquement cette série d’événements.

[![lorsque deux utilisateurs mettent à jour simultanément un enregistrement, il est possible que les modifications d’un utilisateur remplacent les autres](implementing-optimistic-concurrency-vb/_static/image2.png)](implementing-optimistic-concurrency-vb/_static/image1.png)

**Figure 1**: lorsque deux utilisateurs mettent à jour simultanément un enregistrement, il est possible que les modifications d’un utilisateur remplacent les autres ([cliquez pour afficher l’image en plein écran](implementing-optimistic-concurrency-vb/_static/image3.png))

De même, lorsque deux utilisateurs visitent une page, un utilisateur peut être au milieu de la mise à jour d’un enregistrement lorsqu’il est supprimé par un autre utilisateur. Ou bien, entre le moment où un utilisateur charge une page et le moment où il clique sur le bouton supprimer, un autre utilisateur a peut-être modifié le contenu de cet enregistrement.

Trois stratégies de [contrôle d’accès concurrentiel](http://en.wikipedia.org/wiki/Concurrency_control) sont disponibles :

- **Ne rien faire** : si des utilisateurs simultanés modifient le même enregistrement, laissez la dernière validation Win (comportement par défaut)
- [**Accès concurrentiel optimiste**](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) : Partez du principe qu’il peut y avoir des conflits d’accès concurrentiel à chaque instant, puis, la grande majorité du temps, ces conflits ne se produiront pas. par conséquent, en cas de conflit, informez simplement l’utilisateur que les modifications apportées ne peuvent pas être enregistrées, car un autre utilisateur a modifié les mêmes données
- **Accès concurrentiel pessimiste** : Supposons que les conflits d’accès concurrentiel sont courants et que les utilisateurs ne tolèrent pas que leurs modifications n’ont pas été enregistrées en raison de l’activité simultanée d’un autre utilisateur. par conséquent, lorsqu’un utilisateur commence à mettre à jour un enregistrement, le verrouille, empêchant ainsi les autres utilisateurs de modifier ou de supprimer cet enregistrement jusqu’à ce que l’utilisateur valide ses modifications.

Tous nos didacticiels, jusqu’à présent, ont utilisé la stratégie de résolution de concurrence par défaut, à savoir, nous avons laissé le dernier succès à l’écriture. Dans ce didacticiel, nous allons examiner comment implémenter le contrôle d’accès concurrentiel optimiste.

> [!NOTE]
> Nous n’examinerons pas les exemples d’accès concurrentiel pessimiste dans cette série de didacticiels. L’accès concurrentiel pessimiste est rarement utilisé, car ces verrous, s’ils ne sont pas correctement libérés, peuvent empêcher d’autres utilisateurs de mettre à jour les données. Par exemple, si un utilisateur verrouille un enregistrement pour le modifier, puis laisse le jour avant de le déverrouiller, aucun autre utilisateur ne pourra mettre à jour cet enregistrement jusqu’à ce que l’utilisateur d’origine retourne et termine sa mise à jour. Par conséquent, dans les situations où l’accès concurrentiel pessimiste est utilisé, il y a généralement un délai d’expiration qui, s’il est atteint, annule le verrou. Les sites Web de vente de tickets, qui verrouillent un emplacement particulier pour une période brève pendant que l’utilisateur termine le processus de commande, est un exemple de contrôle d’accès concurrentiel pessimiste.

## <a name="step-1-looking-at-how-optimistic-concurrency-is-implemented"></a>Étape 1 : examen de l’implémentation de l’accès concurrentiel optimiste

Le contrôle d’accès concurrentiel optimiste fonctionne en s’assurant que l’enregistrement en cours de mise à jour ou de suppression a les mêmes valeurs que lors du démarrage de la mise à jour ou de la suppression. Par exemple, quand vous cliquez sur le bouton modifier dans un GridView modifiable, les valeurs de l’enregistrement sont lues à partir de la base de données et affichées dans des zones de texte et d’autres contrôles Web. Ces valeurs d’origine sont enregistrées par le contrôle GridView. Plus tard, une fois que l’utilisateur a effectué ses modifications et cliqué sur le bouton mettre à jour, les valeurs d’origine plus les nouvelles valeurs sont envoyées à la couche de logique métier, puis vers la couche d’accès aux données. La couche d’accès aux données doit émettre une instruction SQL qui met à jour l’enregistrement uniquement si les valeurs d’origine que l’utilisateur a commencé à modifier sont identiques aux valeurs figurant toujours dans la base de données. La figure 2 illustre cette séquence d’événements.

[![pour que la mise à jour ou la suppression aboutisse, les valeurs d’origine doivent être égales aux valeurs de la base de données actuelle](implementing-optimistic-concurrency-vb/_static/image5.png)](implementing-optimistic-concurrency-vb/_static/image4.png)

**Figure 2**: pour que la mise à jour ou la suppression aboutisse, les valeurs d’origine doivent être égales aux valeurs de la base de données actuelle ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image6.png))

Il existe différentes approches pour implémenter l’accès concurrentiel optimiste (consultez la logique de [mise à jour de la concurrence optimiste](http://www.eggheadcafe.com/articles/20050719.asp) de [Peter A. Bromberg](http://peterbromberg.net/)pour obtenir un bref aperçu d’un certain nombre d’options). Le DataSet typé ADO.NET fournit une implémentation qui peut être configurée uniquement à l’aide du cycle d’une case à cocher. L’activation de l’accès concurrentiel optimiste pour un TableAdapter dans le DataSet typé augmente les instructions `UPDATE` et `DELETE` du TableAdapter pour inclure une comparaison de toutes les valeurs d’origine dans la clause `WHERE`. Par exemple, l’instruction `UPDATE` suivante met à jour le nom et le prix d’un produit uniquement si les valeurs de la base de données actuelle sont égales aux valeurs récupérées à l’origine lors de la mise à jour de l’enregistrement dans le GridView. Les paramètres `@ProductName` et `@UnitPrice` contiennent les nouvelles valeurs entrées par l’utilisateur, tandis que `@original_ProductName` et `@original_UnitPrice` contiennent les valeurs qui ont été chargées à l’origine dans le contrôle GridView lors d’un clic sur le bouton modifier :

[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample1.sql)]

> [!NOTE]
> Cette instruction `UPDATE` a été simplifiée pour des raisons de lisibilité. Dans la pratique, la vérification `UnitPrice` dans la clause `WHERE` est plus complexe, car `UnitPrice` peut contenir `NULL` s et vérifier si `NULL = NULL` retourne toujours la valeur false (à la place, vous devez utiliser `IS NULL`).

Outre l’utilisation d’une instruction `UPDATE` sous-jacente différente, la configuration d’un TableAdapter pour utiliser l’accès concurrentiel optimiste modifie également la signature de ses méthodes directes de base de données. Rappelez-vous de notre premier didacticiel, [*création d’une couche d’accès aux données*](../introduction/creating-a-data-access-layer-cs.md), que les méthodes de base de données directes étaient celles qui acceptent une liste de valeurs scalaires comme paramètres d’entrée (plutôt qu’une instance DataRow ou DataTable fortement typée). Lors de l’utilisation de l’accès concurrentiel optimiste, les méthodes DB direct `Update()` et `Delete()` incluent également des paramètres d’entrée pour les valeurs d’origine. En outre, le code de la couche BLL pour l’utilisation du modèle de mise à jour par lot (les surcharges de méthode `Update()` qui acceptent des DataRow et des DataTables plutôt que des valeurs scalaires) doit également être modifié.

Au lieu d’étendre les TableAdapters de la couche DAL existante pour utiliser l’accès concurrentiel optimiste (ce qui nécessiterait la modification de la couche BLL pour s’adapter à), créons à la place un nouveau DataSet typé nommé `NorthwindOptimisticConcurrency`, auquel nous ajouterons un TableAdapter `Products` qui utilise l’accès concurrentiel optimiste. Après cela, nous allons créer une classe de couche de logique métier `ProductsOptimisticConcurrencyBLL` qui présente les modifications appropriées pour prendre en charge la couche DAL d’accès concurrentiel optimiste. Une fois cette opération posée, nous sommes prêts à créer la page ASP.NET.

## <a name="step-2-creating-a-data-access-layer-that-supports-optimistic-concurrency"></a>Étape 2 : création d’une couche d’accès aux données qui prend en charge l’accès concurrentiel optimiste

Pour créer un nouveau DataSet typé, cliquez avec le bouton droit sur le dossier `DAL` dans le dossier `App_Code` et ajoutez un nouveau jeu de données nommé `NorthwindOptimisticConcurrency`. Comme nous l’avons vu dans le premier didacticiel, cette opération ajoute un nouveau TableAdapter au DataSet typé, en lançant automatiquement l’Assistant Configuration de TableAdapter. Dans le premier écran, nous sommes invités à spécifier la base de données à laquelle se connecter : Connectez-vous à la même base de données Northwind à l’aide du paramètre `NORTHWNDConnectionString` de `Web.config`.

[![se connecter à la même base de données Northwind](implementing-optimistic-concurrency-vb/_static/image8.png)](implementing-optimistic-concurrency-vb/_static/image7.png)

**Figure 3**: se connecter à la même base de données Northwind ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image9.png))

Ensuite, nous sommes invités à interroger les données : par le biais d’une instruction SQL ad hoc, d’une nouvelle procédure stockée ou d’une procédure stockée existante. Étant donné que nous avons utilisé des requêtes SQL ad hoc dans notre couche DAL d’origine, utilisez cette option ici également.

[![spécifier les données à récupérer à l’aide d’une instruction SQL ad hoc](implementing-optimistic-concurrency-vb/_static/image11.png)](implementing-optimistic-concurrency-vb/_static/image10.png)

**Figure 4**: spécifier les données à récupérer à l’aide d’une instruction SQL ad hoc ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image12.png))

Dans l’écran suivant, entrez la requête SQL à utiliser pour récupérer les informations sur le produit. Nous allons utiliser exactement la même requête SQL que celle utilisée pour le `Products` TableAdapter à partir de notre couche DAL d’origine, qui renvoie toutes les colonnes de `Product`, ainsi que les noms des fournisseurs et des catégories du produit :

[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample2.sql)]

[![utiliser la même requête SQL à partir du TableAdapter Products dans la couche DAL d’origine](implementing-optimistic-concurrency-vb/_static/image14.png)](implementing-optimistic-concurrency-vb/_static/image13.png)

**Figure 5**: utiliser la même requête SQL à partir du `Products` TableAdapter dans la couche DAL d’origine ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image15.png))

Avant de passer à l’écran suivant, cliquez sur le bouton Options avancées. Pour que ce TableAdapter utilise le contrôle d’accès concurrentiel optimiste, activez simplement la case à cocher « utiliser l’accès concurrentiel optimiste ».

[![activer le contrôle d’accès concurrentiel optimiste en activant la case à cocher &quot;utiliser l’accès concurrentiel optimiste&quot;](implementing-optimistic-concurrency-vb/_static/image17.png)](implementing-optimistic-concurrency-vb/_static/image16.png)

**Figure 6**: activer le contrôle d’accès concurrentiel optimiste en cochant la case « utiliser l’accès concurrentiel optimiste » ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image18.png))

Enfin, indiquez que le TableAdapter doit utiliser les modèles d’accès aux données qui remplissent un DataTable et retournent un DataTable ; Indiquez également que les méthodes directes de la base de Modifiez le nom de la méthode pour retourner un modèle de DataTable de GetData en GetProducts, afin de refléter les conventions d’affectation de noms que nous avons utilisées dans notre couche d’origine.

[![que le TableAdapter utilise tous les modèles d’accès aux données](implementing-optimistic-concurrency-vb/_static/image20.png)](implementing-optimistic-concurrency-vb/_static/image19.png)

**Figure 7**: faire en sorte que le TableAdapter utilise tous les modèles d’accès aux données ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image21.png))

Une fois l’exécution de l’Assistant terminée, le concepteur de DataSet inclura un `Products` et un DataTable fortement typés. Prenez un moment pour renommer le DataTable de `Products` en `ProductsOptimisticConcurrency`, ce que vous pouvez faire en cliquant avec le bouton droit sur la barre de titre du DataTable et en choisissant renommer dans le menu contextuel.

[![un DataTable et un TableAdapter ont été ajoutés au DataSet typé](implementing-optimistic-concurrency-vb/_static/image23.png)](implementing-optimistic-concurrency-vb/_static/image22.png)

**Figure 8**: un DataTable et un TableAdapter ont été ajoutés au DataSet typé ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image24.png))

Pour voir les différences entre les requêtes `UPDATE` et `DELETE` entre le TableAdapter `ProductsOptimisticConcurrency` (qui utilise l’accès concurrentiel optimiste) et le TableAdapter Products (qui n’est pas), cliquez sur le TableAdapter et accédez au Fenêtre Propriétés. Dans les propriétés `DeleteCommand` et `UpdateCommand` ' `CommandText` sous-propriétés, vous pouvez voir la syntaxe SQL réelle qui est envoyée à la base de données lors de l’appel des méthodes Update ou DELETE liées à la couche DAL. Pour le `ProductsOptimisticConcurrency` TableAdapter, l’instruction `DELETE` utilisée est :

[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample3.sql)]

Tandis que l’instruction `DELETE` pour le TableAdapter de produit dans notre couche DAL d’origine est la plus simple :

[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample4.sql)]

Comme vous pouvez le voir, la clause `WHERE` dans l’instruction `DELETE` pour le TableAdapter qui utilise l’accès concurrentiel optimiste comprend une comparaison entre chaque valeur de colonne existante de la table `Product` et les valeurs d’origine au moment du dernier remplissage de GridView (ou DetailsView ou FormView). Étant donné que tous les champs autres que `ProductID`, `ProductName`et `Discontinued` peuvent avoir des valeurs `NULL`, des paramètres et des vérifications supplémentaires sont inclus pour comparer correctement les valeurs de `NULL` dans la clause `WHERE`.

Nous n’ajoutons pas de DataTables supplémentaires au jeu de données d’accès concurrentiel optimiste pour ce didacticiel, car notre page ASP.NET fournit uniquement des informations sur les produits de mise à jour et de suppression. Toutefois, nous devons toujours ajouter la méthode `GetProductByProductID(productID)` au TableAdapter `ProductsOptimisticConcurrency`.

Pour ce faire, cliquez avec le bouton droit sur la barre de titre du TableAdapter (la zone située juste au-dessus du `Fill` et `GetProducts` noms de méthode), puis choisissez Ajouter une requête dans le menu contextuel. Cette opération lance l’Assistant Configuration de requêtes TableAdapter. Comme pour la configuration initiale de votre TableAdapter, choisissez de créer la méthode `GetProductByProductID(productID)` à l’aide d’une instruction SQL ad hoc (voir figure 4). Étant donné que la méthode `GetProductByProductID(productID)` retourne des informations sur un produit particulier, indiquez que cette requête est un `SELECT` type de requête qui retourne des lignes.

[![marquer le type de requête en tant que &quot;SELECT qui retourne des lignes&quot;](implementing-optimistic-concurrency-vb/_static/image26.png)](implementing-optimistic-concurrency-vb/_static/image25.png)

**Figure 9**: marquer le type de requête en tant que «`SELECT` qui retourne des lignes » ([Cliquer pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image27.png))

Dans l’écran suivant, nous sommes invités à entrer la requête SQL à utiliser, avec la requête par défaut du TableAdapter préchargée. Augmentez la requête existante pour inclure la clause `WHERE ProductID = @ProductID`, comme illustré à la figure 10.

[![ajouter une clause WHERE à la requête préchargée pour retourner un enregistrement Product spécifique](implementing-optimistic-concurrency-vb/_static/image29.png)](implementing-optimistic-concurrency-vb/_static/image28.png)

**Figure 10**: ajouter une clause `WHERE` à la requête préchargée pour retourner un enregistrement de produit spécifique ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image30.png))

Enfin, modifiez les noms des méthodes générées pour `FillByProductID` et `GetProductByProductID`.

[![renommez les méthodes FillByProductID et GetProductByProductID](implementing-optimistic-concurrency-vb/_static/image32.png)](implementing-optimistic-concurrency-vb/_static/image31.png)

**Figure 11**: renommer les méthodes pour `FillByProductID` et `GetProductByProductID` ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image33.png))

À l’issue de l’exécution de cet Assistant, le TableAdapter contient maintenant deux méthodes pour récupérer des données : `GetProducts()`, qui retourne *tous les* produits ; et `GetProductByProductID(productID)`, qui retourne le produit spécifié.

## <a name="step-3-creating-a-business-logic-layer-for-the-optimistic-concurrency-enabled-dal"></a>Étape 3 : création d’une couche de logique métier pour la couche DAL avec accès concurrentiel optimiste

Notre classe de `ProductsBLL` existante contient des exemples d’utilisation des modèles de mise à jour par lot et de base de base de donnés. La méthode `AddProduct` et les surcharges de `UpdateProduct` utilisent le modèle de mise à jour par lot, en passant une instance de `ProductRow` à la méthode de mise à jour du TableAdapter. La méthode `DeleteProduct`, en revanche, utilise le modèle de base de base de type direct, en appelant la méthode `Delete(productID)` du TableAdapter.

Avec la nouvelle `ProductsOptimisticConcurrency` TableAdapter, les méthodes de base de base de base de source nécessitent désormais que les valeurs d’origine soient également transmises. Par exemple, la méthode `Delete` attend maintenant dix paramètres d’entrée : le `ProductID`d’origine, `ProductName`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`et `Discontinued`. Elle utilise ces valeurs de paramètres d’entrée supplémentaires dans `WHERE` clause de l’instruction `DELETE` envoyée à la base de données, en supprimant uniquement l’enregistrement spécifié si les valeurs actuelles de la base de données sont mappées à celles d’origine.

Alors que la signature de méthode pour la méthode de `Update` du TableAdapter utilisée dans le modèle de mise à jour par lot n’a pas changé, le code nécessaire pour enregistrer les valeurs d’origine et nouvelles. Par conséquent, au lieu de tenter d’utiliser la couche DAL avec accès concurrentiel optimiste avec notre classe de `ProductsBLL` existante, nous allons créer une nouvelle classe de couche de logique métier pour travailler avec notre nouvelle couche DAL.

Ajoutez une classe nommée `ProductsOptimisticConcurrencyBLL` au dossier `BLL` dans le dossier `App_Code`.

![Ajoutez la classe ProductsOptimisticConcurrencyBLL au dossier BLL](implementing-optimistic-concurrency-vb/_static/image34.png)

**Figure 12**: ajouter la classe `ProductsOptimisticConcurrencyBLL` au dossier BLL

Ensuite, ajoutez le code suivant à la classe `ProductsOptimisticConcurrencyBLL` :

[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample5.vb)]

Notez l’instruction using `NorthwindOptimisticConcurrencyTableAdapters` au-dessus du début de la déclaration de classe. L’espace de noms `NorthwindOptimisticConcurrencyTableAdapters` contient la classe `ProductsOptimisticConcurrencyTableAdapter`, qui fournit les méthodes de la couche DAL. En outre, avant la déclaration de classe, vous trouverez l’attribut `System.ComponentModel.DataObject`, qui indique à Visual Studio d’inclure cette classe dans la liste déroulante de l’Assistant ObjectDataSource.

La propriété `Adapter` de `ProductsOptimisticConcurrencyBLL`fournit un accès rapide à une instance de la classe `ProductsOptimisticConcurrencyTableAdapter` et suit le modèle utilisé dans nos classes BLL d’origine (`ProductsBLL`, `CategoriesBLL`, etc.). Enfin, la méthode `GetProducts()` appelle simplement la méthode `GetProducts()` de la couche DAL et retourne un objet `ProductsOptimisticConcurrencyDataTable` rempli avec une instance `ProductsOptimisticConcurrencyRow` pour chaque enregistrement de produit dans la base de données.

## <a name="deleting-a-product-using-the-db-direct-pattern-with-optimistic-concurrency"></a>Suppression d’un produit à l’aide du modèle de base de données direct avec accès concurrentiel optimiste

Lorsque vous utilisez le modèle de base de données direct sur une couche DAL qui utilise l’accès concurrentiel optimiste, les méthodes doivent être passées aux valeurs nouvelles et d’origine. Pour la suppression, il n’y a aucune nouvelle valeur, donc seules les valeurs d’origine doivent être passées. Dans notre couche BLL, ensuite, nous devons accepter tous les paramètres d’origine comme paramètres d’entrée. Supposons que la méthode `DeleteProduct` dans la classe `ProductsOptimisticConcurrencyBLL` utilise la méthode DB direct. Cela signifie que cette méthode doit prendre les dix champs de données de produit comme paramètres d’entrée et les transmettre à la couche DAL, comme illustré dans le code suivant :

[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample6.vb)]

Si les valeurs d’origine (les valeurs qui ont été chargées dans le contrôle GridView (ou DetailsView ou FormView)) diffèrent des valeurs de la base de données lorsque l’utilisateur clique sur le bouton supprimer, la clause `WHERE` ne correspond à aucun enregistrement de base de données et aucun enregistrement n’est affecté. Par conséquent, la méthode `Delete` du TableAdapter retourne `0` et la méthode `DeleteProduct` de la couche BLL retourne `false`.

## <a name="updating-a-product-using-the-batch-update-pattern-with-optimistic-concurrency"></a>Mise à jour d’un produit à l’aide du modèle de mise à jour par lot avec accès concurrentiel optimiste

Comme indiqué précédemment, la méthode `Update` du TableAdapter pour le modèle de mise à jour par lot a la même signature de méthode que l’accès concurrentiel optimiste soit utilisé ou non. À savoir, la méthode `Update` attend un DataRow, un tableau de DataRows, un DataTable ou un DataSet typé. Il n’existe pas de paramètres d’entrée supplémentaires pour spécifier les valeurs d’origine. Cela est possible car le DataTable effectue le suivi des valeurs d’origine et modifiées pour ses DataRow (s). Lorsque la couche DAL émet son instruction `UPDATE`, les paramètres `@original_ColumnName` sont remplis avec les valeurs d’origine du DataRow, tandis que les paramètres `@ColumnName` sont remplis avec les valeurs modifiées du DataRow.

Dans la classe `ProductsBLL` (qui utilise notre couche DAL d’accès concurrentiel d’origine non optimiste), lorsque vous utilisez le modèle de mise à jour par lot pour mettre à jour les informations sur le produit, notre code exécute la séquence d’événements suivante :

1. Lire les informations sur le produit de la base de données actuelle dans une instance de `ProductRow` à l’aide de la méthode `GetProductByProductID(productID)` du TableAdapter
2. Assignez les nouvelles valeurs à l’instance de `ProductRow` à partir de l’étape 1
3. Appelez la méthode `Update` du TableAdapter, en passant l’instance `ProductRow`

Cette séquence d’étapes, cependant, ne prend pas en charge l’accès concurrentiel optimiste car le `ProductRow` rempli à l’étape 1 est renseigné directement à partir de la base de données, ce qui signifie que les valeurs d’origine utilisées par le DataRow sont celles qui existent actuellement dans la base de données, et non celles qui étaient liées au GridView au début du processus de modification. Au lieu de cela, lors de l’utilisation d’une couche DAL avec accès concurrentiel optimiste, nous devons modifier les surcharges de méthode `UpdateProduct` pour utiliser les étapes suivantes :

1. Lire les informations sur le produit de la base de données actuelle dans une instance de `ProductsOptimisticConcurrencyRow` à l’aide de la méthode `GetProductByProductID(productID)` du TableAdapter
2. Assignez les valeurs *d’origine* à l’instance de `ProductsOptimisticConcurrencyRow` à partir de l’étape 1
3. Appelez la méthode `AcceptChanges()` de l’instance `ProductsOptimisticConcurrencyRow`, qui indique à DataRow que ses valeurs actuelles sont les « d’origine ».
4. Assigner les *nouvelles* valeurs à l’instance `ProductsOptimisticConcurrencyRow`
5. Appelez la méthode `Update` du TableAdapter, en passant l’instance `ProductsOptimisticConcurrencyRow`

L’étape 1 lit toutes les valeurs de la base de données actuelle pour l’enregistrement du produit spécifié. Cette étape est superflue dans la surcharge `UpdateProduct` qui met à jour *toutes* les colonnes de produit (puisque ces valeurs sont remplacées à l’étape 2), mais est essentielle pour ces surcharges où seul un sous-ensemble des valeurs de colonne est transmis en tant que paramètres d’entrée. Une fois que les valeurs d’origine ont été assignées à l’instance `ProductsOptimisticConcurrencyRow`, la méthode `AcceptChanges()` est appelée, ce qui marque les valeurs DataRow actuelles comme valeurs d’origine à utiliser dans les paramètres de `@original_ColumnName` de l’instruction `UPDATE`. Ensuite, les nouvelles valeurs de paramètre sont assignées au `ProductsOptimisticConcurrencyRow` et, enfin, la méthode `Update` est appelée, passant le DataRow.

Le code suivant illustre la surcharge `UpdateProduct` qui accepte tous les champs de données de produit comme paramètres d’entrée. Bien que cela ne soit pas illustré ici, la classe `ProductsOptimisticConcurrencyBLL` incluse dans le téléchargement de ce didacticiel contient également une surcharge `UpdateProduct` qui accepte uniquement le nom et le prix du produit en tant que paramètres d’entrée.

[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample7.vb)]

## <a name="step-4-passing-the-original-and-new-values-from-the-aspnet-page-to-the-bll-methods"></a>Étape 4 : passage des valeurs d’origine et nouvelles à partir de la page ASP.NET aux méthodes BLL

Une fois la couche DAL et la couche BLL terminées, il ne reste plus qu’à créer une page ASP.NET qui peut utiliser la logique d’accès concurrentiel optimiste intégrée au système. Plus précisément, le contrôle Web de données (GridView, DetailsView ou FormView) doit mémoriser ses valeurs d’origine et ObjectDataSource doit passer les deux ensembles de valeurs à la couche de logique métier. En outre, la page ASP.NET doit être configurée pour gérer correctement les violations d’accès concurrentiel.

Commencez par ouvrir la page `OptimisticConcurrency.aspx` dans le dossier `EditInsertDelete` et ajoutez un GridView au concepteur, en affectant à sa propriété `ID` la valeur `ProductsGrid`. À partir de la balise active de GridView, choisissez de créer un nouvel ObjectDataSource nommé `ProductsOptimisticConcurrencyDataSource`. Comme nous voulons que cet ObjectDataSource utilise la couche DAL qui prend en charge l’accès concurrentiel optimiste, configurez-le pour qu’il utilise l’objet `ProductsOptimisticConcurrencyBLL`.

[![que ObjectDataSource utilise l’objet ProductsOptimisticConcurrencyBLL](implementing-optimistic-concurrency-vb/_static/image36.png)](implementing-optimistic-concurrency-vb/_static/image35.png)

**Figure 13**: demander à ObjectDataSource d’utiliser l’objet `ProductsOptimisticConcurrencyBLL` ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image37.png))

Choisissez les méthodes `GetProducts`, `UpdateProduct`et `DeleteProduct` dans les listes déroulantes de l’Assistant. Pour la méthode UpdateProduct, utilisez la surcharge qui accepte tous les champs de données du produit.

## <a name="configuring-the-objectdatasource-controls-properties"></a>Configuration des propriétés du contrôle ObjectDataSource

Une fois l’exécution de l’Assistant terminée, le balisage déclaratif de ObjectDataSource doit ressembler à ce qui suit :

[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample8.aspx)]

Comme vous pouvez le voir, la collection `DeleteParameters` contient une instance `Parameter` pour chacun des dix paramètres d’entrée de la méthode `DeleteProduct` de la classe `ProductsOptimisticConcurrencyBLL`. De même, la collection `UpdateParameters` contient une instance `Parameter` pour chacun des paramètres d’entrée dans `UpdateProduct`.

Pour les didacticiels précédents qui impliquaient la modification des données, nous supprimons la propriété ObjectDataSource `OldValuesParameterFormatString` à ce stade, car cette propriété indique que la méthode BLL attend que les anciennes valeurs (ou d’origine) soient passées, ainsi que les nouvelles valeurs. En outre, cette valeur de propriété indique les noms des paramètres d’entrée pour les valeurs d’origine. Étant donné que nous transmettons les valeurs d’origine dans la couche BLL, ne supprimez *pas* cette propriété.

> [!NOTE]
> La valeur de la propriété `OldValuesParameterFormatString` doit correspondre aux noms de paramètres d’entrée dans la couche BLL qui attendent les valeurs d’origine. Étant donné que nous avons nommé ces paramètres `original_productName`, `original_supplierID`, etc., vous pouvez conserver la valeur de la propriété `OldValuesParameterFormatString` en tant que `original_{0}`. Toutefois, si les paramètres d’entrée de la méthode BLL comportent des noms comme `old_productName`, `old_supplierID`, etc., vous devez mettre à jour la propriété `OldValuesParameterFormatString` pour `old_{0}`.

Il existe un paramètre de propriété final qui doit être défini pour que ObjectDataSource passe correctement les valeurs d’origine aux méthodes BLL. ObjectDataSource a une [propriété ConflictDetection](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.conflictdetection.aspx) qui peut être assignée à [l’une des deux valeurs](https://msdn.microsoft.com/library/system.web.ui.conflictoptions.aspx)suivantes :

- `OverwriteChanges`-la valeur par défaut ; n’envoie pas les valeurs d’origine aux paramètres d’entrée d’origine des méthodes BLL
- `CompareAllValues`-envoie les valeurs d’origine aux méthodes BLL ; Choisissez cette option lors de l’utilisation de l’accès concurrentiel optimiste

Prenez un moment pour définir la propriété `ConflictDetection` sur `CompareAllValues`.

## <a name="configuring-the-gridviews-properties-and-fields"></a>Configuration des propriétés et des champs de GridView

Avec les propriétés ObjectDataSource correctement configurées, nous allons nous pencher sur la configuration du contrôle GridView. Tout d’abord, puisque nous voulons que le GridView prenne en charge la modification et la suppression, activez les cases à cocher Activer la modification et activer la suppression à partir de la balise active de GridView. Cette opération ajoute un CommandField dont les `ShowEditButton` et `ShowDeleteButton` ont tous les deux la valeur `true`.

Lorsqu’il est lié au `ProductsOptimisticConcurrencyDataSource` ObjectDataSource, le GridView contient un champ pour chacun des champs de données du produit. Bien qu’un tel GridView puisse être modifié, l’expérience utilisateur est tout à fait acceptable. Le `CategoryID` et `SupplierID` BoundFields s’affichent sous forme de zones de texte, ce qui oblige l’utilisateur à entrer la catégorie et le fournisseur appropriés comme numéros d’identification. Il n’y aura aucune mise en forme pour les champs numériques et aucun contrôle de validation pour vous assurer que le nom du produit a été fourni et que le prix unitaire, les unités en stock, les unités sur la commande et les valeurs de niveau de réorganisation sont des valeurs numériques correctes et sont supérieures ou égales à zéro.

Comme nous l’avons vu dans l’article *Ajout de contrôles de validation aux interfaces de modification et d’insertion* et personnalisation des didacticiels de l' *interface de modification des données* , vous pouvez personnaliser l’interface utilisateur en remplaçant BoundFields par TemplateFields. J’ai modifié ce GridView et son interface de modification des manières suivantes :

- Suppression des `ProductID`, `SupplierName`et `CategoryName` BoundFields
- Converti le `ProductName` BoundField en TemplateField et ajouté un contrôle RequiredFieldValidation.
- Converti le `CategoryID` et `SupplierID` BoundFields en TemplateFields, et ajusté l’interface de modification pour utiliser DropDownList plutôt que des zones de texte. Dans ces TemplateFields' `ItemTemplates`, les champs de données `CategoryName` et `SupplierName` sont affichés.
- Conversion du `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`et `ReorderLevel` BoundFields en TemplateFields et ajout de contrôles CompareValidator.

Étant donné que nous avons déjà examiné comment accomplir ces tâches dans les didacticiels précédents, je vais simplement répertorier la syntaxe déclarative finale ici et quitter l’implémentation en tant que pratique.

[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample9.aspx)]

Nous sommes très proches de l’utilisation d’un exemple complet. Toutefois, il existe quelques subtilités qui vont se produire et provoquer des problèmes. En outre, nous avons encore besoin d’une interface qui avertit l’utilisateur lorsqu’une violation d’accès concurrentiel s’est produite.

> [!NOTE]
> Pour qu’un contrôle Web de données transmette correctement les valeurs d’origine à l’ObjectDataSource (qui est ensuite transmis à la couche BLL), il est vital que la propriété `EnableViewState` de GridView soit définie sur `true` (valeur par défaut). Si vous désactivez l’état d’affichage, les valeurs d’origine sont perdues lors de la publication.

## <a name="passing-the-correct-original-values-to-the-objectdatasource"></a>Passage des valeurs d’origine correctes à ObjectDataSource

Il existe quelques problèmes liés à la façon dont le GridView a été configuré. Si la propriété de `ConflictDetection` ObjectDataSource est définie sur `CompareAllValues` (comme c’est le cas de la nôtre), lorsque les méthodes `Update()` ou `Delete()` de l’ObjectDataSource sont appelées par le GridView (ou DetailsView ou FormView), l’ObjectDataSource tente de copier les valeurs d’origine de GridView dans ses instances de `Parameter` appropriées. Reportez-vous à la figure 2 pour obtenir une représentation graphique de ce processus.

Plus précisément, les valeurs d’origine du contrôle GridView sont affectées aux valeurs dans les instructions de liaison de données bidirectionnelle chaque fois que les données sont liées au GridView. Par conséquent, il est essentiel que les valeurs d’origine requises soient capturées par le biais de la liaison de liaison bidirectionnelle et qu’elles soient fournies dans un format convertible.

Pour comprendre pourquoi cela est important, prenez un moment pour visiter notre page dans un navigateur. Comme prévu, le contrôle GridView répertorie chaque produit avec un bouton modifier et supprimer dans la colonne la plus à gauche.

[![les produits sont répertoriés dans un GridView](implementing-optimistic-concurrency-vb/_static/image39.png)](implementing-optimistic-concurrency-vb/_static/image38.png)

**Figure 14**: les produits sont répertoriés dans un GridView ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image40.png))

Si vous cliquez sur le bouton Supprimer pour un produit, une `FormatException` est levée.

[![tentant de supprimer des résultats de produit dans un FormatException](implementing-optimistic-concurrency-vb/_static/image42.png)](implementing-optimistic-concurrency-vb/_static/image41.png)

**Figure 15**: tentative de suppression des résultats d’un produit dans un `FormatException` ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image43.png))

La `FormatException` est déclenchée lorsque ObjectDataSource tente de lire la valeur de `UnitPrice` d’origine. Étant donné que la `ItemTemplate` a la `UnitPrice` mise en forme en tant que devise (`<%# Bind("UnitPrice", "{0:C}") %>`), elle inclut un symbole monétaire, comme $19,95. Le `FormatException` se produit lorsque ObjectDataSource tente de convertir cette chaîne en `decimal`. Pour contourner ce problème, nous disposons d’un certain nombre d’options :

- Supprimez la mise en forme monétaire de la `ItemTemplate`. Autrement dit, au lieu d’utiliser `<%# Bind("UnitPrice", "{0:C}") %>`, utilisez simplement `<%# Bind("UnitPrice") %>`. L’inconvénient est que le prix n’est plus formaté.
- Affichez la `UnitPrice` mise en forme en tant que devise dans le `ItemTemplate`, mais utilisez le mot clé `Eval` pour effectuer cette opération. Rappelez-vous que `Eval` effectue une liaison de liaison unidirectionnelle. Nous devons toujours fournir la valeur `UnitPrice` pour les valeurs d’origine. nous avons donc toujours besoin d’une instruction de liaison de liaison bidirectionnelle dans le `ItemTemplate`, mais cela peut être placé dans un contrôle Web étiquette dont la propriété `Visible` est définie sur `false`. Nous pourrions utiliser le balisage suivant dans le ItemTemplate :

[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample10.aspx)]

- Supprimez la mise en forme monétaire de la `ItemTemplate`à l’aide de `<%# Bind("UnitPrice") %>`. Dans le gestionnaire d’événements `RowDataBound` du GridView, accédez par programmation au contrôle Web Label dans lequel la valeur `UnitPrice` est affichée et définissez sa propriété `Text` sur la version mise en forme.
- Laissez le `UnitPrice` mis en forme en tant que devise. Dans le gestionnaire d’événements `RowDeleting` du GridView, remplacez la valeur de `UnitPrice` d’origine existante ($19,95) par une valeur décimale réelle à l’aide de `Decimal.Parse`. Nous avons vu comment accomplir une opération similaire dans le gestionnaire d’événements `RowUpdating` dans la [*gestion des exceptions de niveau BLL et dal dans un didacticiel de Page ASP.net*](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) .

Pour mon exemple, j’ai choisi de passer à la deuxième approche, en ajoutant un contrôle Web d’étiquette masqué dont la propriété `Text` est bidirectionnelle liée à la valeur de `UnitPrice` non mise en forme.

Après avoir résolu ce problème, essayez de cliquer à nouveau sur le bouton Supprimer pour tous les produits. Cette fois, vous obtiendrez un `InvalidOperationException` lorsque ObjectDataSource tente d’appeler la méthode de `UpdateProduct` de la couche BLL.

[![ObjectDataSource ne peut pas trouver de méthode avec les paramètres d’entrée qu’il souhaite envoyer](implementing-optimistic-concurrency-vb/_static/image45.png)](implementing-optimistic-concurrency-vb/_static/image44.png)

**Figure 16**: ObjectDataSource ne peut pas trouver de méthode avec les paramètres d’entrée qu’il souhaite envoyer ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image46.png))

En examinant le message de l’exception, il est clair que ObjectDataSource souhaite appeler une méthode BLL `DeleteProduct` qui comprend `original_CategoryName` et `original_SupplierName` paramètres d’entrée. Cela est dû au fait que les `ItemTemplate` s pour les `CategoryID` et les `SupplierID` TemplateFields contiennent actuellement des instructions de liaison bidirectionnelle avec les champs de données `CategoryName` et `SupplierName`. Au lieu de cela, nous devons inclure des instructions `Bind` avec les champs de données `CategoryID` et `SupplierID`. Pour ce faire, remplacez les instructions de liaison existantes par `Eval` instructions, puis ajoutez des contrôles d’étiquette masquée dont les propriétés de `Text` sont liées aux champs de données `CategoryID` et `SupplierID` à l’aide de la liaison de données bidirectionnelle, comme indiqué ci-dessous :

[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample11.aspx)]

Avec ces modifications, nous sommes maintenant en mesure de supprimer et de modifier correctement les informations du produit. À l’étape 5, nous verrons comment vérifier que des violations d’accès concurrentiel sont détectées. Mais pour le moment, prenez quelques minutes pour essayer de mettre à jour et de supprimer quelques enregistrements pour vous assurer que la mise à jour et la suppression d’un seul utilisateur fonctionne comme prévu.

## <a name="step-5-testing-the-optimistic-concurrency-support"></a>Étape 5 : test de la prise en charge de l’accès concurrentiel optimiste

Pour vérifier que les violations d’accès concurrentiel sont détectées (au lieu de provoquer le remplacement aveugle des données), nous devons ouvrir deux fenêtres de navigateur sur cette page. Dans les deux instances du navigateur, cliquez sur le bouton modifier pour Chai. Ensuite, dans un seul des navigateurs, remplacez le nom par « Chai thé », puis cliquez sur mettre à jour. La mise à jour doit s’effectuer correctement et retourner le GridView à son état antérieur à la modification, avec « Chai thé » comme nouveau nom de produit.

Toutefois, dans l’autre instance de la fenêtre du navigateur, la zone de texte Product Name affiche toujours « Chai ». Dans cette deuxième fenêtre de navigateur, mettez à jour le `UnitPrice` sur `25.00`. Sans prise en charge de l’accès concurrentiel optimiste, le fait de cliquer sur mettre à jour dans la deuxième instance du navigateur permet de rétablir le nom du produit sur « Chai », remplaçant ainsi les modifications apportées par la première instance du navigateur. Avec l’accès concurrentiel optimiste utilisé, toutefois, si vous cliquez sur le bouton mettre à jour dans la deuxième instance du navigateur, un [DBConcurrencyException](https://msdn.microsoft.com/library/system.data.dbconcurrencyexception.aspx)est obtenu.

[![lorsqu’une violation d’accès concurrentiel est détectée, un DBConcurrencyException est levé](implementing-optimistic-concurrency-vb/_static/image48.png)](implementing-optimistic-concurrency-vb/_static/image47.png)

**Figure 17**: quand une violation d’accès concurrentiel est détectée, une `DBConcurrencyException` est levée ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image49.png))

La `DBConcurrencyException` est levée uniquement lorsque le modèle de mise à jour par lot de la couche DAL est utilisé. Le modèle DB direct ne lève pas d’exception, il indique simplement qu’aucune ligne n’a été affectée. Pour illustrer cela, renvoyez le contrôle GridView des instances de navigateur à leur état antérieur à la modification. Ensuite, dans la première instance du navigateur, cliquez sur le bouton modifier et remplacez le nom du produit « Chai thé » par « Chai », puis cliquez sur mettre à jour. Dans la deuxième fenêtre du navigateur, cliquez sur le bouton Supprimer pour Chai.

Lorsque vous cliquez sur supprimer, la page est republiée, le GridView appelle la méthode d' `Delete()` ObjectDataSource et l’ObjectDataSource appelle la méthode de `DeleteProduct` de la classe `ProductsOptimisticConcurrencyBLL`, en passant les valeurs d’origine. La valeur de `ProductName` d’origine pour la deuxième instance de navigateur est « Chai Tea », qui ne correspond pas à la valeur de `ProductName` actuelle dans la base de données. Par conséquent, l’instruction `DELETE` envoyée à la base de données n’affecte aucune ligne, car il n’y a pas d’enregistrement dans la base de données satisfait par la clause `WHERE`. La méthode `DeleteProduct` retourne `false` et les données de l’ObjectDataSource sont reliées au GridView.

Du point de vue de l’utilisateur final, le fait de cliquer sur le bouton Supprimer pour le thé chai dans la deuxième fenêtre du navigateur a entraîné le clignotement de l’écran et, à la sortie, le produit est toujours là, bien qu’il soit maintenant listé comme « Chai » (le nom du produit est modifié par le premier navigateur instance). Si l’utilisateur clique à nouveau sur le bouton supprimer, la suppression est effectuée, car la valeur de `ProductName` d’origine du GridView (« Chai ») correspond maintenant à la valeur de la base de données.

Dans ces deux cas, l’expérience utilisateur est loin d’être idéale. Nous ne voulons pas montrer clairement à l’utilisateur les détails de la `DBConcurrencyException` exception lors de l’utilisation du modèle de mise à jour par lot. Le comportement lors de l’utilisation du modèle de base de base de type direct est quelque peu confus lorsque la commande des utilisateurs a échoué, mais il n’existait aucune indication précise de pourquoi.

Pour résoudre ces deux problèmes, nous pouvons créer des contrôles Label Web sur la page qui fournissent une explication de la raison de l’échec d’une mise à jour ou d’une suppression. Pour le modèle de mise à jour par lot, nous pouvons déterminer si une `DBConcurrencyException` exception s’est produite dans le gestionnaire d’événements de la publication du contrôle GridView, en affichant l’étiquette d’avertissement si nécessaire. Pour la méthode DB direct, nous pouvons examiner la valeur de retour de la méthode BLL (qui est `true` si une ligne a été affectée, `false` dans le cas contraire) et afficher un message d’information en fonction des besoins.

## <a name="step-6-adding-informational-messages-and-displaying-them-in-the-face-of-a-concurrency-violation"></a>Étape 6 : ajouter des messages d’information et les afficher en cas de violation de l’accès concurrentiel

En cas de violation de l’accès concurrentiel, le comportement présenté varie selon que la mise à jour par lot ou le modèle de base de données de la couche DAL a été utilisé. Notre didacticiel utilise les deux modèles, le modèle de mise à jour par lot étant utilisé pour la mise à jour et le modèle de base de type direct utilisé pour la suppression. Pour commencer, nous allons ajouter deux contrôles Label Web à notre page qui expliquent qu’une violation d’accès concurrentiel s’est produite lors de la tentative de suppression ou de mise à jour des données. Définissez les propriétés `Visible` et `EnableViewState` du contrôle Label sur `false`; Cela entraîne le masquage de ces pages à chaque visite de page, à l’exception des visites de page dans lesquelles leur `Visible` propriété est définie par programmation sur `true`.

[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample12.aspx)]

En plus de définir leurs propriétés `Visible`, `EnabledViewState`et `Text`, j’ai également défini la propriété `CssClass` sur `Warning`, ce qui entraîne l’affichage de l’étiquette dans une police large, rouge, italique et gras. Cette classe de `Warning` CSS a été définie et ajoutée à styles. CSS dans le didacticiel *examen des événements associés à l’insertion, à la mise à jour et à la suppression* .

Après avoir ajouté ces étiquettes, le concepteur dans Visual Studio doit ressembler à la figure 18.

[![deux contrôles Label ont été ajoutés à la page](implementing-optimistic-concurrency-vb/_static/image51.png)](implementing-optimistic-concurrency-vb/_static/image50.png)

**Figure 18**: deux contrôles Label ont été ajoutés à la page ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image52.png))

Avec ces contrôles Web d’étiquette en place, nous sommes prêts à examiner comment déterminer quand une violation d’accès concurrentiel s’est produite. à ce stade, la propriété `Visible` de l’étiquette appropriée peut être définie sur `true`, ce qui affiche le message d’information.

## <a name="handling-concurrency-violations-when-updating"></a>Gestion des violations d’accès concurrentiel lors de la mise à jour

Voyons tout d’abord comment gérer les violations d’accès concurrentiel lors de l’utilisation du modèle de mise à jour par lot. Étant donné que ces violations avec le modèle de mise à jour par lot entraînent la levée d’une exception `DBConcurrencyException`, nous devons ajouter du code à notre page ASP.NET pour déterminer si une exception `DBConcurrencyException` s’est produite pendant le processus de mise à jour. Dans ce cas, nous devons afficher un message à l’utilisateur expliquant que les modifications n’ont pas été enregistrées, car un autre utilisateur a modifié les mêmes données entre le moment où il a commencé à modifier l’enregistrement et le moment où il a cliqué sur le bouton mettre à jour.

Comme nous l’avons vu dans la *gestion des exceptions de niveau BLL et dal dans un didacticiel de Page ASP.net* , de telles exceptions peuvent être détectées et supprimées dans les gestionnaires d’événements de l’événement de publication du contrôle Web des données. Par conséquent, nous devons créer un gestionnaire d’événements pour l’événement `RowUpdated` de GridView qui vérifie si une exception `DBConcurrencyException` a été levée. Ce gestionnaire d’événements reçoit une référence à toute exception levée pendant le processus de mise à jour, comme indiqué dans le code du gestionnaire d’événements ci-dessous :

[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample13.vb)]

Face à une exception `DBConcurrencyException`, ce gestionnaire d’événements affiche le contrôle d’étiquette `UpdateConflictMessage` et indique que l’exception a été gérée. Avec ce code en place, lorsqu’une violation d’accès concurrentiel se produit lors de la mise à jour d’un enregistrement, les modifications de l’utilisateur sont perdues, car elles auraient remplacé les modifications d’un autre utilisateur en même temps. En particulier, le GridView est retourné à son état antérieur à la modification et lié aux données de la base de données actuelle. Cette opération met à jour la ligne GridView avec les modifications de l’autre utilisateur, qui n’étaient pas visibles auparavant. En outre, le contrôle d’étiquette `UpdateConflictMessage` explique à l’utilisateur ce qui s’est produit. Cette séquence d’événements est détaillée dans la figure 19.

[![les mises à jour des utilisateurs sont perdues en cas de violation de l’accès concurrentiel](implementing-optimistic-concurrency-vb/_static/image54.png)](implementing-optimistic-concurrency-vb/_static/image53.png)

**Figure 19**: les mises à jour des utilisateurs sont perdues en cas de violation d’accès concurrentiel ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image55.png))

> [!NOTE]
> Au lieu de renvoyer le contrôle GridView à l’état antérieur à la modification, nous pourrions conserver le GridView dans son état de modification en affectant à la propriété `KeepInEditMode` de l’objet `GridViewUpdatedEventArgs` passé la valeur true. Toutefois, si vous adoptez cette approche, veillez à relier les données au contrôle GridView (en appelant sa méthode `DataBind()`) afin que les valeurs de l’autre utilisateur soient chargées dans l’interface de modification. Le code disponible en téléchargement dans ce didacticiel contient ces deux lignes de code dans le `RowUpdated` gestionnaire d’événements commenté ; supprimez simplement les marques de commentaire de ces lignes de code pour que le contrôle GridView reste en mode édition après une violation d’accès concurrentiel.

## <a name="responding-to-concurrency-violations-when-deleting"></a>Réponse aux violations d’accès concurrentiel lors de la suppression

Avec le modèle de base de données direct, aucune exception n’est levée en cas de violation d’accès concurrentiel. Au lieu de cela, l’instruction de base de données n’affecte simplement aucun enregistrement, car la clause WHERE ne correspond à aucun enregistrement. Toutes les méthodes de modification des données créées dans la couche BLL ont été conçues de telle sorte qu’elles retournent une valeur booléenne indiquant si elles ont été affectées avec précision un enregistrement. Par conséquent, pour déterminer si une violation d’accès concurrentiel s’est produite lors de la suppression d’un enregistrement, nous pouvons examiner la valeur de retour de la méthode `DeleteProduct` de la couche BLL.

La valeur de retour d’une méthode BLL peut être examinée dans les gestionnaires d’événements de démarrage de l’ObjectDataSource via la propriété `ReturnValue` de l’objet `ObjectDataSourceStatusEventArgs` passé dans le gestionnaire d’événements. Étant donné que nous nous intéressons à déterminer la valeur de retour de la méthode `DeleteProduct`, nous devons créer un gestionnaire d’événements pour l’événement de `Deleted` de l’ObjectDataSource. La propriété `ReturnValue` est de type `object` et peut être `null` si une exception a été levée et si la méthode a été interrompue avant de pouvoir retourner une valeur. Par conséquent, nous devons tout d’abord vous assurer que la propriété `ReturnValue` n’est pas `null` et qu’il s’agit d’une valeur booléenne. En supposant que cette vérification réussit, nous affichons le contrôle d’étiquette `DeleteConflictMessage` si le `ReturnValue` est `false`. Pour ce faire, vous pouvez utiliser le code suivant :

[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample14.vb)]

Face à une violation d’accès concurrentiel, la demande de suppression de l’utilisateur est annulée. Le GridView est actualisé et présente les modifications qui se sont produites pour cet enregistrement entre le moment où l’utilisateur a chargé la page et le moment où il a cliqué sur le bouton supprimer. Quand une violation de ce type est dépassée, le `DeleteConflictMessage` étiquette est indiqué, expliquant ce qui vient de se passer (voir figure 20).

[![la suppression d’un utilisateur est annulée en cas de violation d’accès concurrentiel](implementing-optimistic-concurrency-vb/_static/image57.png)](implementing-optimistic-concurrency-vb/_static/image56.png)

**Figure 20**: une suppression de l’utilisateur est annulée en cas de violation d’accès concurrentiel ([cliquez pour afficher l’image en taille réelle](implementing-optimistic-concurrency-vb/_static/image58.png))

## <a name="summary"></a>Récapitulatif

Les opportunités de violation de l’accès concurrentiel existent dans toutes les applications qui permettent à plusieurs utilisateurs simultanés de mettre à jour ou de supprimer des données. Si de telles violations ne sont pas comptabilisées, lorsque deux utilisateurs mettent à jour simultanément les mêmes données qui se trouvent dans la dernière écriture « WINS », le remplacement des modifications apportées par l’autre utilisateur. Les développeurs peuvent également implémenter un contrôle d’accès concurrentiel optimiste ou pessimiste. Le contrôle de l’accès concurrentiel optimiste suppose que les violations d’accès concurrentiel ne sont pas fréquentes et n’autorisent simplement pas une commande Update ou DELETE qui constituerait une violation d’accès concurrentiel. Le contrôle d’accès concurrentiel pessimiste suppose que les violations d’accès concurrentiel sont fréquentes et qu’il suffit de rejeter la commande de mise à jour ou de suppression d’un utilisateur n’est pas acceptable. Avec le contrôle d’accès concurrentiel pessimiste, la mise à jour d’un enregistrement implique son verrouillage, ce qui empêche les autres utilisateurs de modifier ou de supprimer l’enregistrement pendant qu’il est verrouillé.

Le DataSet typé dans .NET fournit des fonctionnalités pour prendre en charge le contrôle d’accès concurrentiel optimiste. En particulier, les instructions `UPDATE` et `DELETE` émises dans la base de données incluent toutes les colonnes de la table, garantissant ainsi que la mise à jour ou la suppression se produira uniquement si les données actuelles de l’enregistrement correspondent aux données d’origine de l’utilisateur lors de l’exécution de la mise à jour ou de la suppression. Une fois la couche DAL configurée pour prendre en charge l’accès concurrentiel optimiste, les méthodes BLL doivent être mises à jour. En outre, la page ASP.NET qui appelle la couche BLL doit être configurée de sorte que l’ObjectDataSource récupère les valeurs d’origine à partir de son contrôle Web de données et les transmet dans la couche BLL.

Comme nous l’avons vu dans ce didacticiel, l’implémentation du contrôle d’accès concurrentiel optimiste dans une application Web ASP.NET implique la mise à jour de la couche DAL et de la couche BLL et l’ajout de la prise en charge dans la page ASP.NET. Le fait que ce travail ajouté soit un investissement judicieux de votre temps et de vos efforts dépend de votre application. Si vous avez rarement des utilisateurs simultanés qui mettent à jour des données ou si les données qu’ils mettent à jour diffèrent les unes des autres, le contrôle d’accès concurrentiel n’est pas un problème clé. Toutefois, si vous avez régulièrement plusieurs utilisateurs sur votre site qui utilisent les mêmes données, le contrôle d’accès concurrentiel peut contribuer à empêcher les mises à jour ou les suppressions d’un utilisateur de remplacer involontairement un autre.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Précédent](customizing-the-data-modification-interface-vb.md)
> [Suivant](adding-client-side-confirmation-when-deleting-vb.md)
