---
uid: web-forms/overview/data-access/introduction/creating-a-data-access-layer-vb
title: Création d’une couche d’accès aux données (VB) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons commencer par le tout début et créer la couche d’accès aux données (DAL), à l’aide de DataSets typés, pour accéder aux informations d’une base de données.
ms.author: riande
ms.date: 04/05/2010
ms.assetid: 6227233a-6254-4b6b-9a89-947efef22330
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-data-access-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: 51c9255f80f83a68cf26decf318347752498491a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74635138"
---
# <a name="creating-a-data-access-layer-vb"></a>Création d’une couche d’accès aux données (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_1_VB.exe) ou [Télécharger le PDF](creating-a-data-access-layer-vb/_static/datatutorial01vb1.pdf)

> Dans ce didacticiel, nous allons commencer par le tout début et créer la couche d’accès aux données (DAL), à l’aide de DataSets typés, pour accéder aux informations d’une base de données.

## <a name="introduction"></a>Introduction

En tant que développeurs Web, notre vie repose sur l’utilisation des données. Nous créons des bases de données pour stocker les données, le code pour les récupérer et les modifier, et les pages Web pour les collecter et les résumer. Il s’agit du premier didacticiel d’une série longue qui explore les techniques permettant d’implémenter ces modèles courants dans ASP.NET 2,0. Nous allons commencer par créer une [architecture logicielle](http://en.wikipedia.org/wiki/Software_architecture) composée d’une couche d’accès aux données (DAL) à l’aide de datasets typés, d’une couche de logique métier (BLL) qui applique des règles métier personnalisées et d’une couche de présentation composée de pages ASP.net qui partagent une disposition de page commune. Une fois ce serveur principal posé, nous passerons à la création de rapports, en indiquant comment afficher, synthétiser, collecter et valider les données à partir d’une application Web. Ces didacticiels sont destinés à être concis et fournissent des instructions pas à pas avec de nombreuses captures d’écran pour vous guider tout au long du processus. Chaque Didacticiel est disponible dans C# et Visual Basic versions et comprend un téléchargement du code complet utilisé. (Ce premier didacticiel est assez long, mais le reste est présenté dans des blocs beaucoup plus digestibles.)

Pour ces didacticiels, nous allons utiliser une version Microsoft SQL Server 2005 Express Edition de la base de données Northwind placée dans le répertoire `App_Data`. En plus du fichier de base de données, le dossier `App_Data` contient également les scripts SQL pour la création de la base de données, au cas où vous souhaiteriez utiliser une version de base de données différente. Vous pouvez également télécharger ces scripts [directement à partir de Microsoft](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en), si vous préférez. Si vous utilisez une version de SQL Server différente de la base de données Northwind, vous devez mettre à jour le paramètre `NORTHWNDConnectionString` dans le fichier `Web.config` de l’application. L’application Web a été créée à l’aide de Visual Studio 2005 Professional Edition comme un projet de site Web basé sur un système de fichiers. Toutefois, tous les didacticiels fonctionnent aussi bien avec la version gratuite de Visual Studio 2005, [Visual Web Developer](https://msdn.microsoft.com/vstudio/express/vwd/).

Dans ce didacticiel, nous allons commencer par le tout début et créer la couche d’accès aux données (DAL), puis créer la [couche de logique métier (BLL)](creating-a-business-logic-layer-vb.md) dans le deuxième didacticiel et travailler sur la [mise en page et la navigation](master-pages-and-site-navigation-vb.md) dans la troisième. Les didacticiels qui suivent le troisième s’appuient sur les fondations posées dans les trois premières. Nous avons beaucoup à aborder dans ce premier didacticiel, donc à démarrer Visual Studio et à commencer !

## <a name="step-1-creating-a-web-project-and-connecting-to-the-database"></a>Étape 1 : création d’un projet Web et connexion à la base de données

Avant de pouvoir créer notre couche d’accès aux données (DAL), nous devons tout d’abord créer un site Web et configurer notre base de données. Commencez par créer un site Web ASP.NET basé sur un système de fichiers. Pour ce faire, accédez au menu fichier et sélectionnez Nouveau site Web, en affichant la boîte de dialogue Nouveau site Web. Choisissez le modèle de site Web ASP.NET, définissez la liste déroulante emplacement sur système de fichiers, choisissez un dossier pour placer le site Web et définissez la langue sur Visual Basic.

[![créer un site Web basé sur un système de fichiers](creating-a-data-access-layer-vb/_static/image2.png)](creating-a-data-access-layer-vb/_static/image1.png)

**Figure 1**: créer un site Web basé sur un système de fichiers ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image3.png))

Cela permet de créer un nouveau site Web avec une `Default.aspx` page ASP.NET, un dossier `App_Data` et un fichier `Web.config`.

Une fois le site Web créé, l’étape suivante consiste à ajouter une référence à la base de données dans le Explorateur de serveurs de Visual Studio. En ajoutant une base de données au Explorateur de serveurs vous pouvez ajouter des tables, des procédures stockées, des vues, et ainsi de suite dans Visual Studio. Vous pouvez également afficher des données de table ou créer vos propres requêtes à la main ou graphiquement par le biais de l’Générateur de requêtes. En outre, lorsque nous créons les DataSets typés pour la couche DAL, nous devrons pointer Visual Studio vers la base de données à partir de laquelle les DataSets typés doivent être construits. Nous pouvons fournir ces informations de connexion à ce moment-là, mais Visual Studio remplit automatiquement une liste déroulante des bases de données déjà inscrites dans le Explorateur de serveurs.

Les étapes d’ajout de la base de données Northwind au Explorateur de serveurs varient selon que vous souhaitez utiliser la base de données SQL Server 2005 Express Edition dans le dossier `App_Data` ou si vous souhaitez utiliser à la place une installation du serveur de base de données Microsoft SQL Server 2000 ou 2005.

## <a name="using-a-database-in-theapp_datafolder"></a>Utilisation d’une base de données dans le dossier`App_Data`

Si vous ne disposez pas d’un serveur de base de données SQL Server 2000 ou 2005 pour vous connecter à, ou si vous souhaitez simplement éviter d’avoir à ajouter la base de données à un serveur de base de données, vous pouvez utiliser la version SQL Server 2005 Express Edition de la base de données Northwind qui se trouve dans le dossier d' `App_Data` du site Web téléchargé (`NORTHWND.MDF`).

Une base de données placée dans le dossier `App_Data` est automatiquement ajoutée au Explorateur de serveurs. En supposant que vous avez SQL Server 2005 Express Edition installé sur votre ordinateur, vous devez voir un nœud nommé fichier NORTHWND. MDF dans le Explorateur de serveurs, qui vous permet de développer et d’explorer ses tables, vues, procédures stockées, etc. (voir figure 2).

