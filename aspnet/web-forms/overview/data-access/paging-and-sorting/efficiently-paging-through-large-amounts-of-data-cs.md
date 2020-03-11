---
uid: web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs
title: Pagination efficace des grandes quantités de données (C#) | Microsoft Docs
author: rick-anderson
description: L’option de pagination par défaut d’un contrôle de présentation des données n’est pas appropriée quand vous travaillez avec de grandes quantités de données, comme le contrôle de source de données sous-jacent récupère...
ms.author: riande
ms.date: 08/15/2006
ms.assetid: 59c01998-9326-4ecb-9392-cb9615962140
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs
msc.type: authoredcontent
ms.openlocfilehash: a3e9562035cb24987b01fcdff5fbfb5fa8a1f894
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78589998"
---
# <a name="efficiently-paging-through-large-amounts-of-data-c"></a>Pagination efficace dans de grandes quantités de données (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_25_CS.exe) ou [Télécharger le PDF](efficiently-paging-through-large-amounts-of-data-cs/_static/datatutorial25cs1.pdf)

> L’option de pagination par défaut d’un contrôle de présentation des données n’est pas appropriée lorsque vous travaillez avec de grandes quantités de données, car le contrôle de source de données sous-jacent récupère tous les enregistrements, même si seul un sous-ensemble de données est affiché. Dans ce cas, nous devons activer la pagination personnalisée.

## <a name="introduction"></a>Introduction

Comme nous l’avons vu dans le didacticiel précédent, la pagination peut être implémentée de l’une des deux manières suivantes :

- La **pagination par défaut** peut être implémentée en activant simplement l’option Activer la pagination dans la balise active des contrôles Web de données. Toutefois, lorsque vous affichez une page de données, l’ObjectDataSource récupère *tous* les enregistrements, même si un seul sous-ensemble est affiché dans la page
- La **pagination personnalisée** améliore les performances de la pagination par défaut en extrayant uniquement les enregistrements de la base de données qui doivent être affichés pour la page de données particulière demandée par l’utilisateur. Toutefois, la pagination personnalisée implique un peu plus d’effort pour implémenter que la pagination par défaut

En raison de la facilité d’implémentation, activez simplement une case à cocher, puis réexécutez ! la pagination par défaut est une option intéressante. Son approche na ve de la récupération de tous les enregistrements en fait un choix non plausible lors de la pagination de quantités de données suffisamment volumineuses ou de sites avec de nombreux utilisateurs simultanés. Dans ce cas, nous devons activer la pagination personnalisée pour fournir un système réactif.

Le défi de la pagination personnalisée est de pouvoir écrire une requête qui retourne l’ensemble précis des enregistrements nécessaires pour une page de données particulière. Heureusement, Microsoft SQL Server 2005 fournit un nouveau mot clé pour le classement des résultats, qui nous permet d’écrire une requête capable de récupérer efficacement le sous-ensemble approprié d’enregistrements. Dans ce didacticiel, nous allons voir comment utiliser ce nouveau mot clé SQL Server 2005 pour implémenter la pagination personnalisée dans un contrôle GridView. Tandis que l’interface utilisateur pour la pagination personnalisée est identique à celle de la pagination par défaut, l’exécution pas à pas d’une page à la suivante à l’aide de la pagination personnalisée peut être plus rapide que la pagination par défaut.

> [!NOTE]
> Le gain de performance exact présenté par la pagination personnalisée dépend du nombre total d’enregistrements paginés et de la charge placée sur le serveur de base de données. À la fin de ce didacticiel, nous examinerons des mesures approximatives qui illustrent les avantages des performances obtenues par la pagination personnalisée.

## <a name="step-1-understanding-the-custom-paging-process"></a>Étape 1 : comprendre le processus de pagination personnalisé

Lors de la pagination des données, les enregistrements précis affichés dans une page varient en fonction de la page de données demandée et du nombre d’enregistrements affichés par page. Par exemple, imaginons que nous souhaitions parcourir les produits 81, en affichant 10 produits par page. Lorsque vous affichez la première page, nous souhaitons les produits 1 à 10 ; lors de l’affichage de la deuxième page, nous sommes intéressés par les produits 11 à 20, et ainsi de suite.

Trois variables déterminent les enregistrements qui doivent être récupérés et la façon dont l’interface de pagination doit être rendue :

- **Début de la ligne index** index de la première ligne de la page de données à afficher ; Cet index peut être calculé en multipliant l’index de la page par les enregistrements à afficher par page et en en ajoutant un. Par exemple, lors de la pagination d’un enregistrement 10 à la fois, pour la première page (dont l’index est 0), l’index de la ligne de début est 0 \* 10 + 1 ou 1 ; pour la deuxième page (dont l’index de page est 1), l’index de début de ligne est 1 \* 10 + 1 ou 11.
- Nombre **maximal de lignes** nombre maximal d’enregistrements à afficher par page. Cette variable est appelée lignes maximum, car pour la dernière page, il peut y avoir moins d’enregistrements retournés que la taille de la page. Par exemple, lors de la pagination des 81 produits 10 enregistrements par page, les neuvième et dernière pages ne comporteront qu’un seul enregistrement. Cependant, aucune page n’affiche plus d’enregistrements que la valeur nombre maximal de lignes.
- Nombre **total** d’enregistrements nombre total d’enregistrements paginés. Bien que cette variable ne soit pas nécessaire pour déterminer les enregistrements à récupérer pour une page donnée, elle détermine l’interface de pagination. Par exemple, si 81 produits sont paginés, l’interface de pagination sait qu’elle affiche neuf numéros de page dans l’interface utilisateur de pagination.

Avec la pagination par défaut, l’index de début de ligne est calculé en tant que produit de l’index de page et de la taille de page plus un, tandis que le nombre maximal de lignes est simplement la taille de la page. Étant donné que la pagination par défaut récupère tous les enregistrements de la base de données lors du rendu d’une page de données, l’index de chaque ligne est connu, ce qui rend le déplacement vers la ligne d’index de ligne de début une tâche trivial. En outre, le nombre total d’enregistrements est facilement disponible, car il s’agit simplement du nombre d’enregistrements dans le DataTable (ou de tout objet utilisé pour contenir les résultats de la base de données).

Étant donné les variables index de début et nombre maximal de lignes, une implémentation personnalisée de la pagination doit retourner uniquement le sous-ensemble précis des enregistrements commençant à l’index de début de ligne et au maximum de lignes nombre d’enregistrements après cela. La pagination personnalisée présente deux défis :

- Nous devons être en mesure d’associer efficacement un index de ligne à chaque ligne de la totalité des données paginées afin que nous puissions commencer à retourner des enregistrements à l’index de ligne de début spécifié.
- Nous devons fournir le nombre total d’enregistrements paginés.

Dans les deux étapes suivantes, nous examinerons le script SQL nécessaire pour répondre à ces deux défis. En plus du script SQL, nous devons également implémenter des méthodes dans la couche DAL et la couche BLL.

## <a name="step-2-returning-the-total-number-of-records-being-paged-through"></a>Étape 2 : retour du nombre total d’enregistrements paginés

Avant d’examiner comment récupérer le sous-ensemble précis d’enregistrements pour la page affichée, voyons d’abord comment renvoyer le nombre total d’enregistrements paginés. Ces informations sont nécessaires pour configurer correctement l’interface utilisateur de pagination. Le nombre total d’enregistrements renvoyés par une requête SQL particulière peut être obtenu à l’aide de la [fonction d’agrégation`COUNT`](https://msdn.microsoft.com/library/ms175997.aspx). Par exemple, pour déterminer le nombre total d’enregistrements dans la table `Products`, nous pouvons utiliser la requête suivante :

[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample1.sql)]

Nous allons ajouter une méthode à la couche DAL qui retourne ces informations. En particulier, nous allons créer une méthode DAL appelée `TotalNumberOfProducts()` qui exécute l’instruction `SELECT` présentée ci-dessus.

Commencez par ouvrir le `Northwind.xsd` fichier de DataSet typé dans le dossier `App_Code/DAL`. Ensuite, cliquez avec le bouton droit sur l' `ProductsTableAdapter` dans le concepteur, puis choisissez Ajouter une requête. Comme nous l’avons vu dans les didacticiels précédents, cela nous permet d’ajouter une nouvelle méthode à la couche DAL qui, lorsqu’elle est appelée, exécutera une instruction SQL ou une procédure stockée particulière. Comme avec nos méthodes TableAdapter dans les didacticiels précédents, pour cela, optez pour l’utilisation d’une instruction SQL ad hoc.

![Utiliser une instruction SQL ad hoc](efficiently-paging-through-large-amounts-of-data-cs/_static/image1.png)

**Figure 1**: utilisation d’une instruction SQL ad hoc

Dans l’écran suivant, nous pouvons spécifier le type de requête à créer. Étant donné que cette requête retourne une valeur scalaire unique, le nombre total d’enregistrements dans la table `Products` choisissez le `SELECT` qui retourne une option de valeur unique.

![Configurer la requête pour utiliser une instruction SELECT qui retourne une seule valeur](efficiently-paging-through-large-amounts-of-data-cs/_static/image2.png)

**Figure 2**: configurer la requête de façon à utiliser une instruction SELECT qui retourne une valeur unique

Une fois que vous avez indiqué le type de requête à utiliser, vous devez spécifier la requête suivante.

![Utiliser la requête SELECT COUNT (*) FROM Products](efficiently-paging-through-large-amounts-of-data-cs/_static/image3.png)

**Figure 3**: utiliser la requête SELECT COUNT (\*) from Products

Enfin, spécifiez le nom de la méthode. Comme nous l’avons mentionné plus haut, utilisez `TotalNumberOfProducts`.

![Nommer la méthode DAL TotalNumberOfProducts](efficiently-paging-through-large-amounts-of-data-cs/_static/image4.png)

**Figure 4**: nommer la méthode dal TotalNumberOfProducts

Une fois que vous avez cliqué sur terminer, l’Assistant ajoute la méthode `TotalNumberOfProducts` à la couche DAL. Les méthodes de retour scalaires dans la couche DAL retournent des types Nullable, si le résultat de la requête SQL est `NULL`. La requête `COUNT`, cependant, retourne toujours une valeur non`NULL` ; quelle que soit la méthode, la méthode DAL retourne un entier Nullable.

En plus de la méthode DAL, nous avons également besoin d’une méthode dans la couche BLL. Ouvrez le fichier de classe `ProductsBLL` et ajoutez une méthode `TotalNumberOfProducts` qui appelle simplement la méthode DAL s `TotalNumberOfProducts` :

[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample2.cs)]

