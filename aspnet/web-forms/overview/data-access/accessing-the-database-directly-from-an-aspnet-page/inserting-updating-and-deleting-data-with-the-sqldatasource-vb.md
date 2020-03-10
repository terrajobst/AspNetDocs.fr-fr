---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-vb
title: Insertion, mise à jour et suppression de données avec le SqlDataSource (VB) | Microsoft Docs
author: rick-anderson
description: Dans les didacticiels précédents, nous avons appris comment le contrôle ObjectDataSource permettait d’insérer, de mettre à jour et de supprimer des données. Le contrôle SqlDataSource prend en charge t...
ms.author: riande
ms.date: 02/20/2007
ms.assetid: 9673bef3-892c-45ba-a7d8-0da3d6f48ec5
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: 4f7b282b09769272df8ff3a32aa4c509c8917481
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78626559"
---
# <a name="inserting-updating-and-deleting-data-with-the-sqldatasource-vb"></a>Insertion, mise à jour et suppression de données avec SqlDataSource (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_49_VB.exe) ou [Télécharger le PDF](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/datatutorial49vb1.pdf)

> Dans les didacticiels précédents, nous avons appris comment le contrôle ObjectDataSource permettait d’insérer, de mettre à jour et de supprimer des données. Le contrôle SqlDataSource prend en charge les mêmes opérations, mais l’approche est différente, et ce didacticiel montre comment configurer le SqlDataSource pour insérer, mettre à jour et supprimer des données.

## <a name="introduction"></a>Introduction

Comme nous l’avons vu dans [une vue d’ensemble de l’insertion, de la mise à jour et](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)de la suppression, le contrôle GridView fournit des fonctionnalités intégrées de mise à jour et de suppression, tandis que les contrôles DetailsView et FormView incluent la prise en charge de l’insertion avec la fonctionnalité de modification et de suppression. Ces fonctionnalités de modification des données peuvent être directement connectées à un contrôle de source de données sans qu’une ligne de code doive être écrite. [Vue d’ensemble de l’insertion, de la mise à jour et de la suppression](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) examinée à l’aide de ObjectDataSource pour faciliter l’insertion, la mise à jour et la suppression avec les contrôles GridView, DetailsView et FormView. Le SqlDataSource peut également être utilisé à la place de l’ObjectDataSource.

N’oubliez pas que pour prendre en charge l’insertion, la mise à jour et la suppression, avec l’ObjectDataSource, nous avions besoin de spécifier les méthodes de la couche objet à appeler pour exécuter l’action d’insertion, de mise à jour ou de suppression. Avec le SqlDataSource, nous devons fournir des instructions SQL de `INSERT`, `UPDATE`et `DELETE` (ou procédures stockées) à exécuter. Comme nous le verrons dans ce didacticiel, ces instructions peuvent être créées manuellement ou générées automatiquement par l’Assistant Configurer la source de données.

> [!NOTE]
> Étant donné que nous avons déjà abordé les fonctionnalités d’insertion, de modification et de suppression des contrôles GridView, DetailsView et FormView, ce didacticiel se concentre sur la configuration du contrôle SqlDataSource pour prendre en charge ces opérations. Si vous avez besoin de mettre en œuvre ces fonctionnalités dans les didacticiels GridView, DetailsView et FormView, revenez aux didacticiels de modification, d’insertion et de suppression de données, en commençant par [une vue d’ensemble de l’insertion, de la mise à jour et de la suppression](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md).

## <a name="step-1-specifyinginsertupdate-anddeletestatements"></a>Étape 1 : spécification des instructions`INSERT`,`UPDATE`et`DELETE`

Comme nous l’avons vu dans les deux derniers didacticiels, pour récupérer des données à partir d’un contrôle SqlDataSource, nous devons définir deux propriétés :

1. `ConnectionString`, qui spécifie la base de données à laquelle envoyer la requête, et
2. `SelectCommand`, qui spécifie l’instruction SQL ad hoc ou le nom de la procédure stockée à exécuter pour retourner les résultats.

