---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs
title: Interrogation des données avec le contrôle SqlDataSourceC#() | Microsoft Docs
author: rick-anderson
description: Dans les didacticiels précédents, nous avons utilisé le contrôle ObjectDataSource pour séparer entièrement la couche de présentation de la couche d’accès aux données. À partir de ce...
ms.author: riande
ms.date: 02/20/2007
ms.assetid: 60512d6a-b572-4b7a-beb3-3e44b4d2020c
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 5bda42965f7d1db71b207c0b76e251b8fff64e31
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78626426"
---
# <a name="querying-data-with-the-sqldatasource-control-c"></a>Interrogation des données avec le contrôle SqlDataSource (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_47_CS.exe) ou [Télécharger le PDF](querying-data-with-the-sqldatasource-control-cs/_static/datatutorial47cs1.pdf)

> Dans les didacticiels précédents, nous avons utilisé le contrôle ObjectDataSource pour séparer entièrement la couche de présentation de la couche d’accès aux données. À partir de ce didacticiel, nous allons découvrir comment le contrôle SqlDataSource peut être utilisé pour les applications simples qui ne nécessitent pas une telle séparation stricte de la présentation et de l’accès aux données.

## <a name="introduction"></a>Introduction

Tous les didacticiels que nous avons examinés jusqu’à présent ont utilisé une architecture à plusieurs niveaux composée de couches de présentation, de logique métier et d’accès aux données. La couche DAL (Data Access Layer) a été créée dans le premier didacticiel ([création d’une couche d’accès aux données](../introduction/creating-a-data-access-layer-cs.md)) et la couche de logique métier dans la seconde ([création d’une couche de logique métier](../introduction/creating-a-business-logic-layer-cs.md)). À partir du didacticiel sur l' [affichage des données avec ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) , nous avons vu comment utiliser le nouveau contrôle objectdatasource d’ASP.NET 2,0 s pour interagir de manière déclarative avec l’architecture de la couche de présentation.

Bien que tous les didacticiels jusqu’à présent aient utilisé l’architecture pour travailler avec les données, il est également possible d’accéder, d’insérer, de mettre à jour et de supprimer des données de base de données directement à partir d’une page ASP.NET, en ignorant l’architecture. Cela place les requêtes de base de données et la logique métier spécifiques directement dans la page Web. Pour les applications suffisamment volumineuses ou complexes, la conception, l’implémentation et l’utilisation d’une architecture à plusieurs niveaux sont vitales pour la réussite, la mise à jour et la maintenabilité de l’application. Toutefois, le développement d’une architecture robuste peut ne pas être nécessaire lors de la création d’applications uniques et extrêmement simples.

ASP.NET 2,0 fournit cinq contrôles de source de données intégrés : [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx)et [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx). Le SqlDataSource peut être utilisé pour accéder et modifier des données directement à partir d’une base de données relationnelle, y compris Microsoft SQL Server, Microsoft Access, Oracle, MySQL, etc. Dans ce didacticiel et les trois suivants, nous allons examiner comment utiliser le contrôle SqlDataSource, comment interroger et filtrer les données de la base de données, ainsi que comment utiliser le SqlDataSource pour insérer, mettre à jour et supprimer des données.

![ASP.NET 2,0 comprend cinq contrôles de source de données intégrés.](querying-data-with-the-sqldatasource-control-cs/_static/image1.gif)

**Figure 1**: ASP.NET 2,0 comprend cinq contrôles de source de données intégrés

## <a name="comparing-the-objectdatasource-and-sqldatasource"></a>Comparaison des ObjectDataSource et SqlDataSource

Conceptuellement, les contrôles ObjectDataSource et SqlDataSource sont simplement des proxies de données. Comme indiqué dans le didacticiel [affichage des données avec ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) , l’ObjectDataSource possède des propriétés qui indiquent le type d’objet qui fournit les données et les méthodes à appeler pour sélectionner, insérer, mettre à jour et supprimer des données du type d’objet sous-jacent. Une fois que les propriétés ObjectDataSource s ont été configurées, un contrôle Web de données tel qu’un contrôle GridView, DetailsView ou DataList peut être lié au contrôle à l’aide de la `Select()`ObjectDataSource s, `Insert()`, `Delete()`et `Update()` méthodes pour interagir avec l’architecture sous-jacente.