La méthode `TotalNumberOfProducts` DAL retourne un entier Nullable. Toutefois, nous avons créé la méthode `ProductsBLL` Class s `TotalNumberOfProducts` afin qu’elle retourne un entier standard. Par conséquent, nous devons faire en sorte que la méthode `ProductsBLL` classe s `TotalNumberOfProducts` retourne la partie valeur de l’entier Nullable retourné par la méthode `TotalNumberOfProducts` DAL. L’appel à `GetValueOrDefault()` retourne la valeur de l’entier Nullable, s’il existe ; Toutefois, si l’entier Nullable est `null`, il retourne la valeur entière par défaut, 0.

## <a name="step-3-returning-the-precise-subset-of-records"></a>Étape 3 : retour du sous-ensemble précis des enregistrements

La tâche suivante consiste à créer des méthodes dans la couche DAL et la couche BLL qui acceptent les variables index de début et nombre maximal de lignes abordées précédemment et retournent les enregistrements appropriés. Avant cela, commençons par examiner le script SQL nécessaire. Le défi auquel nous sommes confrontés est que nous devons être en mesure d’affecter efficacement un index à chaque ligne dans l’ensemble des résultats paginés pour que nous puissions retourner uniquement les enregistrements commençant à l’index de début de ligne (et jusqu’au nombre maximal d’enregistrements).