Le dossier `App_Data` peut également contenir des fichiers `.mdb` Microsoft Access, qui, comme leurs SQL Server équivalents, sont automatiquement ajoutés au Explorateur de serveurs. Si vous ne souhaitez pas utiliser l’une des options de SQL Server, vous pouvez toujours [Télécharger une version Microsoft Access du fichier de base de données Northwind](https://www.microsoft.com/downloads/details.aspx?FamilyID=C6661372-8DBE-422B-8676-C632D66C529C&amp;displaylang=EN) et la placer dans le répertoire `App_Data`. Gardez toutefois à l’esprit que les bases de données Access ne sont pas aussi riches en fonctionnalités que SQL Server et ne sont pas conçues pour être utilisées dans les scénarios de site Web. En outre, quelques-uns des 35 didacticiels utiliseront certaines fonctionnalités de niveau base de données qui ne sont pas prises en charge par Access.

## <a name="connecting-to-the-database-in-a-microsoft-sql-server-2000-or-2005-database-server"></a>Connexion à la base de données dans un serveur de base de données Microsoft SQL Server 2000 ou 2005

Vous pouvez également vous connecter à une base de données Northwind installée sur un serveur de base de données. Si la base de données Northwind n’est pas encore installée sur le serveur de base de données, vous devez d’abord l’ajouter au serveur de base de données en exécutant le script d’installation inclus dans le téléchargement de ce didacticiel ou en [téléchargeant la version 2000 de Northwind et SQL Server le script d’installation](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en) directement à partir du site Web de Microsoft.

Une fois la base de données installée, accédez au Explorateur de serveurs dans Visual Studio, cliquez avec le bouton droit sur le nœud Connexions de données, puis choisissez Ajouter une connexion. Si vous ne voyez pas la Explorateur de serveurs accédez à la vue/Explorateur de serveurs ou appuyez sur Ctrl + Alt + S. La boîte de dialogue Ajouter une connexion s’affiche, dans laquelle vous pouvez spécifier le serveur auquel vous souhaitez vous connecter, les informations d’authentification et le nom de la base de données. Une fois que vous avez correctement configuré les informations de connexion à la base de données et cliqué sur le bouton OK, la base de données est ajoutée en tant que nœud sous le nœud Connexions de données. Vous pouvez développer le nœud de base de données pour explorer ses tables, vues, procédures stockées, etc.

![Ajouter une connexion à la base de données Northwind de votre serveur de base de données](creating-a-data-access-layer-vb/_static/image4.png)

**Figure 2**: ajouter une connexion à la base de données Northwind de votre serveur de base de données

## <a name="step-2-creating-the-data-access-layer"></a>Étape 2 : création de la couche d’accès aux données

Lorsque vous utilisez des données, une option consiste à incorporer la logique spécifique aux données directement dans la couche de présentation (dans une application Web, les pages ASP.NET constituent la couche de présentation). Cela peut prendre la forme d’écrire du code ADO.NET dans la partie de code de la page ASP.NET ou à l’aide du contrôle SqlDataSource à partir de la partie du balisage. Dans les deux cas, cette approche couple étroitement la logique d’accès aux données à la couche de présentation. Toutefois, l’approche recommandée consiste à séparer la logique d’accès aux données de la couche de présentation. Cette couche distincte est appelée couche d’accès aux données, DAL pour Short, et est généralement implémentée en tant que projet de bibliothèque de classes distinct. Les avantages de cette architecture en couches sont bien documentés (reportez-vous à la section « lectures supplémentaires » à la fin de ce didacticiel pour plus d’informations sur ces avantages) et c’est l’approche que nous allons suivre dans cette série.

Tout le code spécifique à la source de données sous-jacente, par exemple créer une connexion à la base de données, émettre `SELECT`, `INSERT`, `UPDATE`et `DELETE` des commandes, etc., doit se trouver dans la couche DAL. La couche de présentation ne doit pas contenir de références à ce code d’accès aux données, mais doit à la place effectuer des appels dans la couche DAL pour toutes les requêtes de données. Les couches d’accès aux données contiennent généralement des méthodes permettant d’accéder aux données de la base de données sous-jacente. Par exemple, la base de données Northwind a `Products` et `Categories` des tables qui enregistrent les produits à la vente et les catégories auxquelles elles appartiennent. Dans notre DAL, nous disposons de méthodes telles que :

- `GetCategories(),` qui retournera des informations sur toutes les catégories
- `GetProducts()`, qui renverra des informations sur tous les produits
- `GetProductsByCategoryID(categoryID)`, qui retournera tous les produits appartenant à une catégorie spécifiée
- `GetProductByProductID(productID)`, qui renverra des informations sur un produit particulier

Ces méthodes, lorsqu’elles sont appelées, se connectent à la base de données, émettent la requête appropriée et retournent les résultats. La façon dont nous retournons ces résultats est importante. Ces méthodes peuvent simplement retourner un jeu de données ou un DataReader rempli par la requête de base de données, mais dans l’idéal, ces résultats doivent être retournés à l’aide d' *objets fortement typés*. Un objet fortement typé est un objet dont le schéma est défini de façon rigide au moment de la compilation, tandis que l’inverse, un objet faiblement typé, est un objet dont le schéma n’est pas connu jusqu’à l’exécution.

Par exemple, le DataReader et le DataSet (par défaut) sont des objets faiblement typés, car leur schéma est défini par les colonnes retournées par la requête de base de données utilisée pour les remplir. Pour accéder à une colonne particulière à partir d’un DataTable faiblement typé, nous devons utiliser une syntaxe comme : `DataTable.Rows(index)("columnName")`. Le typage libre du DataTable dans cet exemple est présenté par le fait que nous devons accéder au nom de colonne à l’aide d’un index de chaîne ou d’un index ordinal. En revanche, un DataTable fortement typé aura chacune de ses colonnes implémentée en tant que propriétés, ce qui entraînera un code similaire à celui-ci : `DataTable.Rows(index).columnName`.

Pour retourner des objets fortement typés, les développeurs peuvent créer leurs propres objets métier personnalisés ou utiliser des DataSets typés. Un objet métier est implémenté par le développeur sous la forme d’une classe dont les propriétés reflètent généralement les colonnes de la table de base de données sous-jacente représentée par l’objet métier. Un DataSet typé est une classe générée par Visual Studio, basée sur un schéma de base de données et dont les membres sont fortement typés en fonction de ce schéma. Le DataSet typé se compose de classes qui étendent les classes DataSet, DataTable et DataRow ADO.NET. En plus des DataTables fortement typés, les DataSets typés incluent désormais également les TableAdapters, qui sont des classes avec des méthodes permettant de remplir les DataTables du DataSet et de propager les modifications dans les DataTables vers la base de données.

> [!NOTE]
> Pour plus d’informations sur les avantages et les inconvénients de l’utilisation de DataSets typés par rapport à des objets métier personnalisés, consultez conception de composants de la [couche données et transmission de données à travers des niveaux](https://msdn.microsoft.com/library/ms978496.aspx).

Nous utiliserons des DataSets fortement typés pour l’architecture de ces didacticiels. La figure 3 illustre le flux de travail entre les différentes couches d’une application qui utilise des DataSets typés.

[![tous les codes d’accès aux données sont relégués à la couche DAL](creating-a-data-access-layer-vb/_static/image6.png)](creating-a-data-access-layer-vb/_static/image5.png)

**Figure 3**: tous les codes d’accès aux données sont relégués à la couche DAL ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image7.png))