Le SqlDataSource offre les mêmes fonctionnalités, mais fonctionne sur une base de données relationnelle plutôt que sur une bibliothèque d’objets. Avec le SqlDataSource, nous devons spécifier la chaîne de connexion de la base de données et les requêtes SQL ou les procédures stockées ad hoc à exécuter pour insérer, mettre à jour, supprimer et récupérer des données. Les méthodes SqlDataSource s `Select()`, `Insert()`, `Update()`et `Delete()`, lorsqu’elles sont appelées, se connectent à la base de données spécifiée et émettent la requête SQL appropriée. Comme le montre le diagramme suivant, ces méthodes effectuent le travail grunt pour se connecter à une base de données, émettre une requête et retourner les résultats.

![Le SqlDataSource sert de proxy à la base de données](querying-data-with-the-sqldatasource-control-cs/_static/image2.gif)

**Figure 2**: le SqlDataSource sert de proxy à la base de données

> [!NOTE]
> Dans ce didacticiel, nous allons nous concentrer sur la récupération des données de la base de données. Dans le didacticiel sur l' [insertion, la mise à jour et la suppression de données avec le contrôle SqlDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md) , nous verrons comment configurer le SqlDataSource pour prendre en charge l’insertion, la mise à jour et la suppression.

## <a name="the-sqldatasource-and-accessdatasource-controls"></a>Contrôles SqlDataSource et AccessDataSource

En plus du contrôle SqlDataSource, ASP.NET 2,0 comprend également un contrôle AccessDataSource. Ces deux contrôles différents conduisent de nombreux développeurs à ASP.NET 2,0 à soupçonner que le contrôle AccessDataSource est conçu pour fonctionner exclusivement avec Microsoft Access avec le contrôle SqlDataSource conçu pour fonctionner exclusivement avec Microsoft SQL Server. Si le contrôle AccessDataSource est conçu pour fonctionner spécifiquement avec Microsoft Access, le contrôle SqlDataSource fonctionne avec *n’importe quelle* base de données relationnelle accessible via .net. Cela comprend tous les magasins de données compatibles OleDb ou ODBC, tels que Microsoft SQL Server, Microsoft Access, Oracle, Informix, MySQL et PostgreSQL, entre autres.

La seule différence entre les contrôles AccessDataSource et SqlDataSource est la façon dont les informations de connexion à la base de données sont spécifiées. Le contrôle AccessDataSource a uniquement besoin du chemin d’accès au fichier de base de données Access. En revanche, le SqlDataSource requiert une chaîne de connexion complète.

## <a name="step-1-creating-the-sqldatasource-web-pages"></a>Étape 1 : création des pages Web SqlDataSource

Avant de commencer à explorer la manière de travailler directement avec les données de base de données à l’aide du contrôle SqlDataSource, commençons par prendre un moment pour créer les pages ASP.NET dans notre projet de site Web dont nous aurons besoin pour ce didacticiel et les trois prochaines. Commencez par ajouter un nouveau dossier nommé `SqlDataSource`. Ajoutez ensuite les pages ASP.NET suivantes à ce dossier, en veillant à associer chaque page à la page maître `Site.master` :

- `Default.aspx`
- `Querying.aspx`
- `ParameterizedQueries.aspx`
- `InsertUpdateDelete.aspx`
- `OptimisticConcurrency.aspx`

![Ajouter les pages ASP.NET pour les didacticiels liés à SqlDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image3.gif)

**Figure 3**: ajouter les pages ASP.net pour les didacticiels relatifs à SqlDataSource

Comme dans les autres dossiers, `Default.aspx` dans le dossier `SqlDataSource` répertorie les didacticiels de la section. N’oubliez pas que le contrôle utilisateur `SectionLevelTutorialListing.ascx` fournit cette fonctionnalité. Par conséquent, ajoutez ce contrôle utilisateur à `Default.aspx` en le faisant glisser de l’Explorateur de solutions sur la page s Mode Création.