Ce n’est pas un défi s’il existe déjà une colonne dans la table de base de données qui sert d’index de ligne. À première vue, nous pourrions penser que le champ `Products` de la table s `ProductID` peut suffire, étant donné que le premier produit a `ProductID` 1, le deuxième a 2, et ainsi de suite. Toutefois, la suppression d’un produit laisse un intervalle dans la séquence, annulant cette approche.

Il existe deux techniques générales utilisées pour associer efficacement un index de ligne avec les données à la page, ce qui permet de récupérer le sous-ensemble précis des enregistrements :

- **À l’aide de SQL Server 2005 s `ROW_NUMBER()` mot clé** new en SQL Server 2005, le mot clé `ROW_NUMBER()` associe un classement à chaque enregistrement retourné selon un ordre donné. Ce classement peut être utilisé comme index de ligne pour chaque ligne.
- **Utilisation d’une variable de table et d' `SET ROWCOUNT`** L’instruction SQL Server s [`SET ROWCOUNT`](https://msdn.microsoft.com/library/ms188774.aspx) peut être utilisée pour spécifier le nombre total d’enregistrements qu’une requête doit traiter avant de s’arrêter. les [variables de table](http://www.sqlteam.com/item.asp?ItemID=9454) sont des variables T-SQL locales qui peuvent contenir des données tabulaires, comme les [tables temporaires](http://www.sqlteam.com/item.asp?ItemID=2029). Cette approche fonctionne également bien avec Microsoft SQL Server 2005 et SQL Server 2000 (tandis que l’approche `ROW_NUMBER()` fonctionne uniquement avec SQL Server 2005).  
  
  L’idée est de créer une variable de table qui a une colonne `IDENTITY` et des colonnes pour les clés primaires de la table dont les données sont paginées. Ensuite, le contenu de la table dont les données sont paginées via est vidé dans la variable de table, associant ainsi un index de ligne séquentiel (via la colonne `IDENTITY`) pour chaque enregistrement de la table. Une fois la variable de table remplie, une instruction `SELECT` sur la variable de table, jointe à la table sous-jacente, peut être exécutée pour extraire les enregistrements particuliers. L’instruction `SET ROWCOUNT` est utilisée pour limiter intelligemment le nombre d’enregistrements qui doivent être vidés dans la variable de table.  
  
  L’efficacité de cette approche est basée sur le nombre de pages demandées, car la valeur de l’index de début de ligne et le nombre maximal de lignes sont affectées à `SET ROWCOUNT` valeur. Lors de la pagination sur des pages de faible numérotation, telles que les premières pages de données, cette approche est très efficace. Toutefois, elle présente des performances de type pagination par défaut lors de la récupération d’une page près de la fin.

Ce didacticiel met en œuvre la pagination personnalisée à l’aide du mot clé `ROW_NUMBER()`. Pour plus d’informations sur l’utilisation de la variable de table et `SET ROWCOUNT` technique, consultez [une méthode plus efficace pour la pagination à l’aide de grands jeux de résultats](http://www.4guysfromrolla.com/webtech/042606-1.shtml).

Le mot clé `ROW_NUMBER()` a associé un classement à chaque enregistrement retourné sur un classement particulier à l’aide de la syntaxe suivante :

[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample3.sql)]

`ROW_NUMBER()` retourne une valeur numérique qui spécifie le classement de chaque enregistrement en ce qui concerne l’ordre indiqué. Par exemple, pour voir le classement de chaque produit, classé du plus onéreux au moins, nous pourrions utiliser la requête suivante :

[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample4.sql)]

La figure 5 montre les résultats de cette requête lorsqu’elle est exécutée par le biais de la fenêtre de requête dans Visual Studio. Notez que les produits sont classés par prix, ainsi qu’un classement de prix pour chaque ligne.

![Le rang du prix est inclus pour chaque enregistrement retourné](efficiently-paging-through-large-amounts-of-data-cs/_static/image5.png)

**Figure 5**: le classement du prix est inclus pour chaque enregistrement retourné

> [!NOTE]
> `ROW_NUMBER()` n’est qu’une des nombreuses fonctions de classement disponibles dans SQL Server 2005. Pour une discussion plus approfondie sur les `ROW_NUMBER()`, ainsi que sur les autres fonctions de classement, lisez [renvoi des résultats classés avec Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml).

Lors du classement des résultats par la colonne de `ORDER BY` spécifiée dans la clause `OVER` (`UnitPrice`, dans l’exemple ci-dessus), SQL Server doit trier les résultats. Il s’agit d’une opération rapide s’il existe un index cluster sur la ou les colonnes dans lesquelles les résultats sont triés, ou s’il existe un index de couverture, mais peut être plus coûteux dans le cas contraire. Pour améliorer les performances des requêtes suffisamment volumineuses, envisagez d’ajouter un index non cluster pour la colonne par laquelle les résultats sont triés. Pour plus d’informations sur les performances, consultez [classement des fonctions et des performances dans SQL Server 2005](http://www.sql-server-performance.com/ak_ranking_functions.asp) .

Les informations de classement retournées par `ROW_NUMBER()` ne peuvent pas être utilisées directement dans la clause `WHERE`. Toutefois, une table dérivée peut être utilisée pour retourner le résultat `ROW_NUMBER()`, qui peut ensuite apparaître dans la clause `WHERE`. Par exemple, la requête ci-dessous utilise une table dérivée pour retourner les colonnes ProductName et UnitPrice, ainsi que le résultat `ROW_NUMBER()`, puis utilise une clause `WHERE` pour retourner uniquement les produits dont le rang du prix est compris entre 11 et 20 :

[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample5.sql)]