## <a name="creating-a-typed-dataset-and-table-adapter"></a>Création d’un DataSet typé et d’un adaptateur de table

Pour commencer à créer notre couche DAL, nous commençons par ajouter un DataSet typé à notre projet. Pour ce faire, cliquez avec le bouton droit sur le nœud du projet dans la Explorateur de solutions et choisissez Ajouter un nouvel élément. Sélectionnez l’option DataSet dans la liste des modèles et nommez-la `Northwind.xsd`.

[![choisissez d’ajouter un nouveau jeu de données à votre projet](creating-a-data-access-layer-vb/_static/image9.png)](creating-a-data-access-layer-vb/_static/image8.png)

**Figure 4**: choisir d’ajouter un nouveau jeu de données à votre projet ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image10.png))

Après avoir cliqué sur Ajouter, lorsque vous êtes invité à ajouter le jeu de données au dossier `App_Code`, choisissez Oui. Le concepteur du DataSet typé s’affiche alors et l’Assistant Configuration de TableAdapter démarre, ce qui vous permet d’ajouter votre premier TableAdapter au DataSet typé.

Un DataSet typé sert de collection fortement typée de données ; elle est composée d’instances de DataTable fortement typées, chacune d’elles étant à son tour composée d’instances DataRow fortement typées. Nous allons créer un DataTable fortement typé pour chaque table de base de données sous-jacente que nous devons utiliser dans cette série de didacticiels. Commençons par créer un DataTable pour la table `Products`.

N’oubliez pas que les DataTables fortement typés n’incluent aucune information sur la façon d’accéder aux données à partir de la table de base de données sous-jacente. Pour récupérer les données afin de remplir le DataTable, nous utilisons une classe TableAdapter, qui fonctionne comme la couche d’accès aux données. Pour notre `Products` DataTable, le TableAdapter contient les méthodes `GetProducts()`, `GetProductByCategoryID(categoryID)`, etc. que nous appellerons à partir de la couche de présentation. Le rôle du DataTable est de servir d’objets fortement typés utilisés pour passer des données entre les couches.

L’Assistant Configuration de TableAdapter démarre en vous invitant à sélectionner la base de données à utiliser. La liste déroulante affiche les bases de données du Explorateur de serveurs. Si vous n’avez pas ajouté la base de données Northwind au Explorateur de serveurs, vous pouvez cliquer sur le bouton nouvelle connexion à ce moment-là.

[![choisir la base de données Northwind dans la liste déroulante](creating-a-data-access-layer-vb/_static/image12.png)](creating-a-data-access-layer-vb/_static/image11.png)

**Figure 5**: choisir la base de données Northwind dans la liste déroulante ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image13.png))

Après avoir sélectionné la base de données et cliqué sur suivant, un message vous demande si vous souhaitez enregistrer la chaîne de connexion dans le fichier de `Web.config`. En enregistrant la chaîne de connexion, vous évitez de la coder en dur dans les classes TableAdapter, ce qui simplifie les choses si les informations de chaîne de connexion changent à l’avenir. Si vous choisissez d’enregistrer la chaîne de connexion dans le fichier de configuration, elle est placée dans la section `<connectionStrings>`, qui peut [éventuellement être chiffrée](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx) pour améliorer la sécurité ou être modifiée ultérieurement via la nouvelle page de propriétés ASP.NET 2,0 dans l’outil d’administration de l’interface utilisateur graphique IIS, ce qui est plus idéal pour les administrateurs.

[![enregistrer la chaîne de connexion dans Web. config](creating-a-data-access-layer-vb/_static/image15.png)](creating-a-data-access-layer-vb/_static/image14.png)

**Figure 6**: enregistrer la chaîne de connexion dans `Web.config` ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image16.png))

Ensuite, nous devons définir le schéma pour le premier DataTable fortement typé et fournir la première méthode que notre TableAdapter utilisera lors du remplissage du DataSet fortement typé. Ces deux étapes sont effectuées simultanément en créant une requête qui retourne les colonnes de la table que nous voulons voir reflétées dans notre DataTable. À la fin de l’Assistant, nous attribuons un nom de méthode à cette requête. Une fois cette opération effectuée, cette méthode peut être appelée à partir de notre couche de présentation. La méthode exécutera la requête définie et remplira un DataTable fortement typé.

Pour commencer à définir la requête SQL, nous devons d’abord indiquer comment le TableAdapter doit émettre la requête. Nous pouvons utiliser une instruction SQL ad hoc, créer une nouvelle procédure stockée ou utiliser une procédure stockée existante. Pour ces didacticiels, nous allons utiliser des instructions SQL ad hoc. Reportez-vous à l’article de [Brian auteur](http://briannoyes.net/), [créer une couche d’accès aux données avec le concepteur de DataSet Visual Studio 2005](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner) pour obtenir un exemple d’utilisation de procédures stockées.

[![interroger les données à l’aide d’une instruction SQL ad hoc](creating-a-data-access-layer-vb/_static/image18.png)](creating-a-data-access-layer-vb/_static/image17.png)

**Figure 7**: interroger les données à l’aide d’une instruction SQL ad hoc ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image19.png))

À ce stade, nous pouvons taper la requête SQL manuellement. Lors de la création de la première méthode dans le TableAdapter, vous souhaitez que la requête retourne les colonnes qui doivent être exprimées dans le DataTable correspondant. Pour ce faire, nous pouvons créer une requête qui retourne toutes les colonnes et toutes les lignes de la table `Products` :

[![entrez la requête SQL dans la zone de texte](creating-a-data-access-layer-vb/_static/image21.png)](creating-a-data-access-layer-vb/_static/image20.png)

**Figure 8**: entrez la requête SQL dans la zone de texte ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image22.png))

Vous pouvez également utiliser la Générateur de requêtes et construire graphiquement la requête, comme illustré à la figure 9.