Pour les valeurs de `SelectCommand` avec des paramètres, les valeurs de paramètre sont spécifiées via la collection de SqlDataSource s `SelectParameters` et peuvent inclure des valeurs codées en dur, des valeurs de source de paramètre communes (champs QueryString, variables de session, valeurs de contrôle Web, etc.) ou peuvent être assignées par programmation. Lorsque la méthode du contrôle SqlDataSource s `Select()` est appelée par programme ou automatiquement à partir d’un contrôle Web de données, une connexion à la base de données est établie, les valeurs des paramètres sont affectées à la requête et la commande est transversée vers la base de données. Les résultats sont ensuite renvoyés sous la forme d’un DataSet ou d’un DataReader, en fonction de la valeur de la propriété Control s `DataSourceMode`.

En plus de sélectionner des données, le contrôle SqlDataSource peut être utilisé pour insérer, mettre à jour et supprimer des données en fournissant `INSERT`, `UPDATE`et `DELETE` instructions SQL de la même façon. Affectez simplement les propriétés `InsertCommand`, `UpdateCommand`et `DeleteCommand` les instructions SQL `INSERT`, `UPDATE`et `DELETE` à exécuter. Si les instructions ont des paramètres (comme elles le sont le plus souvent), incluez-les dans les collections `InsertParameters`, `UpdateParameters`et `DeleteParameters`.

Une fois qu’une valeur `InsertCommand`, `UpdateCommand`ou `DeleteCommand` a été spécifiée, l’option Activer l’insertion, activer la modification ou activer la suppression dans la balise active des contrôles Web de données correspondants devient disponible. Pour illustrer cela, prenons un exemple de la page de `Querying.aspx` que nous avons créée dans le didacticiel sur l' [interrogation des données avec le contrôle de SqlDataSource](querying-data-with-the-sqldatasource-control-vb.md) et augmentez-la pour inclure les fonctionnalités de suppression.

Commencez par ouvrir les pages `InsertUpdateDelete.aspx` et `Querying.aspx` à partir du dossier `SqlDataSource`. Dans le concepteur de la page `Querying.aspx`, sélectionnez le contrôle SqlDataSource et GridView dans le premier exemple (les contrôles `ProductsDataSource` et `GridView1`). Après avoir sélectionné les deux contrôles, accédez au menu Edition, puis choisissez copier (ou appuyez simplement sur Ctrl + C). Ensuite, accédez au concepteur de `InsertUpdateDelete.aspx` et collez les contrôles. Une fois que vous avez déplacé les deux contrôles vers `InsertUpdateDelete.aspx`, testez la page dans un navigateur. Vous devez voir les valeurs des colonnes `ProductID`, `ProductName`et `UnitPrice` pour tous les enregistrements de la table de base de données `Products`.