En étendant ce concept un peu plus loin, nous pouvons utiliser cette approche pour récupérer une page de données spécifique en fonction de l’index de la ligne de début souhaité et des valeurs de lignes au maximum :

[!code-html[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample6.html)]

> [!NOTE]
> Comme nous le verrons plus loin dans ce didacticiel, les *`StartRowIndex`* fournies par ObjectDataSource sont indexées à partir de zéro, tandis que la valeur `ROW_NUMBER()` retournée par SQL Server 2005 est indexée à partir de 1. Par conséquent, la clause `WHERE` retourne les enregistrements dans lesquels `PriceRank` est strictement supérieur à *`StartRowIndex`* et inférieur ou égal à`StartRowIndex` * + `MaximumRows`.*

Maintenant que nous avons abordé la manière dont `ROW_NUMBER()` peut être utilisé pour récupérer une page de données particulière en fonction des valeurs d’index de début de ligne et de nombre maximal de lignes, nous devons maintenant implémenter cette logique en tant que méthodes dans la couche DAL et la couche BLL.

Lors de la création de cette requête, nous devons déterminer l’ordre selon lequel les résultats seront classés. les produits sont triés par nom dans l’ordre alphabétique. Cela signifie qu’avec l’implémentation personnalisée de la pagination dans ce didacticiel, nous ne pourrons pas créer un rapport paginé personnalisé que ce qui peut également être trié. Dans le didacticiel suivant, toutefois, nous allons voir comment ces fonctionnalités peuvent être fournies.

Dans la section précédente, nous avons créé la méthode DAL comme une instruction SQL ad hoc. Malheureusement, l’analyseur T-SQL dans Visual Studio utilisé par l’Assistant TableAdapter n’a pas la même syntaxe que la `OVER` la syntaxe utilisée par la fonction `ROW_NUMBER()`. Par conséquent, nous devons créer cette méthode DAL comme procédure stockée. Sélectionnez l’Explorateur de serveurs dans le menu Affichage (ou appuyez sur Ctrl + Alt + S), puis développez le nœud `NORTHWND.MDF`. Pour ajouter une nouvelle procédure stockée, cliquez avec le bouton droit sur le nœud procédures stockées et choisissez Ajouter une nouvelle procédure stockée (voir figure 6).

![Ajouter une nouvelle procédure stockée pour la pagination des produits](efficiently-paging-through-large-amounts-of-data-cs/_static/image6.png)

**Figure 6**: ajouter une nouvelle procédure stockée pour la pagination des produits

Cette procédure stockée doit accepter deux paramètres d’entrée d’entier : `@startRowIndex` et `@maximumRows` et utiliser la fonction `ROW_NUMBER()` ordonnée par le champ `ProductName`, en ne retournant que les lignes supérieures à la `@startRowIndex` spécifiée et inférieure ou égale à `@startRowIndex` + s.`@maximumRow` Entrez le script suivant dans la nouvelle procédure stockée, puis cliquez sur l’icône Enregistrer pour ajouter la procédure stockée à la base de données.

[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample7.sql)]

Après avoir créé la procédure stockée, prenez un moment pour la tester. Cliquez avec le bouton droit sur le nom de la procédure stockée `GetProductsPaged` dans le Explorateur de serveurs et choisissez l’option Exécuter. Visual Studio vous invite ensuite à entrer les paramètres d’entrée, `@startRowIndex` et `@maximumRow` s (voir la figure 7). Essayez différentes valeurs et examinez les résultats.

![Entrez une valeur pour les paramètres @startRowIndex et @maximumRows](efficiently-paging-through-large-amounts-of-data-cs/_static/image7.png)

<strong>Figure 7</strong>: entrer une valeur pour les paramètres @startRowIndex et @maximumRows

Une fois que vous avez choisi ces valeurs de paramètres d’entrée, la fenêtre sortie affiche les résultats. La figure 8 illustre les résultats lors du passage de 10 pour les paramètres `@startRowIndex` et `@maximumRows`.

[![les enregistrements qui apparaîtraient dans la deuxième page de données sont retournés](efficiently-paging-through-large-amounts-of-data-cs/_static/image9.png)](efficiently-paging-through-large-amounts-of-data-cs/_static/image8.png)

**Figure 8**: les enregistrements qui apparaîtraient dans la deuxième page de données sont retournés ([cliquez pour afficher l’image en taille réelle](efficiently-paging-through-large-amounts-of-data-cs/_static/image10.png))

Une fois cette procédure stockée créée, nous sommes prêts à créer la méthode `ProductsTableAdapter`. Ouvrez le jeu de données typé `Northwind.xsd`, cliquez avec le bouton droit dans le `ProductsTableAdapter`, puis choisissez l’option Ajouter une requête. Au lieu de créer la requête à l’aide d’une instruction SQL ad hoc, créez-la à l’aide d’une procédure stockée existante.

![Créer la méthode DAL à l’aide d’une procédure stockée existante](efficiently-paging-through-large-amounts-of-data-cs/_static/image11.png)

**Figure 9**: créer la méthode dal à l’aide d’une procédure stockée existante

Ensuite, nous sommes invités à sélectionner la procédure stockée à appeler. Sélectionnez la procédure stockée `GetProductsPaged` dans la liste déroulante.

![Choisissez la procédure stockée GetProductsPaged dans la liste déroulante.](efficiently-paging-through-large-amounts-of-data-cs/_static/image12.png)

**Figure 10**: choisir la procédure stockée GetProductsPaged dans la liste déroulante

L’écran suivant vous demande ensuite le type de données retournées par la procédure stockée : données tabulaires, une valeur unique ou aucune valeur. Étant donné que la procédure stockée `GetProductsPaged` peut retourner plusieurs enregistrements, indiquez qu’elle retourne des données tabulaires.