[![créer la requête graphiquement, par le biais de l’éditeur de requête](creating-a-data-access-layer-vb/_static/image24.png)](creating-a-data-access-layer-vb/_static/image23.png)

**Figure 9**: créer la requête graphiquement, par le biais de l’éditeur de requête ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image25.png))

Après avoir créé la requête, mais avant de passer à l’écran suivant, cliquez sur le bouton Options avancées. Dans les projets de site Web, « générer des instructions INSERT, Update et DELETE » est la seule option avancée sélectionnée par défaut. Si vous exécutez cet Assistant à partir d’une bibliothèque de classes ou d’un projet Windows, l’option utiliser l’accès concurrentiel optimiste est également sélectionnée. Laissez l’option « utiliser l’accès concurrentiel optimiste » désactivée pour le moment. Nous examinerons l’accès concurrentiel optimiste dans les prochains didacticiels.

[![sélectionnez uniquement l’option générer des instructions INSERT, Update et Delete](creating-a-data-access-layer-vb/_static/image27.png)](creating-a-data-access-layer-vb/_static/image26.png)

**Figure 10**: sélectionner uniquement l’option générer des instructions INSERT, Update et Delete ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image28.png))

Après avoir vérifié les options avancées, cliquez sur suivant pour passer à l’écran final. Ici, nous sommes invités à sélectionner les méthodes à ajouter au TableAdapter. Il existe deux modèles pour alimenter les données :

- **Remplissez un DataTable** avec cette approche. une méthode est créée qui prend un DataTable comme paramètre et le remplit en fonction des résultats de la requête. Par exemple, la classe ADO.NET DataAdapter implémente ce modèle avec sa méthode `Fill()`.
- **Retourner un DataTable** avec cette approche, la méthode crée et remplit le DataTable pour vous et le retourne comme valeur de retour des méthodes.

Vous pouvez faire en sorte que le TableAdapter implémente l’un ou l’autre de ces modèles. Vous pouvez également renommer les méthodes fournies ici. Laissez les deux cases à cocher activées, même si nous n’utiliserons le dernier modèle que dans ces didacticiels. Nous allons également renommer la méthode `GetData` plutôt générique pour `GetProducts`.

Si elle est activée, la case à cocher finale, « GenerateDBDirectMethods », crée `Insert()`méthodes, `Update()`et `Delete()` pour le TableAdapter. Si vous laissez cette option désactivée, toutes les mises à jour doivent être effectuées via la seule `Update()` méthode du TableAdapter, qui prend le DataSet typé, un DataTable, un DataRow unique ou un tableau de DataRows. (Si vous avez désactivé l’option « générer des instructions INSERT, Update et Delete » dans les propriétés avancées de la figure 9, le paramètre de cette case à cocher n’a aucun effet.) Laissez cette case à cocher activée.

[![modifier le nom de la méthode GetData en GetProducts](creating-a-data-access-layer-vb/_static/image30.png)](creating-a-data-access-layer-vb/_static/image29.png)

**Figure 11**: remplacer le nom de la méthode `GetData` par `GetProducts` ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image31.png))

Terminez l’Assistant en cliquant sur Terminer. Une fois l’Assistant fermé, nous revenons au concepteur de DataSet qui affiche le DataTable que nous venons de créer. Vous pouvez voir la liste des colonnes dans le `Products` DataTable (`ProductID`, `ProductName`, etc.), ainsi que les méthodes de l' `ProductsTableAdapter` (`Fill()` et `GetProducts()`).

[![les produits, DataTable et ProductsTableAdapter, ont été ajoutés au DataSet typé](creating-a-data-access-layer-vb/_static/image33.png)](creating-a-data-access-layer-vb/_static/image32.png)

**Figure 12**: l' `Products` DataTable et `ProductsTableAdapter` ont été ajoutés au DataSet typé ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image34.png))

À ce stade, nous avons un DataSet typé avec un DataTable unique (`Northwind.Products`) et une classe DataAdapter fortement typée (`NorthwindTableAdapters.ProductsTableAdapter`) avec une méthode `GetProducts()`. Ces objets peuvent être utilisés pour accéder à une liste de tous les produits à partir d’un code tel que :

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample1.vb)]

Ce code n’a pas besoin d’écrire un peu de code spécifique à l’accès aux données. Nous n’avons pas eu à instancier des classes ADO.NET, nous n’avons pas besoin de faire référence à des chaînes de connexion, des requêtes SQL ou des procédures stockées. Au lieu de cela, le TableAdapter fournit le code d’accès aux données de bas niveau pour nous.

Chaque objet utilisé dans cet exemple est également fortement typé, ce qui permet à Visual Studio de fournir IntelliSense et la vérification de type au moment de la compilation. Et mieux que tous les DataTables retournés par le TableAdapter peuvent être liés à des contrôles Web de données ASP.NET, tels que GridView, DetailsView, DropDownList, CheckBoxList et plusieurs autres. L’exemple suivant illustre la liaison du DataTable retourné par la méthode `GetProducts()` à un GridView dans une simple ou trois lignes de code dans le gestionnaire d’événements `Page_Load`.

AllProducts. aspx

[!code-aspx[Main](creating-a-data-access-layer-vb/samples/sample2.aspx)]

AllProducts. aspx. vb

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample3.vb)]

[![la liste des produits s’affiche dans un GridView](creating-a-data-access-layer-vb/_static/image36.png)](creating-a-data-access-layer-vb/_static/image35.png)

**Figure 13**: la liste des produits s’affiche dans un GridView ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image37.png))

Bien que cet exemple exige que nous écrivions trois lignes de code dans le gestionnaire d’événements `Page_Load` de notre page ASP.NET, dans les prochains didacticiels, nous allons examiner comment utiliser ObjectDataSource pour récupérer de façon déclarative les données de la couche DAL. Avec ObjectDataSource, nous n’aurons pas à écrire de code et obtiendrons également la prise en charge de la pagination et du tri.

## <a name="step-3-adding-parameterized-methods-to-the-data-access-layer"></a>Étape 3 : ajout de méthodes paramétrables à la couche d’accès aux données

À ce stade, notre classe de `ProductsTableAdapter` a, mais une méthode, `GetProducts()`, qui retourne tous les produits de la base de données. Bien qu’il soit très utile de pouvoir travailler avec tous les produits, il peut arriver que nous puissions récupérer des informations sur un produit spécifique ou sur tous les produits appartenant à une catégorie particulière. Pour ajouter cette fonctionnalité à notre couche d’accès aux données, nous pouvons ajouter des méthodes paramétrables au TableAdapter.

Ajoutons la méthode `GetProductsByCategoryID(categoryID)`. Pour ajouter une nouvelle méthode à la couche DAL, revenez au concepteur de DataSet, cliquez avec le bouton droit dans la section `ProductsTableAdapter`, puis choisissez Ajouter une requête.