[![tous les produits sont répertoriés, classés par ProductID](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image1.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image1.png)

**Figure 1**: tous les produits sont répertoriés, classés par `ProductID` ([cliquez pour afficher l’image en taille réelle](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image2.png))

## <a name="adding-the-sqldatasource-sdeletecommandanddeleteparametersproperties"></a>Ajout des propriétés`DeleteCommand`et`DeleteParameters`du SqlDataSource

À ce stade, nous disposons d’un SqlDataSource qui retourne simplement tous les enregistrements de la table `Products` et un GridView qui restitue ces données. Notre objectif est d’étendre cet exemple pour permettre à l’utilisateur de supprimer des produits via le contrôle GridView. Pour ce faire, nous devons spécifier des valeurs pour les propriétés du contrôle SqlDataSource s `DeleteCommand` et `DeleteParameters`, puis configurer le contrôle GridView pour prendre en charge la suppression.

Les propriétés `DeleteCommand` et `DeleteParameters` peuvent être spécifiées de plusieurs façons :

- À l’aide de la syntaxe déclarative
- À partir de la Fenêtre Propriétés dans le concepteur
- À partir de l’écran spécifier une instruction SQL personnalisée ou une procédure stockée, dans l’Assistant Configuration de la source de données
- Via le bouton avancé dans l’écran spécifier les colonnes d’une table de vue dans l’Assistant Configurer la source de données, qui génère en fait automatiquement l' `DELETE` instruction SQL et la collection de paramètres utilisés dans les propriétés `DeleteCommand` et `DeleteParameters`

Nous allons examiner comment créer automatiquement l’instruction `DELETE` créée à l’étape 2. Pour le moment, nous utilisons les Fenêtre Propriétés dans le concepteur, bien que l’Assistant Configurer la source de données ou l’option de syntaxe déclarative fonctionne également.

Dans le concepteur de `InsertUpdateDelete.aspx`, cliquez sur le `ProductsDataSource` SqlDataSource, puis affichez le Fenêtre Propriétés (dans le menu Affichage, choisissez Fenêtre Propriétés ou appuyez simplement sur F4). Sélectionnez la propriété DeleteQuery, qui affichera un ensemble de ellipses.

![Sélectionnez la propriété DeleteQuery dans la fenêtre Propriétés.](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image2.gif)

**Figure 2**: sélectionner la propriété DeleteQuery dans la fenêtre Propriétés

> [!NOTE]
> Le SqlDataSource ne dispose pas d’une propriété DeleteQuery. Au lieu de cela, DeleteQuery est une combinaison des propriétés `DeleteCommand` et `DeleteParameters` et est uniquement listé dans le Fenêtre Propriétés lors de l’affichage de la fenêtre par le biais du concepteur. Si vous examinez les Fenêtre Propriétés dans la vue source, vous trouverez la propriété `DeleteCommand` à la place.

Cliquez sur les points de suspension dans la propriété DeleteQuery pour afficher la boîte de dialogue Éditeur de commandes et de paramètres (voir figure 3). À partir de cette boîte de dialogue, vous pouvez spécifier l’instruction SQL `DELETE` et spécifier les paramètres. Entrez la requête suivante dans la zone de texte commande `DELETE` (manuellement ou à l’aide de la Générateur de requêtes, si vous le souhaitez) :

[!code-sql[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/samples/sample1.sql)]

Ensuite, cliquez sur le bouton actualiser les paramètres pour ajouter le paramètre `@ProductID` à la liste de paramètres ci-dessous.

[![sélectionnez la propriété DeleteQuery dans la fenêtre Propriétés.](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image3.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image3.png)

**Figure 3**: sélectionner la propriété DeleteQuery dans la fenêtre Propriétés ([cliquez pour afficher l’image en taille réelle](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image4.png))

Ne fournissez *pas* de valeur pour ce paramètre (laissez la source du paramètre sur None). Une fois que nous avons ajouté la prise en charge de la suppression au GridView, le GridView fournit automatiquement cette valeur de paramètre à l’aide de la valeur de sa collection `DataKeys` pour la ligne sur laquelle l’utilisateur a cliqué sur le bouton supprimer.

> [!NOTE]
> Le nom du paramètre utilisé dans la requête de `DELETE` *doit* être le même que le nom de la valeur `DataKeyNames` dans GridView, DetailsView ou FormView. Autrement dit, le paramètre dans l’instruction `DELETE` est nommé intentionnellement `@ProductID` (au lieu de, disons, `@ID`), car le nom de colonne de clé primaire dans la table Products (et par conséquent la valeur DataKeyNames dans le GridView) est `ProductID`.

Si le nom du paramètre et la valeur de `DataKeyNames` ne correspondent pas, le GridView ne peut pas attribuer automatiquement au paramètre la valeur de la collection de `DataKeys`.

Après avoir entré les informations relatives à la suppression dans la boîte de dialogue Éditeur de commandes et de paramètres, cliquez sur OK et accédez à la vue source pour examiner le balisage déclaratif obtenu :

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/samples/sample2.aspx)]

Notez l’ajout de la propriété `DeleteCommand`, ainsi que la section `<DeleteParameters>` et l’objet de paramètre nommé `productID`.