![Indiquer que la procédure stockée retourne des données tabulaires](efficiently-paging-through-large-amounts-of-data-cs/_static/image13.png)

**Figure 11**: indiquer que la procédure stockée retourne des données tabulaires

Enfin, indiquez les noms des méthodes que vous souhaitez créer. Comme pour les didacticiels précédents, vous allez créer des méthodes en utilisant à la fois le remplissage d’un DataTable et le renvoi d’un DataTable. Nommez la première méthode `FillPaged` et la deuxième `GetProductsPaged`.

![Nommez les méthodes FillPaged et GetProductsPaged](efficiently-paging-through-large-amounts-of-data-cs/_static/image14.png)

**Figure 12**: nommer les méthodes FillPaged et GetProductsPaged

En plus de créer une méthode DAL pour retourner une page de produits particulière, nous devons également fournir ce type de fonctionnalité dans la couche BLL. À l’instar de la méthode DAL, la méthode BLL s GetProductsPaged doit accepter deux entrées entières pour spécifier l’index de début de ligne et le nombre maximal de lignes, et doit renvoyer uniquement les enregistrements qui se trouvent dans la plage spécifiée. Créez une telle méthode BLL dans la classe ProductsBLL qui appelle simplement la méthode DAL s GetProductsPaged, comme suit :

[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample8.cs)]

Vous pouvez utiliser n’importe quel nom pour les paramètres d’entrée de la méthode BLL, mais, comme nous le verrons bientôt, le choix de l’utilisation de `startRowIndex` et `maximumRows` nous fait gagner un peu de travail lors de la configuration d’un ObjectDataSource pour utiliser cette méthode.

## <a name="step-4-configuring-the-objectdatasource-to-use-custom-paging"></a>Étape 4 : configuration de l’ObjectDataSource pour utiliser la pagination personnalisée

Avec les méthodes BLL et DAL pour accéder à un sous-ensemble particulier d’enregistrements, nous sommes prêts à créer un contrôle GridView qui pagine ses enregistrements sous-jacents à l’aide d’une pagination personnalisée. Commencez par ouvrir la page `EfficientPaging.aspx` dans le dossier `PagingAndSorting`, ajoutez un GridView à la page et configurez-le pour qu’il utilise un nouveau contrôle ObjectDataSource. Dans les didacticiels précédents, l’ObjectDataSource a souvent été configuré pour utiliser la méthode de `GetProducts` de la classe `ProductsBLL`. Cette fois, cependant, nous souhaitons utiliser la méthode `GetProductsPaged`, car la méthode `GetProducts` retourne *tous* les produits de la base de données tandis que `GetProductsPaged` retourne simplement un sous-ensemble particulier d’enregistrements.

![Configurer ObjectDataSource pour utiliser la méthode ProductsBLL de la classe s GetProductsPaged](efficiently-paging-through-large-amounts-of-data-cs/_static/image15.png)

**Figure 13**: configurer ObjectDataSource pour utiliser la méthode ProductsBLL de la classe s GetProductsPaged

Étant donné que nous avons recréé un GridView en lecture seule, prenez un moment pour définir la liste déroulante des méthodes dans les onglets insertion, mise à jour et suppression sur (aucune).

Ensuite, l’Assistant ObjectDataSource nous invite à entrer les sources des `GetProductsPaged` méthode s `startRowIndex` et `maximumRows` valeurs des paramètres d’entrée. Ces paramètres d’entrée sont définis automatiquement par le contrôle GridView. par conséquent, il vous suffit de conserver la source définie sur aucun et de cliquer sur Terminer.

![Conserver les sources du paramètre d’entrée sur aucun](efficiently-paging-through-large-amounts-of-data-cs/_static/image16.png)

**Figure 14**: conserver les sources des paramètres d’entrée sur aucun

Une fois l’exécution de l’Assistant ObjectDataSource terminée, le contrôle GridView contient un BoundField ou un CheckBoxField pour chacun des champs de données de produit. N’hésitez pas à personnaliser l’apparence de GridView s comme vous le souhaitez. J’ai choisi d’afficher uniquement les `ProductName`, `CategoryName`, `SupplierName`, `QuantityPerUnit`et `UnitPrice` BoundFields. En outre, configurez le contrôle GridView pour prendre en charge la pagination en activant la case à cocher Activer la pagination dans sa balise active. Une fois ces modifications effectuées, les balises déclaratives GridView et ObjectDataSource doivent ressembler à ce qui suit :

[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample9.aspx)]

Toutefois, si vous visitez la page à l’aide d’un navigateur, le GridView n’est pas trouvé.

![Le contrôle GridView n’est pas affiché](efficiently-paging-through-large-amounts-of-data-cs/_static/image17.png)

**Figure 15**: le contrôle GridView n’est pas affiché

Le GridView est manquant, car ObjectDataSource utilise actuellement 0 comme valeurs pour les deux `GetProductsPaged` `startRowIndex` et `maximumRows` paramètres d’entrée. Par conséquent, la requête SQL obtenue ne retourne aucun enregistrement et, par conséquent, le contrôle GridView n’est pas affiché.

Pour y remédier, nous devons configurer ObjectDataSource pour utiliser la pagination personnalisée. Pour ce faire, procédez comme suit :