[![ajouter le contrôle utilisateur SectionLevelTutorialListing. ascx à default. aspx](querying-data-with-the-sqldatasource-control-cs/_static/image5.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image4.gif)

**Figure 4**: ajouter le contrôle utilisateur `SectionLevelTutorialListing.ascx` à `Default.aspx` ([cliquez pour afficher l’image en taille réelle](querying-data-with-the-sqldatasource-control-cs/_static/image6.gif))

Enfin, ajoutez ces quatre pages en tant qu’entrées au fichier `Web.sitemap`. Plus précisément, ajoutez le balisage suivant après avoir ajouté les boutons personnalisés aux `<siteMapNode>`de contrôle DataList et Repeater :

[!code-sql[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample1.sql)]

Après avoir mis à jour `Web.sitemap`, prenez un moment pour afficher le site Web des didacticiels via un navigateur. Le menu à gauche contient maintenant des éléments pour les didacticiels de modification, d’insertion et de suppression.

![Le plan de site comprend désormais des entrées pour les didacticiels de SqlDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image7.gif)

**Figure 5**: le plan de site contient maintenant des entrées pour les didacticiels SqlDataSource

## <a name="step-2-adding-and-configuring-the-sqldatasource-control"></a>Étape 2 : ajout et configuration du contrôle SqlDataSource

Commencez par ouvrir la page `Querying.aspx` dans le dossier `SqlDataSource` et basculez vers Mode Création. Faites glisser un contrôle SqlDataSource de la boîte à outils vers le concepteur et affectez à son `ID` la valeur `ProductsDataSource`. Comme avec ObjectDataSource, le SqlDataSource ne produit pas de sortie rendue et apparaît donc sous la forme d’une zone grise sur l’aire de conception. Pour configurer le SqlDataSource, cliquez sur le lien configurer la source de données dans la balise active SqlDataSource s.

![Cliquez sur le lien configurer la source de données dans la balise active SqlDataSource s.](querying-data-with-the-sqldatasource-control-cs/_static/image8.gif)

**Figure 6**: cliquer sur le lien configurer la source de données dans la balise active SqlDataSource s

L’Assistant contrôle de la source de données s’affiche. Bien que les étapes de l’Assistant diffèrent des contrôles ObjectDataSource, l’objectif final est le même pour fournir des détails sur la récupération, l’insertion, la mise à jour et la suppression de données via la source de données. Pour le SqlDataSource, cela implique de spécifier la base de données sous-jacente à utiliser et de fournir les instructions SQL ou les procédures stockées ad hoc.

La première étape de l’Assistant nous invite à entrer la base de données. La liste déroulante comprend les bases de données qui se trouvent dans le dossier application Web s `App_Data` et celles qui ont été ajoutées au nœud Connexions de données dans la Explorateur de serveurs. Étant donné que nous avons déjà ajouté une chaîne de connexion pour la base de données `NORTHWIND.MDF` dans le dossier `App_Data` à notre fichier de `Web.config` de projet, la liste déroulante contient une référence à cette chaîne de connexion, `NORTHWINDConnectionString`. Choisissez cet élément dans la liste déroulante, puis cliquez sur suivant.

![Choisissez le NORTHWINDConnectionString dans la liste déroulante](querying-data-with-the-sqldatasource-control-cs/_static/image9.gif)

**Figure 7**: choisir la `NORTHWINDConnectionString` dans la liste déroulante

Après avoir choisi la base de données, l’Assistant vous demande de renvoyer les données. Nous pouvons soit spécifier les colonnes d’une table ou d’une vue à renvoyer, soit entrer une instruction SQL personnalisée ou spécifier une procédure stockée. Vous pouvez basculer entre ce choix à l’aide des cases d’option spécifier une instruction SQL personnalisée ou une procédure stockée et spécifier des colonnes à partir d’une table ou d’une vue.

> [!NOTE]
> Pour le premier exemple, utilisez l’option spécifier les colonnes à partir d’une table ou d’une vue. Nous reviendrons à l’Assistant plus loin dans ce didacticiel et explorons l’option spécifier une instruction SQL personnalisée ou une procédure stockée.

La figure 8 montre l’écran configurer l’instruction SELECT lorsque la case d’option spécifier les colonnes d’une table ou d’une vue est cochée. La liste déroulante contient l’ensemble des tables et des vues de la base de données Northwind, avec les colonnes de la table ou de la vue sélectionnée répertoriées dans la liste de cases à cocher ci-dessous. Pour cet exemple, nous allons retourner les colonnes `ProductID`, `ProductName`et `UnitPrice` de la table `Products`. Comme le montre la figure 8, après avoir effectué ces sélections, l’Assistant affiche l’instruction SQL qui en résulte `SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`.

![Retourner des données de la table Products](querying-data-with-the-sqldatasource-control-cs/_static/image10.gif)

**Figure 8**: retourner des données à partir de la table `Products`

Une fois que vous avez configuré l’Assistant pour retourner les colonnes `ProductID`, `ProductName`et `UnitPrice` de la table `Products`, cliquez sur le bouton suivant. Cet écran final donne la possibilité d’examiner les résultats de la requête configurée à partir de l’étape précédente. En cliquant sur le bouton tester la requête, vous exécutez l’instruction `SELECT` configurée et affiche les résultats dans une grille.

![Cliquez sur le bouton tester la requête pour passer en revue votre requête SELECT](querying-data-with-the-sqldatasource-control-cs/_static/image11.gif)

**Figure 9**: cliquer sur le bouton tester la requête pour passer en revue votre requête `SELECT`

Pour mettre fin à l'Assistant, cliquez sur Terminer.

Comme avec ObjectDataSource, l’Assistant SqlDataSource s affecte simplement des valeurs aux propriétés du contrôle, à savoir les propriétés [`ConnectionString`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.connectionstring.aspx) et [`SelectCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommand.aspx) . Une fois l’exécution de l’Assistant terminée, le balisage déclaratif de votre contrôle SqlDataSource doit ressembler à ce qui suit :

[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample2.aspx)]

La propriété `ConnectionString` fournit des informations sur la façon de se connecter à la base de données. Cette propriété peut être assignée à une valeur de chaîne de connexion complète, codée en dur ou peut pointer vers une chaîne de connexion dans `Web.config`. Pour référencer une valeur de chaîne de connexion dans Web. config, utilisez la syntaxe `<%$ expressionPrefix:expressionValue %>`. En général, *expressionPrefix* est connectionStrings et *expressionValue* est le nom de la chaîne de connexion dans la [section `Web.config``<connectionStrings>`](https://msdn.microsoft.com/library/bf7sd233.aspx). Toutefois, la syntaxe peut être utilisée pour référencer des éléments de `<appSettings>` ou du contenu à partir de fichiers de ressources. Pour plus d’informations sur cette syntaxe, consultez [vue d’ensemble des Expressions ASP.net](https://msdn.microsoft.com/library/d5bd1tad.aspx) .

La propriété `SelectCommand` spécifie l’instruction SQL ou la procédure stockée ad hoc à exécuter pour retourner les données.

## <a name="step-3-adding-a-data-web-control-and-binding-it-to-the-sqldatasource"></a>Étape 3 : ajout d’un contrôle Web de données et liaison de celui-ci au SqlDataSource

Une fois que le SqlDataSource a été configuré, il peut être lié à un contrôle Web de données, tel qu’un GridView ou DetailsView. Pour ce didacticiel, vous allez afficher les données dans un GridView. À partir de la boîte à outils, faites glisser un contrôle GridView sur la page et liez-le au `ProductsDataSource` SqlDataSource en choisissant la source de données dans la liste déroulante de la balise active GridView s.

[![ajouter un GridView et le lier au contrôle SqlDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image13.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image12.gif)

**Figure 10**: ajouter un GridView et le lier au contrôle SqlDataSource ([cliquez pour afficher l’image en taille réelle](querying-data-with-the-sqldatasource-control-cs/_static/image14.gif))

Une fois que vous avez sélectionné le contrôle SqlDataSource dans la liste déroulante de la balise active GridView s, Visual Studio ajoute automatiquement un BoundField ou un CheckBoxField au GridView pour chacune des colonnes retournées par le contrôle de source de données. Étant donné que le SqlDataSource retourne trois colonnes de base de données `ProductID`, `ProductName`et `UnitPrice` il y a trois champs dans le GridView.

Prenez un moment pour configurer le contrôle GridView s trois BoundFields. Remplacez la valeur de la propriété `ProductName` champ s `HeaderText` par le nom du produit et le champ `UnitPrice` s par Price. Mettez également en forme le champ `UnitPrice` en tant que devise. Une fois ces modifications apportées, votre balisage déclaratif s doit ressembler à ce qui suit :

[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample3.aspx)]

Visitez cette page via un navigateur. Comme le montre la figure 11, le contrôle GridView répertorie chaque produit `ProductID`, `ProductName`et `UnitPrice` valeurs.

[![le contrôle GridView affiche les valeurs ProductID, ProductName et UnitPrice de chaque produit](querying-data-with-the-sqldatasource-control-cs/_static/image16.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image15.gif)

**Figure 11**: le contrôle GridView affiche les valeurs de `ProductID`, `ProductName`et `UnitPrice` du produit ([cliquez pour afficher l’image en taille réelle](querying-data-with-the-sqldatasource-control-cs/_static/image17.gif))

Quand la page est visitée, le contrôle GridView appelle sa méthode `Select()` de contrôle de source de données. Lors de l’utilisation du contrôle ObjectDataSource, il s’agit de la méthode `ProductsBLL` classe s `GetProducts()`. Toutefois, avec le SqlDataSource, la méthode `Select()` établit une connexion à la base de données spécifiée et émet le `SelectCommand` (`SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`, dans cet exemple). Le SqlDataSource retourne ses résultats que le GridView énumère ensuite, en créant une ligne dans le GridView pour chaque enregistrement de base de données retourné.

## <a name="the-built-in-data-web-control-features-and-the-sqldatasource-control"></a>Fonctionnalités de contrôle Web de données intégrées et le contrôle SqlDataSource

En général, les fonctionnalités inhérentes aux contrôles Web de données de pagination, de tri, de modification, de suppression, d’insertion, etc. sont spécifiques au contrôle Web de données et ne dépendent pas du contrôle de source de données utilisé. Autrement dit, le GridView peut utiliser ses fonctionnalités intégrées de pagination, de tri, de modification et de suppression s’il est lié à un ObjectDataSource ou un SqlDataSource. Toutefois, certaines fonctionnalités de contrôle Web de données sont sensibles au contrôle de source de données utilisé ou à la configuration du contrôle de source de données.

Par exemple, dans le didacticiel [pagination efficace de grandes quantités de données](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) , nous avons abordé la manière dont, par défaut, la logique de pagination pour les contrôles Web de données naïvement retourne *tous les* enregistrements de la source de données sous-jacente, puis affiche uniquement le sous-ensemble approprié des enregistrements en fonction de l’index de page actuel et le nombre d’enregistrements à afficher par page. Ce modèle est très inefficace lors de la pagination de jeux de résultats suffisamment volumineux. Heureusement, l’ObjectDataSource peut être configuré pour prendre en charge la pagination personnalisée, qui retourne uniquement le sous-ensemble précis des enregistrements à afficher. Toutefois, le contrôle SqlDataSource ne dispose pas des propriétés permettant d’implémenter une pagination personnalisée.

Une autre subtilité avec la pagination et le tri survient avec le SqlDataSource. Par défaut, les données retournées par un SqlDataSource peuvent être paginées ou triées via le contrôle GridView. Pour illustrer cela, activez les options activer la pagination et activer le tri dans la balise active GridView s dans `Querying.aspx` et vérifier que cela fonctionne comme prévu.

Le tri et la pagination fonctionnent car le SqlDataSource récupère les données de la base de données dans un DataSet faiblement typé. Le nombre total d’enregistrements renvoyés par la requête est un aspect essentiel de la mise en œuvre de la pagination à partir du jeu de données. En outre, les résultats du jeu de données peuvent être triés par l’intermédiaire d’un DataView. Ces fonctionnalités sont utilisées automatiquement par le SqlDataSource lorsque le GridView demande des données paginées ou triées.

Le SqlDataSource peut être configuré pour retourner un DataReader au lieu d’un jeu de données en remplaçant sa [propriété`DataSourceMode`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx) `DataSet` (valeur par défaut) par `DataReader`. L’utilisation d’un DataReader peut être préférable dans les cas où vous passez les résultats du SqlDataSource à un code existant qui attend un DataReader. En outre, étant donné que les DataReaders sont des objets beaucoup plus simples que les jeux de données, ils offrent de meilleures performances. Toutefois, si vous apportez cette modification, le contrôle Web de données ne peut pas effectuer de tri ou de page, car le SqlDataSource ne peut pas déterminer le nombre d’enregistrements retournés par la requête et le DataReader n’offre aucune technique pour trier les données renvoyées.

## <a name="step-4-using-a-custom-sql-statement-or-stored-procedure"></a>Étape 4 : utilisation d’une instruction SQL personnalisée ou d’une procédure stockée

Lors de la configuration du contrôle SqlDataSource, la requête utilisée pour retourner des données peut être spécifiée dans l’une des deux approches comme une instruction SQL personnalisée ou une procédure stockée, ou sous forme de colonnes d’une table ou d’une vue existante. À l’étape 2, nous avons examiné la sélection des colonnes dans la table `Products`. Observons l’utilisation d’une instruction SQL personnalisée.

Ajoutez un autre contrôle GridView à la page `Querying.aspx` et choisissez de créer une nouvelle source de données dans la liste déroulante de la balise active. Ensuite, indiquez que les données seront extraites d’une base de données. cette opération créera un nouveau contrôle SqlDataSource. Nommez le contrôle `ProductsWithCategoryInfoDataSource`.

![Créer un nouveau contrôle SqlDataSource nommé ProductsWithCategoryInfoDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image18.gif)

**Figure 12**: créer un contrôle SqlDataSource nommé `ProductsWithCategoryInfoDataSource`

L’écran suivant nous demande de spécifier la base de données. Comme nous l’avons fait à la figure 7, sélectionnez la `NORTHWINDConnectionString` dans la liste déroulante, puis cliquez sur suivant. Dans l’écran configurer l’instruction SELECT, sélectionnez la case d’option spécifier une instruction SQL personnalisée ou une procédure stockée, puis cliquez sur suivant. Cela entraîne l’affichage de l’écran définir des instructions personnalisées ou des procédures stockées, qui propose des onglets intitulés sélectionner, mettre à jour, insérer et supprimer. Dans chaque onglet, vous pouvez entrer une instruction SQL personnalisée dans la zone de texte ou choisir une procédure stockée dans la liste déroulante. Dans ce didacticiel, nous allons examiner la saisie d’une instruction SQL personnalisée. le didacticiel suivant contient un exemple qui utilise une procédure stockée.

![Entrer une instruction SQL personnalisée ou choisir une procédure stockée](querying-data-with-the-sqldatasource-control-cs/_static/image19.gif)

**Figure 13**: entrer une instruction SQL personnalisée ou choisir une procédure stockée

L’instruction SQL personnalisée peut être entrée manuellement dans la zone de texte ou peut être construite graphiquement en cliquant sur le bouton Générateur de requêtes. À partir de la Générateur de requêtes ou de la zone de texte, utilisez la requête suivante pour retourner les champs `ProductID` et `ProductName` de la table `Products` à l’aide d’un `JOIN` pour récupérer le `CategoryName` du produit à partir de la table `Categories` :

[!code-sql[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample4.sql)]

![Vous pouvez construire la requête sous forme graphique à l’aide de l’Générateur de requêtes](querying-data-with-the-sqldatasource-control-cs/_static/image20.gif)

**Figure 14**: vous pouvez créer graphiquement la requête à l’aide de l’générateur de requêtes

Après avoir spécifié la requête, cliquez sur suivant pour passer à l’écran tester la requête. Cliquez sur Terminer pour terminer l’Assistant SqlDataSource.

Une fois l’exécution de l’Assistant terminée, le GridView aura trois BoundFields ajoutées à celui-ci, qui affiche les colonnes `ProductID`, `ProductName`et `CategoryName` retournées par la requête et qui génèrent le balisage déclaratif suivant :

[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample5.aspx)]

[![le GridView affiche l’ID, le nom et le nom de la catégorie associée du produit](querying-data-with-the-sqldatasource-control-cs/_static/image22.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image21.gif)

**Figure 15**: le GridView affiche l’ID, le nom et le nom de la catégorie associée du produit ([cliquez pour afficher l’image en taille réelle](querying-data-with-the-sqldatasource-control-cs/_static/image23.gif))

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, nous avons vu comment interroger et afficher les données à l’aide du contrôle SqlDataSource. À l’instar de l’ObjectDataSource, le SqlDataSource fait office de proxy, fournissant une approche déclarative de l’accès aux données. Ses propriétés spécifient la base de données à laquelle se connecter et la requête de `SELECT` SQL à exécuter. ils peuvent être spécifiés par le biais de l’Fenêtre Propriétés ou à l’aide de l’Assistant Configurer la source de donnée.

Les exemples de requêtes `SELECT` que nous avons examinés dans ce didacticiel ont renvoyé tous les enregistrements de la requête spécifiée. Toutefois, le contrôle SqlDataSource peut inclure une clause `WHERE` avec des paramètres dont les valeurs sont assignées par programme ou qui sont automatiquement extraites d’une source spécifiée. Nous allons examiner comment créer et utiliser des requêtes paramétrables dans le didacticiel suivant.

Bonne programmation !

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Accès aux données de la base de données relationnelle](http://aspnet.4guysfromrolla.com/articles/022206-1.aspx)
- [Vue d’ensemble du contrôle SqlDataSource](https://msdn.microsoft.com/library/dz12d98w.aspx)
- [Didacticiels de démarrage rapide ASP.NET : contrôle SqlDataSource](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/sqldatasource.aspx)
- [Élément `<connectionStrings>` Web. config](https://msdn.microsoft.com/library/bf7sd233.aspx)
- [Référence de chaîne de connexion de base de données](http://www.connectionstrings.com/)

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel étaient Suzanne Connery, Bernadette Leigh et David SURU. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](using-parameterized-queries-with-the-sqldatasource-cs.md)