![Cliquez avec le bouton droit sur le TableAdapter, puis choisissez Ajouter une requête.](creating-a-data-access-layer-vb/_static/image38.png)

**Figure 14**: cliquez avec le bouton droit sur le TableAdapter, puis choisissez Ajouter une requête.

Nous commençons par indiquer si nous souhaitons accéder à la base de données à l’aide d’une instruction SQL ad hoc ou d’une procédure stockée nouvelle ou existante. Nous allons choisir une nouvelle fois une instruction SQL ad hoc. Ensuite, nous vous demanderons le type de requête SQL que nous souhaitons utiliser. Étant donné que nous souhaitons renvoyer tous les produits appartenant à une catégorie spécifiée, nous voulons écrire une instruction `SELECT` qui retourne des lignes.

[![choisir de créer une instruction SELECT qui retourne des lignes](creating-a-data-access-layer-vb/_static/image40.png)](creating-a-data-access-layer-vb/_static/image39.png)

**Figure 15**: choisir de créer une instruction `SELECT` qui renvoie des lignes ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image41.png))

L’étape suivante consiste à définir la requête SQL utilisée pour accéder aux données. Étant donné que nous souhaitons retourner uniquement les produits appartenant à une catégorie particulière, j’utilise la même instruction `SELECT` dans `GetProducts()`, mais j’ajoute la clause `WHERE` suivante : `WHERE CategoryID = @CategoryID`. Le paramètre `@CategoryID` indique à l’Assistant TableAdapter que la méthode que nous créons nécessite un paramètre d’entrée du type correspondant (c’est-à-dire un entier Nullable).

[![entrer une requête pour retourner uniquement les produits d’une catégorie spécifiée](creating-a-data-access-layer-vb/_static/image43.png)](creating-a-data-access-layer-vb/_static/image42.png)

**Figure 16**: entrer une requête pour retourner uniquement les produits d’une catégorie spécifiée ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image44.png))

Dans la dernière étape, nous pouvons choisir les modèles d’accès aux données à utiliser, ainsi que personnaliser les noms des méthodes générées. Pour le modèle de remplissage, nous allons remplacer le nom par `FillByCategoryID` et pour que retourne un modèle de retour de DataTable (méthodes de `GetX`), nous utilisons `GetProductsByCategoryID`.

[![choisir les noms des méthodes TableAdapter](creating-a-data-access-layer-vb/_static/image46.png)](creating-a-data-access-layer-vb/_static/image45.png)

**Figure 17**: choisir les noms des méthodes TableAdapter ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image47.png))

Une fois l’Assistant terminé, le concepteur de DataSet comprend les nouvelles méthodes TableAdapter.

![Les produits peuvent désormais être interrogés par catégorie](creating-a-data-access-layer-vb/_static/image48.png)

**Figure 18**: les produits peuvent désormais être interrogés par catégorie

Prenez un moment pour ajouter une méthode de `GetProductByProductID(productID)` à l’aide de la même technique.

Ces requêtes paramétrables peuvent être testées directement à partir du concepteur de DataSet. Cliquez avec le bouton droit sur la méthode dans le TableAdapter et choisissez Aperçu des données. Ensuite, entrez les valeurs à utiliser pour les paramètres, puis cliquez sur Aperçu.

[![les produits appartenant à la catégorie boissons sont affichés](creating-a-data-access-layer-vb/_static/image50.png)](creating-a-data-access-layer-vb/_static/image49.png)

**Figure 19**: les produits appartenant à la catégorie boissons sont affichés ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image51.png))

Avec la méthode `GetProductsByCategoryID(categoryID)` dans notre couche DAL, nous pouvons maintenant créer une page ASP.NET qui affiche uniquement les produits appartenant à une catégorie spécifiée. L’exemple suivant affiche tous les produits qui se trouvent dans la catégorie boissons, qui ont une `CategoryID` de 1.

Boissons. aspx

[!code-aspx[Main](creating-a-data-access-layer-vb/samples/sample4.aspx)]

Boissons. aspx. vb

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample5.vb)]

[![les produits de la catégorie boissons s’affichent.](creating-a-data-access-layer-vb/_static/image53.png)](creating-a-data-access-layer-vb/_static/image52.png)

**Figure 20**: les produits de la catégorie boissons sont affichés ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image54.png))

## <a name="step-4-inserting-updating-and-deleting-data"></a>Étape 4 : insertion, mise à jour et suppression de données

Il existe deux modèles couramment utilisés pour l’insertion, la mise à jour et la suppression de données. Le premier modèle, que j’appellerai le modèle de base de données direct, implique la création de méthodes qui, lorsqu’elles sont appelées, émettent une commande `INSERT`, `UPDATE`ou `DELETE` dans la base de données qui fonctionne sur un seul enregistrement de base de données. Ces méthodes sont généralement transmises dans une série de valeurs scalaires (entiers, chaînes, booléens, DateTime, etc.) qui correspondent aux valeurs à insérer, mettre à jour ou supprimer. Par exemple, avec ce modèle pour la table `Products` la méthode Delete prendrait un paramètre entier, indiquant la `ProductID` de l’enregistrement à supprimer, tandis que la méthode Insert prendrait une chaîne pour le `ProductName`, un décimal pour le `UnitPrice`, un entier pour la `UnitsOnStock`, et ainsi de suite.

[![chaque demande d’insertion, de mise à jour et de suppression est immédiatement envoyée à la base de données.](creating-a-data-access-layer-vb/_static/image56.png)](creating-a-data-access-layer-vb/_static/image55.png)

**Figure 21**: chaque demande d’insertion, de mise à jour et de suppression est immédiatement envoyée à la base de données ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image57.png))

L’autre modèle, que je vais désigner comme modèle de mise à jour par lot, consiste à mettre à jour l’intégralité d’un DataSet, d’un DataTable ou d’une collection de DataRows dans un appel de méthode. Avec ce modèle, un développeur supprime, insère et modifie les DataRows dans un DataTable, puis passe ces DataRow ou DataTable dans une méthode de mise à jour. Cette méthode énumère ensuite les DataRows passés, détermine si elles ont été modifiées, ajoutées ou supprimées (via la valeur de la [propriété RowState](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) du DataRow) et émet la requête de base de données appropriée pour chaque enregistrement.

[![toutes les modifications sont synchronisées avec la base de données lorsque la méthode Update est appelée](creating-a-data-access-layer-vb/_static/image59.png)](creating-a-data-access-layer-vb/_static/image58.png)

**Figure 22**: toutes les modifications sont synchronisées avec la base de données lorsque la méthode de mise à jour est appelée ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image60.png))