1. **Définissez la propriété ObjectDataSource s `EnablePaging` sur `true`** indique à l’ObjectDataSource qu’il doit passer à l' `SelectMethod` deux paramètres supplémentaires : un pour spécifier l’index de ligne de début ([`StartRowIndexParameterName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.startrowindexparametername.aspx)) et un pour spécifier le nombre maximal de lignes ([`MaximumRowsParameterName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.maximumrowsparametername.aspx)).
2. **Définissez les propriétés ObjectDataSource s `StartRowIndexParameterName` et `MaximumRowsParameterName` en conséquence** . les propriétés `StartRowIndexParameterName` et `MaximumRowsParameterName` indiquent les noms des paramètres d’entrée passés dans la `SelectMethod` à des fins de pagination personnalisée. Par défaut, ces noms de paramètres sont `startIndexRow` et `maximumRows`, c’est pourquoi, lors de la création de la méthode `GetProductsPaged` dans la couche BLL, j’ai utilisé ces valeurs pour les paramètres d’entrée. Si vous avez choisi d’utiliser des noms de paramètres différents pour la méthode BLL s `GetProductsPaged` comme `startIndex` et `maxRows`, par exemple, vous devez définir les propriétés ObjectDataSource s `StartRowIndexParameterName` et `MaximumRowsParameterName` en conséquence (comme startIndex pour `StartRowIndexParameterName` et maxRows pour `MaximumRowsParameterName`).
3. **Définissez la propriété ObjectDataSource s [`SelectCountMethod`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selectcountmethod(VS.80).aspx) sur le nom de la méthode qui retourne le nombre total d’enregistrements paginés (`TotalNumberOfProducts`)** . Rappelez-vous que la méthode `ProductsBLL` classe s `TotalNumberOfProducts` retourne le nombre total d’enregistrements paginés via une méthode dal qui exécute une requête `SELECT COUNT(*) FROM Products`. Ces informations sont nécessaires à ObjectDataSource pour restituer correctement l’interface de pagination.
4. **Supprimez le `startRowIndex` et `maximumRows` `<asp:Parameter>` éléments de la balise déclarative de ObjectDataSource s** lors de la configuration de l’ObjectDataSource via l’Assistant, Visual Studio a ajouté automatiquement deux éléments de `<asp:Parameter>` pour les paramètres d’entrée de la méthode `GetProductsPaged`. En définissant `EnablePaging` sur `true`, ces paramètres sont transmis automatiquement. s’ils apparaissent également dans la syntaxe déclarative, ObjectDataSource tente de passer *quatre* paramètres à la méthode `GetProductsPaged` et deux paramètres à la méthode `TotalNumberOfProducts`. Si vous oubliez de supprimer ces `<asp:Parameter>` éléments, lors de la visite de la page via un navigateur, vous obtiendrez un message d’erreur tel que : *ObjectDataSource’ObjectDataSource1 'n’a pas pu trouver une méthode non générique’TotalNumberOfProducts’qui a des paramètres : StartRowIndex, MaximumRows*.

Une fois ces modifications apportées, la syntaxe déclarative de ObjectDataSource s doit ressembler à ce qui suit :

[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample10.aspx)]

Notez que les propriétés `EnablePaging` et `SelectCountMethod` ont été définies et que les éléments `<asp:Parameter>` ont été supprimés. La figure 16 illustre une capture d’écran du Fenêtre Propriétés une fois que ces modifications ont été apportées.

![Pour utiliser la pagination personnalisée, configurez le contrôle ObjectDataSource](efficiently-paging-through-large-amounts-of-data-cs/_static/image18.png)

**Figure 16**: pour utiliser la pagination personnalisée, configurez le contrôle ObjectDataSource

Après avoir apporté ces modifications, visitez cette page via un navigateur. Vous devez voir 10 produits répertoriés, classés par ordre alphabétique. Prenez un moment pour parcourir les données une page à la fois. Bien qu’il n’y ait pas de différence visuelle entre la perspective de l’utilisateur final entre la pagination par défaut et la pagination personnalisée, la pagination personnalisée rend les pages plus efficaces par le biais de grandes quantités de données, car elle récupère uniquement les enregistrements qui doivent être affichés pour une page donnée.

[![les données, classées par le nom du produit, sont paginées à l’aide d’une pagination personnalisée](efficiently-paging-through-large-amounts-of-data-cs/_static/image20.png)](efficiently-paging-through-large-amounts-of-data-cs/_static/image19.png)

**Figure 17**: les données, classées par le nom du produit, sont paginées à l’aide d’une pagination personnalisée ([cliquez pour afficher l’image en taille réelle](efficiently-paging-through-large-amounts-of-data-cs/_static/image21.png))

> [!NOTE]
> Avec la pagination personnalisée, la valeur du nombre de pages retournée par l' `SelectCountMethod` ObjectDataSource est stockée dans l’état d’affichage GridView s. Autres variables GridView les `PageIndex`, `EditIndex`, `SelectedIndex`, `DataKeys` collection, etc. sont stockés dans l' *État du contrôle*, qui est conservé, quelle que soit la valeur de la propriété GridView s `EnableViewState`. Étant donné que la valeur `PageCount` est persistante entre les publications (postback) à l’aide de l’état d’affichage, lors de l’utilisation d’une interface de pagination qui comprend un lien pour vous connecter à la dernière page, il est impératif que l’état d’affichage de GridView s soit activé. (Si votre interface de pagination n’inclut pas de lien direct vers la dernière page, vous pouvez désactiver l’état d’affichage.)

Le fait de cliquer sur le lien de la dernière page provoque une publication et indique au GridView de mettre à jour sa propriété `PageIndex`. Si l’utilisateur clique sur le lien de la dernière page, le contrôle GridView attribue à sa propriété `PageIndex` une valeur inférieure à sa propriété `PageCount`. Lorsque l’état d’affichage est désactivé, la valeur `PageCount` est perdue entre les publications et la valeur entière maximale est assignée au `PageIndex`. Ensuite, le contrôle GridView tente de déterminer l’index de début de ligne en multipliant les propriétés `PageSize` et `PageCount`. Cela aboutit à une `OverflowException` puisque le produit dépasse la taille maximale autorisée pour les entiers.

## <a name="implement-custom-paging-and-sorting"></a>Implémenter la pagination et le tri personnalisés