## <a name="configuring-the-gridview-for-deleting"></a>Configuration du contrôle GridView pour la suppression

Une fois la propriété `DeleteCommand` ajoutée, la balise active de GridView s contient désormais l’option Activer la suppression. Continuez et cochez cette case. Comme nous l’avons vu dans [une vue d’ensemble de l’insertion, de la mise à jour et](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)de la suppression, le contrôle GridView ajoute un CommandField avec sa propriété `ShowDeleteButton` définie sur `True`. Comme le montre la figure 4, lorsque la page est visitée via un navigateur, un bouton supprimer est inclus. Testez cette page en supprimant certains produits.

[![chaque ligne GridView contient maintenant un bouton supprimer](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image4.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image5.png)

**Figure 4**: chaque ligne GridView contient maintenant un bouton supprimer ([cliquez pour afficher l’image en taille réelle](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image6.png))

Lorsque vous cliquez sur un bouton supprimer, une publication (postback) se produit, le GridView affecte au paramètre `ProductID` la valeur de la valeur de la collection `DataKeys` pour la ligne dont l’utilisateur a cliqué sur le bouton supprimer et appelle la méthode SqlDataSource s `Delete()`. Le contrôle SqlDataSource se connecte ensuite à la base de données et exécute l’instruction `DELETE`. Le GridView est ensuite relié au SqlDataSource, en obtenant et en affichant l’ensemble actuel de produits (qui n’incluent plus l’enregistrement juste-à-temps).

> [!NOTE]
> Étant donné que le contrôle GridView utilise sa collection `DataKeys` pour remplir les paramètres SqlDataSource, il est vital que la propriété GridView s `DataKeyNames` soit définie sur la ou les colonnes qui constituent la clé primaire et que le SqlDataSource s `SelectCommand` retourne ces colonnes. En outre, il est important que le nom du paramètre dans le `DeleteCommand` SqlDataSource soit défini sur `@ProductID`. Si la propriété `DataKeyNames` n’est pas définie ou si le paramètre n’est pas nommé `@ProductsID`, le fait de cliquer sur le bouton supprimer entraîne une publication, mais ne supprime pas réellement un enregistrement.

La figure 5 illustre cette interaction graphiquement. Reportez-vous au didacticiel [examen des événements associés à l’insertion, à la mise à jour et](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) à la suppression pour obtenir une discussion plus détaillée sur la chaîne d’événements associée à l’insertion, la mise à jour et la suppression d’un contrôle Web de données.

![En cliquant sur le bouton supprimer dans le contrôle GridView, vous appelez la méthode SqlDataSource s Delete ().](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image5.gif)

**Figure 5**: Si vous cliquez sur le bouton supprimer dans le contrôle GridView, la méthode `Delete()` SqlDataSource s est appelée

## <a name="step-2-automatically-generating-theinsertupdate-anddeletestatements"></a>Étape 2 : génération automatique des instructions`INSERT`,`UPDATE`et`DELETE`

À l’étape 1, les instructions SQL `INSERT`, `UPDATE`et `DELETE` peuvent être spécifiées via le Fenêtre Propriétés ou la syntaxe déclarative du contrôle. Toutefois, cette approche nécessite que nous écrivions manuellement les instructions SQL à la main, ce qui peut être monotone et sujet aux erreurs. Heureusement, l’Assistant Configuration de la source de données offre la possibilité de générer automatiquement les instructions `INSERT`, `UPDATE`et `DELETE` lors de l’utilisation de l’écran spécifier les colonnes d’une table de vue.

Nous allons explorer cette option de génération automatique. Ajoutez un DetailsView au concepteur dans `InsertUpdateDelete.aspx` et affectez à sa propriété `ID` la valeur `ManageProducts`. Ensuite, dans la balise active DetailsView s, choisissez de créer une nouvelle source de données et de créer un SqlDataSource nommé `ManageProductsDataSource`.

[![créer un nouveau SqlDataSource nommé ManageProductsDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image6.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image7.png)

**Figure 6**: créer un nouveau SqlDataSource nommé `ManageProductsDataSource` ([cliquez pour afficher l’image en taille réelle](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image8.png))

Dans l’Assistant Configurer la source de données, choisissez d’utiliser la chaîne de connexion `NORTHWINDConnectionString`, puis cliquez sur suivant. Dans l’écran configurer l’instruction SELECT, laissez la case d’option spécifier les colonnes d’une table ou d’une vue sélectionnée, puis sélectionnez la table `Products` dans la liste déroulante. Sélectionnez les colonnes `ProductID`, `ProductName`, `UnitPrice`et `Discontinued` dans la liste de cases à cocher.

[![à l’aide de la table Products, retournez les colonnes ProductID, ProductName, UnitPrice et Discontinued](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image7.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image9.png)

**Figure 7**: utilisation de la table `Products`, retourner les colonnes `ProductID`, `ProductName`, `UnitPrice`et `Discontinued` ([cliquez pour afficher l’image en taille réelle](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image10.png))

Pour générer automatiquement des instructions `INSERT`, `UPDATE`et `DELETE` en fonction de la table et des colonnes sélectionnées, cliquez sur le bouton Avancé et cochez la case générer des `INSERT`, des `UPDATE`et des instructions `DELETE`.

![Cochez la case générer des instructions INSERT, UPDATE et DELETE](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image8.gif)

**Figure 8**: cochez la case générer des instructions `INSERT`, `UPDATE`et `DELETE`

La case à cocher générer des instructions `INSERT`, `UPDATE`et `DELETE` ne peut être vérifiée que si la table sélectionnée a une clé primaire et que la ou les colonnes de clé primaire sont incluses dans la liste des colonnes retournées. La case à cocher utiliser l’accès concurrentiel optimiste, qui devient sélectionnable Lorsque la case à cocher générer `INSERT`, `UPDATE`et `DELETE` instructions a été activée, augmente les clauses `WHERE` dans les instructions `UPDATE` et `DELETE` résultantes pour fournir un contrôle d’accès concurrentiel optimiste. Pour le moment, laissez cette case désactivée. Nous allons examiner l’accès concurrentiel optimiste avec le contrôle SqlDataSource dans le prochain didacticiel.

Après avoir activé la case à cocher générer `INSERT`, `UPDATE`et `DELETE` des instructions, cliquez sur OK pour revenir à l’écran configurer l’instruction SELECT, puis cliquez sur suivant et enfin sur Terminer pour terminer l’Assistant Configuration de la source de données. À la fin de l’Assistant, Visual Studio ajoute BoundFields au DetailsView pour les colonnes `ProductID`, `ProductName`et `UnitPrice` et un CheckBoxField pour la colonne `Discontinued`. À partir de la balise active de DetailsView s, activez l’option Activer la pagination afin que l’utilisateur visitant cette page puisse parcourir les produits. Désactivez également les propriétés du contrôle DetailsView s `Width` et `Height`.

Notez que les options activer l’insertion, activer la modification et activer la suppression sont disponibles pour la balise active. Cela est dû au fait que le SqlDataSource contient des valeurs pour son `InsertCommand`, `UpdateCommand`et `DeleteCommand`, comme le montre la syntaxe déclarative suivante :

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/samples/sample3.aspx)]

Notez que les valeurs du contrôle SqlDataSource ont été automatiquement définies pour ses propriétés `InsertCommand`, `UpdateCommand`et `DeleteCommand`. L’ensemble de colonnes référencé dans les propriétés `InsertCommand` et `UpdateCommand` est basé sur ceux de l’instruction `SELECT`. Autrement dit, au lieu d’avoir *chaque* colonne Products dans le `InsertCommand` et `UpdateCommand`, seules les colonnes spécifiées dans le `SelectCommand` (moins `ProductID`, ce qui est omis parce qu’il s’agit d’une [colonne`IDENTITY`](http://www.sqlteam.com/item.asp?ItemID=102), dont la valeur ne peut pas être modifiée lors de la modification et qui est affectée automatiquement lors de l’insertion de). En outre, pour chaque paramètre dans les propriétés `InsertCommand`, `UpdateCommand`et `DeleteCommand`, les paramètres correspondants dans les collections `InsertParameters`, `UpdateParameters`et `DeleteParameters`.

Pour activer les fonctionnalités de modification des données du contrôle DetailsView, cochez les options activer l’insertion, activer la modification et activer la suppression dans sa balise active. Cela ajoute une CommandField avec ses propriétés `ShowInsertButton`, `ShowEditButton`et `ShowDeleteButton` définies sur `True`.

Accédez à la page dans un navigateur et notez les boutons modifier, supprimer et nouveau inclus dans le DetailsView. Le fait de cliquer sur le bouton Modifier permet de transformer le DetailsView en mode édition, qui affiche chaque BoundField dont la propriété `ReadOnly` a la valeur `False` (valeur par défaut) en tant que zone de texte, et CheckBoxField en tant que case à cocher.

[![l’interface d’édition par défaut de DetailsView s](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image9.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image11.png)

**Figure 9**: interface de modification par défaut de DetailsView s ([cliquez pour afficher l’image en taille réelle](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image12.png))

De même, vous pouvez supprimer le produit actuellement sélectionné ou ajouter un nouveau produit au système. Étant donné que l’instruction `InsertCommand` fonctionne uniquement avec les colonnes `ProductName`, `UnitPrice`et `Discontinued`, les autres colonnes ont soit `NULL`, soit leur valeur par défaut affectée par la base de données lors de l’insertion. À l’instar de l’ObjectDataSource, si la `InsertCommand` n’a pas de colonnes de table de base de données qui n’autorisent pas `NULL` s et que Don t a une valeur par défaut, une erreur SQL se produit lors de la tentative d’exécution de l’instruction `INSERT`.

> [!NOTE]
> Les interfaces d’insertion et de modification du contrôle DetailsView n’ont pas de personnalisation ou de validation. Pour ajouter des contrôles de validation ou pour personnaliser les interfaces, vous devez convertir le BoundFields en TemplateFields. Pour plus d’informations, consultez [Ajout de contrôles de validation aux interfaces de modification et d’insertion](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) et personnalisation des didacticiels sur [l’interface de modification des données](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) .

En outre, gardez à l’esprit que pour la mise à jour et la suppression, le contrôle DetailsView utilise la valeur de `DataKey` du produit actuel, qui est présente uniquement si la propriété `DataKeyNames` est configurée. Si la modification ou la suppression semble n’avoir aucun effet, vérifiez que la propriété `DataKeyNames` est définie.

## <a name="limitations-of-automatically-generating-sql-statements"></a>Limitations de la génération automatique d’instructions SQL

Étant donné que l’option Generate `INSERT`, `UPDATE`et `DELETE`s est disponible uniquement lors du prélèvement de colonnes dans une table, pour les requêtes plus complexes, vous devrez écrire vos propres instructions `INSERT`, `UPDATE`et `DELETE` comme nous l’avons fait à l’étape 1. En règle générale, les instructions SQL `SELECT` utilisent des `JOIN` s pour ramener les données d’une ou de plusieurs tables de recherche à des fins d’affichage (par exemple, la remise du champ `Categories` table s `CategoryName` lors de l’affichage des informations sur le produit). En même temps, nous pouvons autoriser l’utilisateur à modifier, mettre à jour ou insérer des données dans la table principale (`Products`, dans le cas présent).

Alors que les instructions `INSERT`, `UPDATE`et `DELETE` peuvent être entrées manuellement, prenez en compte le Conseil qui économise le temps suivant. Configurez initialement le SqlDataSource afin qu’il récupère les données uniquement à partir de la table `Products`. Utilisez l’Assistant Configurer la source de données pour spécifier les colonnes d’une table ou d’un écran d’affichage afin de pouvoir générer automatiquement les instructions `INSERT`, `UPDATE`et `DELETE`. Ensuite, après avoir terminé l’Assistant, choisissez de configurer le SelectQuery à partir de la Fenêtre Propriétés (ou bien, revenez à l’Assistant Configuration de la source de données, mais utilisez l’option spécifier une instruction SQL personnalisée ou une procédure stockée). Ensuite, mettez à jour l’instruction `SELECT` pour inclure la syntaxe `JOIN`. Cette technique offre les avantages de gagner du temps pour les instructions SQL générées automatiquement et permet d’obtenir une instruction `SELECT` plus personnalisée.

Une autre limitation de la génération automatique des instructions `INSERT`, `UPDATE`et `DELETE` est que les colonnes des instructions `INSERT` et `UPDATE` sont basées sur les colonnes retournées par l’instruction `SELECT`. Toutefois, nous pouvons être amenés à mettre à jour ou à insérer plus ou moins de champs. Par exemple, dans l’exemple de l’étape 2, peut-être souhaitez-vous que le `UnitPrice` BoundField soit en lecture seule. Dans ce cas, il ne doit pas apparaître dans la `UpdateCommand`. Ou bien, vous pouvez définir la valeur d’un champ de table qui n’apparaît pas dans le GridView. Par exemple, lors de l’ajout d’un nouvel enregistrement, nous pouvons souhaiter que la valeur `QuantityPerUnit` soit définie sur TODO.

Si de telles personnalisations sont requises, vous devez les effectuer manuellement, à l’aide de la Fenêtre Propriétés, de l’option spécifier une instruction SQL personnalisée ou une procédure stockée dans l’Assistant, ou via la syntaxe déclarative.

> [!NOTE]
> Lorsque vous ajoutez des paramètres qui n’ont pas de champs correspondants dans le contrôle Web de données, n’oubliez pas que ces valeurs de paramètres doivent être affectées à des valeurs de quelque manière que ce soit. Ces valeurs peuvent être : codées en dur directement dans le `InsertCommand` ou `UpdateCommand`; peut provenir d’une source prédéfinie (queryString, état de session, contrôles Web sur la page, etc.); ou peuvent être affectés par programme, comme nous l’avons vu dans le didacticiel précédent.

## <a name="summary"></a>Récapitulatif

Pour que les contrôles Web de données utilisent leurs fonctionnalités intégrées d’insertion, de modification et de suppression, le contrôle de source de données auquel elles sont liées doit offrir une telle fonctionnalité. Pour le SqlDataSource, cela signifie que les instructions SQL `INSERT`, `UPDATE`et `DELETE` doivent être assignées aux propriétés `InsertCommand`, `UpdateCommand`et `DeleteCommand`. Ces propriétés, ainsi que les collections de paramètres correspondantes, peuvent être ajoutées manuellement ou générées automatiquement via l’Assistant Configuration de la source de données. Dans ce didacticiel, nous avons examiné les deux techniques.

Nous avons examiné l’utilisation de l’accès concurrentiel optimiste avec ObjectDataSource dans le didacticiel sur l’implémentation de l' [accès concurrentiel optimiste](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) . Le contrôle SqlDataSource fournit également la prise en charge de l’accès concurrentiel optimiste. Comme indiqué à l’étape 2, lors de la génération automatique des instructions `INSERT`, `UPDATE`et `DELETE`, l’Assistant propose une option d’accès concurrentiel optimiste. Comme nous le verrons dans le didacticiel suivant, l’utilisation de l’accès concurrentiel optimiste avec le SqlDataSource modifie les clauses `WHERE` dans les instructions `UPDATE` et `DELETE` pour s’assurer que les valeurs des autres colonnes n’ont pas changé depuis la dernière fois que les données ont été affichées sur la page.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Précédent](using-parameterized-queries-with-the-sqldatasource-vb.md)
> [Suivant](implementing-optimistic-concurrency-with-the-sqldatasource-vb.md)