Le TableAdapter utilise le modèle de mise à jour par lot par défaut, mais il prend également en charge le modèle de base de base de type direct. Étant donné que nous avons sélectionné l’option « générer des instructions INSERT, Update et Delete » dans les propriétés avancées lors de la création de notre TableAdapter, le `ProductsTableAdapter` contient une méthode `Update()`, qui implémente le modèle de mise à jour par lot. Plus précisément, le TableAdapter contient une méthode `Update()` qui peut être passée au DataSet typé, à un DataTable fortement typé ou à un ou plusieurs DataRows. Si vous avez laissé la case à cocher « GenerateDBDirectMethods » activée lors de la création initiale du TableAdapter, le modèle de base de base de type direct est également implémenté via les méthodes `Insert()`, `Update()`et `Delete()`.

Les deux modèles de modification de données utilisent les propriétés `InsertCommand`, `UpdateCommand`et `DeleteCommand` du TableAdapter pour émettre leurs commandes `INSERT`, `UPDATE`et `DELETE` dans la base de données. Vous pouvez inspecter et modifier les propriétés `InsertCommand`, `UpdateCommand`et `DeleteCommand` en cliquant sur le TableAdapter dans le concepteur de DataSet, puis en accédant au Fenêtre Propriétés. (Assurez-vous que vous avez sélectionné le TableAdapter et que l’objet `ProductsTableAdapter` est celui sélectionné dans la liste déroulante de la Fenêtre Propriétés.)

[![le TableAdapter possède les propriétés InsertCommand, UpdateCommand et DeleteCommand](creating-a-data-access-layer-vb/_static/image62.png)](creating-a-data-access-layer-vb/_static/image61.png)

**Figure 23**: le TableAdapter possède les propriétés `InsertCommand`, `UpdateCommand`et `DeleteCommand` ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image63.png))

Pour examiner ou modifier l’une de ces propriétés de commande de base de données, cliquez sur la `CommandText` sous-propriété, qui affichera le Générateur de requêtes.

[![configurer les instructions INSERT, UPDATE et DELETE dans le Générateur de requêtes](creating-a-data-access-layer-vb/_static/image65.png)](creating-a-data-access-layer-vb/_static/image64.png)

**Figure 24**: configurer les instructions `INSERT`, `UPDATE`et `DELETE` dans le générateur de requêtes ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image66.png))

L’exemple de code suivant montre comment utiliser le modèle de mise à jour par lot pour doubler le prix de tous les produits qui ne sont pas interrompus et qui ont 25 unités en stock ou moins :

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample6.vb)]

Le code ci-dessous illustre l’utilisation du modèle de base de données direct pour supprimer par programmation un produit particulier, puis le mettre à jour, puis en ajouter un nouveau :

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample7.vb)]

## <a name="creating-custom-insert-update-and-delete-methods"></a>Création de méthodes d’insertion, de mise à jour et de suppression personnalisées

Les méthodes `Insert()`, `Update()`et `Delete()` créées par la méthode DB direct peuvent être un peu fastidieuses, en particulier pour les tables comportant de nombreuses colonnes. En examinant l’exemple de code précédent, sans l’aide d’IntelliSense, il n’est pas très facile de savoir ce que `Products` colonne de table mappe à chaque paramètre d’entrée aux méthodes de `Update()` et de `Insert()`. Il peut arriver que vous souhaitiez uniquement mettre à jour une ou deux colonnes, ou une méthode de `Insert()` personnalisée qui, éventuellement, retourne la valeur du champ `IDENTITY` (incrémentation automatique) de l’enregistrement récemment inséré.

Pour créer une telle méthode personnalisée, retournez dans le concepteur de DataSet. Cliquez avec le bouton droit sur le TableAdapter et choisissez Ajouter une requête, en revenant à l’Assistant TableAdapter. Dans le deuxième écran, nous pouvons indiquer le type de requête à créer. Créons une méthode qui ajoute un nouveau produit, puis retourne la valeur de la `ProductID`de l’enregistrement qui vient d’être ajouté. Par conséquent, choisissez de créer une requête de `INSERT`.

[![créer une méthode pour ajouter une nouvelle ligne à la table Products](creating-a-data-access-layer-vb/_static/image68.png)](creating-a-data-access-layer-vb/_static/image67.png)

**Figure 25**: créer une méthode pour ajouter une nouvelle ligne à la table `Products` ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image69.png))

Dans l’écran suivant, le `CommandText` du `InsertCommand`s’affiche. Augmentez cette requête en ajoutant `SELECT SCOPE_IDENTITY()` à la fin de la requête, qui renverra la dernière valeur d’identité insérée dans une colonne `IDENTITY` de la même portée. (Consultez la [documentation technique](https://msdn.microsoft.com/library/ms190315.aspx) pour plus d’informations sur les `SCOPE_IDENTITY()` et la raison pour laquelle vous souhaiterez probablement [utiliser l’étendue\_identité () à la place de @@IDENTITY](http://weblogs.sqlteam.com/travisl/archive/2003/10/29/405.aspx).) Veillez à terminer l’instruction `INSERT` par un point-virgule avant d’ajouter l’instruction `SELECT`.

[![augmenter la requête pour retourner la valeur SCOPE_IDENTITY ()](creating-a-data-access-layer-vb/_static/image71.png)](creating-a-data-access-layer-vb/_static/image70.png)

**Figure 26**: augmenter la requête pour retourner la valeur `SCOPE_IDENTITY()` ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image72.png))

Enfin, nommez la nouvelle méthode `InsertProduct`.

[![définir le nom de la nouvelle méthode sur InsertProduct](creating-a-data-access-layer-vb/_static/image74.png)](creating-a-data-access-layer-vb/_static/image73.png)

**Figure 27**: définir le nom de la nouvelle méthode sur `InsertProduct` ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image75.png))

Lorsque vous revenez au concepteur de DataSet, vous verrez que le `ProductsTableAdapter` contient une nouvelle méthode, `InsertProduct`. Si cette nouvelle méthode n’a pas de paramètre pour chaque colonne de la table `Products`, il est probable que vous ayez oublié de terminer l’instruction `INSERT` par un point-virgule. Configurez la méthode `InsertProduct` et assurez-vous de disposer d’un point-virgule délimitant les instructions `INSERT` et `SELECT`.

Par défaut, les méthodes d’insertion émettent des méthodes qui ne sont pas des requêtes, ce qui signifie qu’elles retournent le nombre de lignes affectées. Toutefois, nous voulons que la méthode `InsertProduct` retourne la valeur retournée par la requête, et non le nombre de lignes affectées. Pour ce faire, affectez à la propriété `ExecuteMode` de la méthode `InsertProduct` la valeur `Scalar`.