Notre implémentation de pagination personnalisée actuelle exige que l’ordre par lequel les données sont paginées soit spécifié de manière statique lors de la création de la `GetProductsPaged` procédure stockée. Toutefois, vous avez peut-être remarqué que la balise active de GridView s contient une case à cocher Activer le tri en plus de l’option Activer la pagination. Malheureusement, l’ajout de la prise en charge du tri au contrôle GridView avec notre implémentation de pagination personnalisée actuelle ne triera que les enregistrements sur la page de données actuellement affichée. Par exemple, si vous configurez le contrôle GridView pour prendre également en charge la pagination, puis, lors de l’affichage de la première page de données, triez par nom de produit dans l’ordre décroissant, l’ordre des produits sur la page 1 est inversée. Comme le montre la figure 18, ce qui indique que Carnarvon tigres est le premier produit lors du tri dans l’ordre alphabétique inversé, qui ignore les 71 autres produits qui viennent après Carnarvon tigres, par ordre alphabétique ; Seuls les enregistrements de la première page sont pris en compte dans le tri.

[![seules les données affichées sur la page actuelle sont triées](efficiently-paging-through-large-amounts-of-data-cs/_static/image23.png)](efficiently-paging-through-large-amounts-of-data-cs/_static/image22.png)

**Figure 18**: seules les données affichées sur la page actuelle sont triées ([cliquez pour afficher l’image en taille réelle](efficiently-paging-through-large-amounts-of-data-cs/_static/image24.png))

Le tri s’applique uniquement à la page de données actuelle, car le tri se produit une fois que les données ont été récupérées de la méthode de `GetProductsPaged` BLL s, et cette méthode retourne uniquement les enregistrements pour la page spécifique. Pour implémenter correctement le tri, nous devons transmettre l’expression de tri à la méthode `GetProductsPaged` afin que les données puissent être classées de manière appropriée avant de retourner la page de données spécifique. Nous verrons comment effectuer cette procédure dans le prochain didacticiel.

## <a name="implementing-custom-paging-and-deleting"></a>Implémentation de la pagination et de la suppression personnalisées

Si vous activez la fonctionnalité de suppression dans un contrôle GridView dont les données sont paginées à l’aide de techniques de pagination personnalisées, vous constaterez que lors de la suppression du dernier enregistrement de la dernière page, le contrôle GridView disparaît au lieu de décrémenter de manière appropriée les `PageIndex`GridView. Pour reproduire ce bogue, activez la suppression du didacticiel que nous venons de créer. Accédez à la dernière page (page 9), où vous devriez voir un produit unique puisque nous effectuons la pagination de 81 produits, 10 produits à la fois. Supprimer ce produit.

Lors de la suppression du dernier produit, le contrôle GridView *doit* automatiquement passer à la huitième page et cette fonctionnalité est exposée avec la pagination par défaut. Toutefois, avec la pagination personnalisée, après la suppression de ce dernier produit sur la dernière page, le GridView disparaît simplement de l’écran. La raison exacte *pour laquelle* cela se produit est un peu au-delà de la portée de ce didacticiel. consultez [suppression du dernier enregistrement sur la dernière page d’un GridView avec pagination personnalisée](http://scottonwriting.net/sowblog/posts/7326.aspx) pour obtenir des détails de bas niveau sur la source de ce problème. En résumé, il s’agit de la séquence suivante d’étapes qui sont effectuées par le contrôle GridView lorsque l’utilisateur clique sur le bouton supprimer :

1. Supprimer l’enregistrement
2. Obtenir les enregistrements appropriés à afficher pour le `PageIndex` spécifié et `PageSize`
3. Vérifiez que le `PageIndex` ne dépasse pas le nombre de pages de données dans la source de données ; Si c’est le cas, décrémente automatiquement la propriété de `PageIndex` GridView s
4. Lier la page de données appropriée au GridView à l’aide des enregistrements obtenus à l’étape 2

Le problème provient du fait que, à l’étape 2, le `PageIndex` utilisé lors de la saisie des enregistrements à afficher est toujours le `PageIndex` de la dernière page dont l’enregistrement unique vient d’être supprimé. Par conséquent, à l’étape 2, *aucun* enregistrement n’est retourné, car cette dernière page de données ne contient plus d’enregistrements. Ensuite, à l’étape 3, le contrôle GridView se rend compte que sa propriété `PageIndex` est supérieure au nombre total de pages dans la source de données (puisque nous avons supprimé le dernier enregistrement dans la dernière page) et décrémente donc sa propriété `PageIndex`. À l’étape 4, le GridView tente de se lier aux données récupérées à l’étape 2. Toutefois, à l’étape 2, aucun enregistrement n’a été retourné, ce qui aboutit à un GridView vide. Avec la pagination par défaut, ce problème ne se surface pas, car à l’étape 2, *tous les* enregistrements sont récupérés à partir de la source de données.

Pour résoudre ce problème, nous avons deux options. La première consiste à créer un gestionnaire d’événements pour le gestionnaire d’événements `RowDeleted` GridView s qui détermine le nombre d’enregistrements affichés dans la page qui vient d’être supprimée. S’il n’y avait qu’un seul enregistrement, l’enregistrement que vous venez de supprimer doit avoir été le dernier et nous devons décrémenter le `PageIndex`GridView s. Bien entendu, nous voulons uniquement mettre à jour le `PageIndex` si l’opération de suppression a réussi, ce qui peut être déterminé en s’assurant que la propriété `e.Exception` est `null`.

Cette approche fonctionne, car elle met à jour le `PageIndex` après l’étape 1, mais avant l’étape 2. Par conséquent, à l’étape 2, le jeu d’enregistrements approprié est retourné. Pour ce faire, utilisez un code similaire à ce qui suit :

[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample11.cs)]