[![modifier la propriété ExecuteMode en scalaire](creating-a-data-access-layer-vb/_static/image77.png)](creating-a-data-access-layer-vb/_static/image76.png)

**Figure 28**: modifier la propriété `ExecuteMode` en `Scalar` ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image78.png))

Le code suivant illustre cette nouvelle méthode de `InsertProduct` en action :

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample8.vb)]

## <a name="step-5-completing-the-data-access-layer"></a>Étape 5 : fin de la couche d’accès aux données

Notez que la classe `ProductsTableAdapters` retourne les valeurs `CategoryID` et `SupplierID` de la table `Products`, mais n’inclut pas la colonne `CategoryName` de la table `Categories` ou la colonne `CompanyName` de la table `Suppliers`, bien qu’il s’agisse des colonnes que nous souhaitons afficher lors de l’affichage des informations sur le produit. Nous pouvons augmenter la méthode initiale du TableAdapter, `GetProducts()`, pour inclure les valeurs de colonne `CategoryName` et `CompanyName`, qui mettra à jour le DataTable fortement typé pour inclure ces nouvelles colonnes également.

Cela peut présenter un problème, toutefois, comme les méthodes du TableAdapter pour l’insertion, la mise à jour et la suppression des données sont basées sur cette méthode initiale. Heureusement, les méthodes générées automatiquement pour l’insertion, la mise à jour et la suppression ne sont pas affectées par les sous-requêtes dans la clause `SELECT`. En veillant à ajouter nos requêtes à `Categories` et `Suppliers` en tant que sous-requêtes, au lieu de `JOIN`, nous éviterons de devoir réutiliser ces méthodes pour modifier les données. Cliquez avec le bouton droit sur la méthode `GetProducts()` dans le `ProductsTableAdapter` et choisissez configurer. Ensuite, ajustez la clause `SELECT` afin qu’elle ressemble à ceci :

[!code-sql[Main](creating-a-data-access-layer-vb/samples/sample9.sql)]

[![mettre à jour l’instruction SELECT pour la méthode GetProducts ()](creating-a-data-access-layer-vb/_static/image80.png)](creating-a-data-access-layer-vb/_static/image79.png)

**Figure 29**: mettre à jour l’instruction `SELECT` pour la méthode `GetProducts()` ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image81.png))

Après la mise à jour de la méthode `GetProducts()` pour utiliser cette nouvelle requête, le DataTable inclut deux nouvelles colonnes : `CategoryName` et `SupplierName`.

![Le DataTable Products comporte deux nouvelles colonnes](creating-a-data-access-layer-vb/_static/image82.png)

**Figure 30**: le `Products` DataTable a deux nouvelles colonnes

Prenez un moment pour mettre à jour la clause `SELECT` dans la méthode `GetProductsByCategoryID(categoryID)` également.

Si vous mettez à jour le `GetProducts()` `SELECT` à l’aide de la syntaxe `JOIN`, le concepteur de DataSet ne pourra pas générer automatiquement les méthodes d’insertion, de mise à jour et de suppression des données de base de données à l’aide du modèle de base de données direct. Au lieu de cela, vous devez les créer manuellement comme nous l’avons fait avec la méthode `InsertProduct` plus haut dans ce didacticiel. En outre, vous devrez fournir manuellement les valeurs de propriété `InsertCommand`, `UpdateCommand`et `DeleteCommand` si vous souhaitez utiliser le modèle de mise à jour par lot.

## <a name="adding-the-remaining-tableadapters"></a>Ajout des TableAdapters restants

Jusqu’à présent, nous avons uniquement regardé l’utilisation d’un seul TableAdapter pour une seule table de base de données. Toutefois, la base de données Northwind contient plusieurs tables associées que nous devrons utiliser dans notre application Web. Un DataSet typé peut contenir plusieurs DataTables associés. Par conséquent, pour terminer notre DAL, nous devons ajouter des DataTables pour les autres tables que nous utiliserons dans ces didacticiels. Pour ajouter un nouveau TableAdapter à un DataSet typé, ouvrez le concepteur de DataSet, cliquez avec le bouton droit dans le concepteur, puis choisissez Ajouter/TableAdapter. Cela créera un nouveau DataTable et TableAdapter et vous guidera tout au long de l’Assistant que nous avons examiné précédemment dans ce didacticiel.

Prenez quelques minutes pour créer les TableAdapters et les méthodes suivants à l’aide des requêtes suivantes. Notez que les requêtes dans le `ProductsTableAdapter` incluent les sous-requêtes pour récupérer les noms de catégorie et de fournisseur de chaque produit. En outre, si vous avez déjà effectué les opérations suivantes, vous avez déjà ajouté les méthodes de `GetProducts()` et de `GetProductsByCategoryID(categoryID)` de la classe `ProductsTableAdapter`.

- **ProductsTableAdapter**

  - **GetProducts**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample10.sql)]
  - **GetProductsByCategoryID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample11.sql)]
  - **GetProductsBySupplierID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample12.sql)]
  - **GetProductByProductID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample13.sql)]
- **CategoriesTableAdapter**

  - **GetCategories**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample14.sql)]
  - **GetCategoryByCategoryID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample15.sql)]
- **SuppliersTableAdapter**

  - **GetSuppliers**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample16.sql)]
  - **GetSuppliersByCountry**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample17.sql)]
  - **GetSupplierBySupplierID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample18.sql)]
- **EmployeesTableAdapter**

  - **GetEmployees**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample19.sql)]
  - **GetEmployeesByManager**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample20.sql)]
  - **GetEmployeeByEmployeeID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample21.sql)]

[![le concepteur de DataSet une fois que les quatre TableAdapters ont été ajoutés](creating-a-data-access-layer-vb/_static/image84.png)](creating-a-data-access-layer-vb/_static/image83.png)

**Figure 31**: concepteur de DataSet une fois que les quatre TableAdapters ont été ajoutés ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image85.png))

## <a name="adding-custom-code-to-the-dal"></a>Ajout de code personnalisé à la couche DAL

Les TableAdapters et les DataTables ajoutés au DataSet typé sont exprimés sous la forme d’un fichier de définition de schéma XML (`Northwind.xsd`). Pour afficher ces informations de schéma, cliquez avec le bouton droit sur le fichier `Northwind.xsd` dans le Explorateur de solutions puis sélectionnez Afficher le code.

[![le fichier XSD (XML Schema Definition) du DataSet typé Northwind](creating-a-data-access-layer-vb/_static/image87.png)](creating-a-data-access-layer-vb/_static/image86.png)

**Figure 32**: fichier XSD (XML Schema Definition) du DataSet typé Northwind ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image88.png))

Ces informations de schéma sont traduites dans C# ou Visual Basic code au moment de la conception, lors de la compilation ou au moment de l’exécution (si nécessaire), à partir de quel moment vous pouvez effectuer un pas à pas détaillé avec le débogueur. Pour afficher ce code généré automatiquement, accédez à la Affichage de classes et explorez les classes TableAdapter ou DataSet typé. Si vous ne voyez pas l’Affichage de classes sur votre écran, accédez au menu Affichage et sélectionnez-le à partir de là, ou appuyez sur Ctrl + Maj + C. À partir de la Affichage de classes vous pouvez voir les propriétés, les méthodes et les événements du DataSet typé et des classes TableAdapter. Pour afficher le code d’une méthode particulière, double-cliquez sur le nom de la méthode dans la Affichage de classes ou cliquez dessus avec le bouton droit, puis choisissez atteindre la définition.

![Inspectez le code généré automatiquement en sélectionnant atteindre la définition dans la Affichage de classes](creating-a-data-access-layer-vb/_static/image89.png)

**Figure 33**: inspecter le code généré automatiquement en sélectionnant atteindre la définition dans la affichage de classes

Alors que le code généré automatiquement peut être un économiseur de temps, le code est souvent très générique et doit être personnalisé pour répondre aux besoins uniques d’une application. Toutefois, le risque d’étendre le code généré automatiquement est que l’outil qui a généré le code peut décider qu’il est temps de « régénérer » et de remplacer vos personnalisations. Avec le nouveau concept de classe partielle de .NET 2.0, il est facile de fractionner une classe entre plusieurs fichiers. Cela nous permet d’ajouter nos propres méthodes, propriétés et événements aux classes générées automatiquement sans avoir à vous soucier de l’écrasement de nos personnalisations par Visual Studio.

Pour illustrer comment personnaliser la couche DAL, nous allons ajouter une méthode `GetProducts()` à la classe `SuppliersRow`. La classe `SuppliersRow` représente un enregistrement unique dans la table `Suppliers` ; chaque fournisseur peut fournir zéro à de nombreux produits, `GetProducts()` renverra donc les produits du fournisseur spécifié. Pour ce faire, créez un nouveau fichier de classe dans le dossier `App_Code` nommé `SuppliersRow.vb` et ajoutez le code suivant :

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample22.vb)]

Cette classe partielle indique au compilateur que, lors de la génération de la classe `Northwind.SuppliersRow`, il doit inclure la méthode `GetProducts()` que nous venons de définir. Si vous générez votre projet, puis revenez à la Affichage de classes vous verrez `GetProducts()` maintenant listé comme méthode de `Northwind.SuppliersRow`.

![La méthode GetProducts () fait maintenant partie de la classe Northwind. SuppliersRow](creating-a-data-access-layer-vb/_static/image90.png)

**Figure 34**: la méthode `GetProducts()` fait maintenant partie de la classe `Northwind.SuppliersRow`

La méthode `GetProducts()` peut maintenant être utilisée pour énumérer l’ensemble des produits pour un fournisseur particulier, comme le montre le code suivant :

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample23.vb)]

Ces données peuvent également être affichées dans n’importe quel ASP. Contrôles Web de données du réseau. La page suivante utilise un contrôle GridView avec deux champs :

- BoundField qui affiche le nom de chaque fournisseur et
- TemplateField qui contient un contrôle BulletedList lié aux résultats retournés par la méthode `GetProducts()` pour chaque fournisseur.

Nous allons examiner comment afficher ces rapports maître/détail dans les prochains didacticiels. Pour l’instant, cet exemple est conçu pour illustrer l’utilisation de la méthode personnalisée ajoutée à la classe `Northwind.SuppliersRow`.

SuppliersAndProducts. aspx

[!code-aspx[Main](creating-a-data-access-layer-vb/samples/sample24.aspx)]

SuppliersAndProducts. aspx. vb

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample25.vb)]

[![le nom de la société du fournisseur est indiqué dans la colonne de gauche, ses produits à droite](creating-a-data-access-layer-vb/_static/image92.png)](creating-a-data-access-layer-vb/_static/image91.png)

**Figure 35**: le nom de la société du fournisseur est indiqué dans la colonne de gauche, à savoir ses produits à droite ([cliquez pour afficher l’image en plein écran](creating-a-data-access-layer-vb/_static/image93.png))

## <a name="summary"></a>Récapitulatif

Quand vous créez une application Web, la création de la couche DAL doit être l’une de vos premières étapes, qui se produit avant de commencer à créer votre couche de présentation. Avec Visual Studio, la création d’une couche DAL basée sur des DataSets typés est une tâche qui peut être accomplie en 10-15 minutes sans écrire une ligne de code. Les didacticiels qui évoluent vers l’avant seront générés sur cette couche DAL. Dans le [didacticiel suivant](creating-a-business-logic-layer-vb.md) , nous allons définir un certain nombre de règles d’entreprise et voir comment les implémenter dans une couche de logique métier distincte.

Bonne programmation !

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Génération d’une couche DAL à l’aide de TableAdapters et de DataTables fortement typés dans VS 2005 et ASP.NET 2,0](https://weblogs.asp.net/scottgu/435498)
- [Conception de composants de la couche données et transmission de données à travers les niveaux](https://msdn.microsoft.com/library/ms978496.aspx)
- [Créer une couche d’accès aux données avec le concepteur de DataSet Visual Studio 2005](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner)
- [Chiffrement des informations de configuration dans les applications ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [Vue d’ensemble de TableAdapter](https://msdn.microsoft.com/library/bz9tthwx.aspx)
- [Utilisation d’un DataSet typé](https://msdn.microsoft.com/library/esbykkzb.aspx)
- [Utilisation de l’accès aux données fortement typées dans Visual Studio 2005 et ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/020806-1.aspx)
- [Comment étendre des méthodes TableAdapter](https://blogs.msdn.com/vbteam/archive/2005/05/04/ExtendingTableAdapters.aspx)
- [Récupération de données scalaires à partir d’une procédure stockée](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Formation vidéo sur les rubriques contenues dans ce didacticiel

- [Couches d’accès aux données dans les applications ASP.NET](../../../videos/data-access/adonet-data-services/data-access-layers-in-aspnet-applications.md)
- [Comment lier manuellement un DataSet à un DataGrid](../../../videos/data-access/adonet-data-services/how-to-manually-bind-a-dataset-to-a-datagrid.md)
- [Utilisation des jeux de données et des filtres d’une application ASP](../../../videos/data-access/adonet-data-services/how-to-work-with-datasets-and-filters-from-an-asp-application.md)

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel sont Ron Green, Hilton Giesenow, Denis Patterson, Liz Shulok, Gomez et Carlos Santos. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](master-pages-and-site-navigation-cs.md)
> [Suivant](creating-a-business-logic-layer-vb.md)