Une autre solution consiste à créer un gestionnaire d’événements pour l’événement ObjectDataSource s `RowDeleted` et à affecter à la propriété `AffectedRows` la valeur 1. Après la suppression de l’enregistrement à l’étape 1 (mais avant la récupération des données à l’étape 2), le contrôle GridView met à jour sa propriété `PageIndex` si une ou plusieurs lignes sont affectées par l’opération. Toutefois, la propriété `AffectedRows` n’est pas définie par l’ObjectDataSource et, par conséquent, cette étape est omise. Une façon d’exécuter cette étape consiste à définir manuellement la propriété `AffectedRows` si l’opération de suppression se termine correctement. Cela peut être accompli à l’aide d’un code semblable au suivant :

[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample12.cs)]

Vous trouverez le code de ces deux gestionnaires d’événements dans la classe code-behind de l’exemple de `EfficientPaging.aspx`.

## <a name="comparing-the-performance-of-default-and-custom-paging"></a>Comparaison des performances de la pagination par défaut et personnalisée

Étant donné que la pagination personnalisée récupère uniquement les enregistrements nécessaires, tandis que la pagination par défaut retourne *tous* les enregistrements de chaque page affichée, il est clair que la pagination personnalisée est plus efficace que la pagination par défaut. Mais quel est le niveau de pagination personnalisé le plus efficace ? Quel type d’amélioration des performances peut être observé en passant de la pagination par défaut à la pagination personnalisée ?

Malheureusement, aucune taille n’est adaptée à toutes les réponses ici. Le gain de performances dépend d’un certain nombre de facteurs, les deux plus importants étant le nombre d’enregistrements paginés et la charge placée sur le serveur de base de données et les canaux de communication entre le serveur Web et le serveur de base de données. Pour les petites tables avec seulement quelques dizaines d’enregistrements, la différence de performances peut être négligeable. Toutefois, pour les tables volumineuses, avec des milliers de centaines de milliers de lignes, la différence de performances est aiguë.

Un article de mine, [pagination personnalisée dans ASP.NET 2,0 avec SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx), contient des tests de performances que j’ai exécutés pour présenter les différences de performances entre ces deux techniques de pagination lors de la pagination d’une table de base de données avec 50 000 enregistrements. Dans ces tests, j’ai examiné le temps d’exécution de la requête au niveau de la SQL Server (à l’aide du [Générateur de profils SQL](https://msdn.microsoft.com/library/ms173757.aspx)) et sur la page ASP.net à l’aide des [fonctionnalités de suivi de ASP.net s](https://msdn.microsoft.com/library/y13fw6we.aspx). N’oubliez pas que ces tests ont été exécutés sur mon cadre de développement avec un seul utilisateur actif et qu’ils ne sont donc pas scientifiques et ne imitent pas les modèles de charge de site Web standard. Quoi qu’il en soit, les résultats illustrent les différences relatives en termes de temps d’exécution pour la pagination par défaut et personnalisée lorsque vous travaillez avec suffisamment de données.

|  | **Durée moyenne (s)** | **Lectures** |
| --- | --- | --- |
| **Profileur SQL de pagination par défaut** | 1.411 | 383 |
| **Profileur SQL de pagination personnalisée** | 0.002 | 29 |
| **Suivi ASP.NET de pagination par défaut** | 2.379 | *N/A* |
| **Suivi de ASP.NET de pagination personnalisé** | 0.029 | *N/A* |

Comme vous pouvez le voir, la récupération d’une page de données particulière requiert 354 moins de lectures en moyenne et s’est terminée en une fraction du temps. Au niveau de la page ASP.NET, la page a pu s’afficher à une date proche<sup>de 1/100 après</sup> l’utilisation de la pagination par défaut. Pour plus d’informations sur ces résultats, consultez [mon article](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx) , ainsi que le code et une base de données que vous pouvez télécharger pour reproduire ces tests dans votre propre environnement.

## <a name="summary"></a>Récapitulatif

La pagination par défaut est un peu à implémenter. activez la case à cocher Activer la pagination dans la balise active des contrôles Web de données, mais cette simplicité est le coût des performances. Avec la pagination par défaut, lorsqu’un utilisateur demande une page de données, *tous les* enregistrements sont retournés, même si une fraction minuscule peut être affichée. Pour combattre cette surcharge de performances, l’ObjectDataSource offre une autre option de pagination personnalisée.

Bien que la pagination personnalisée améliore les problèmes de performances de pagination par défaut en extrayant uniquement les enregistrements qui doivent être affichés, elle est plus impliquée dans l’implémentation de la pagination personnalisée. Tout d’abord, une requête doit être écrite de manière à accéder correctement (et efficacement) au sous-ensemble spécifique d’enregistrements demandés. Cela peut être accompli de plusieurs façons ; celui que nous avons examiné dans ce didacticiel consiste à utiliser la nouvelle fonction `ROW_NUMBER()` de SQL Server 2005 s pour classer les résultats, puis à retourner uniquement les résultats dont le classement se situe dans une plage spécifiée. En outre, nous devons ajouter un moyen de déterminer le nombre total d’enregistrements paginés. Après avoir créé ces méthodes DAL et BLL, nous devons également configurer l’ObjectDataSource afin qu’il puisse déterminer le nombre total d’enregistrements paginés et peut transmettre correctement les valeurs d’index de ligne de début et de lignes au maximum à la couche BLL.

Tandis que l’implémentation de la pagination personnalisée nécessite un certain nombre d’étapes et n’est pas aussi simple que la pagination par défaut, la pagination personnalisée est une nécessité pour la pagination de quantités de données suffisamment volumineuses. Comme le montrent les résultats examinés, la pagination personnalisée peut être à l’heure de la sortie de la page ASP.NET et peut éclaircir la charge sur le serveur de base de données d’un ou de plusieurs ordres de grandeur.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Précédent](paging-and-sorting-report-data-cs.md)
> [Suivant](sorting-custom-paged-data-cs.md)
