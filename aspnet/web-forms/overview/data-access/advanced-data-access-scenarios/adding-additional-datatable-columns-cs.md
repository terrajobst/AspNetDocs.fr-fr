---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-cs
title: Ajout de colonnes de DataTableC#supplémentaires () | Microsoft Docs
author: rick-anderson
description: Lorsque vous utilisez l’Assistant TableAdapter pour créer un DataSet typé, le DataTable correspondant contient les colonnes retournées par la requête de base de données principale. Mais ici...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: 615f3361-f21f-4338-8bc1-fce8ae071de9
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-cs
msc.type: authoredcontent
ms.openlocfilehash: a96f254aa54e7077456ac1a9bd6c5e2a17619d96
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78552065"
---
# <a name="adding-additional-datatable-columns-c"></a>Ajout de colonnes de DataTable supplémentaires (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_70_CS.zip) ou [Télécharger le PDF](adding-additional-datatable-columns-cs/_static/datatutorial70cs1.pdf)

> Lorsque vous utilisez l’Assistant TableAdapter pour créer un DataSet typé, le DataTable correspondant contient les colonnes retournées par la requête de base de données principale. Toutefois, il existe des occasions où le DataTable doit inclure des colonnes supplémentaires. Dans ce didacticiel, nous expliquons pourquoi des procédures stockées sont recommandées lorsque nous avons besoin de colonnes DataTable supplémentaires.

## <a name="introduction"></a>Introduction

Quand vous ajoutez un TableAdapter à un DataSet typé, le schéma du DataTable s correspondant est déterminé par la requête principale du TableAdapter. Par exemple, si la requête principale retourne les champs *de données a*, *b*et *c*, le DataTable aura trois colonnes correspondantes nommées *a*, *b*et *c*. En plus de sa requête principale, un TableAdapter peut inclure des requêtes supplémentaires qui retournent, éventuellement, un sous-ensemble de données basé sur un paramètre. Par exemple, en plus de la requête principale de `ProductsTableAdapter` s, qui retourne des informations sur tous les produits, elle contient également des méthodes comme `GetProductsByCategoryID(categoryID)` et `GetProductByProductID(productID)`, qui retournent des informations spécifiques sur les produits en fonction d’un paramètre fourni.

Le modèle d’utilisation du schéma de la table de données reflète bien la requête principale de TableAdapter si toutes les méthodes de TableAdapter retournent le même nombre de champs de données ou moins que ceux spécifiés dans la requête principale. Si une méthode TableAdapter doit retourner des champs de données supplémentaires, nous devons développer le schéma DataTable s en conséquence. Dans le [maître/détail à l’aide d’une liste à puces d’enregistrements maîtres avec un didacticiel détails sur DataList](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) , nous avons ajouté une méthode au `CategoriesTableAdapter` qui a renvoyé les champs de données `CategoryID`, `CategoryName`et `Description` définis dans la requête principale plus `NumberOfProducts`, un champ de données supplémentaire qui a signalé le nombre de produits associés à chaque catégorie. Nous avons ajouté manuellement une nouvelle colonne au `CategoriesDataTable` afin de capturer la valeur du champ de données `NumberOfProducts` à partir de cette nouvelle méthode.

Comme indiqué dans le didacticiel sur le [téléchargement de fichiers](../working-with-binary-files/uploading-files-cs.md) , il est important de bien comprendre les TableAdapters qui utilisent des instructions SQL ad hoc et les méthodes dont les champs de données ne correspondent pas exactement à la requête principale. Si l’Assistant Configuration de TableAdapter est réexécuté, il met à jour toutes les méthodes de TableAdapter pour que la liste des champs de données corresponde à la requête principale. Par conséquent, toutes les méthodes avec des listes de colonnes personnalisées sont restaurées dans la liste des colonnes de la requête principale et ne retournent pas les données attendues. Ce problème ne se pose pas lors de l’utilisation de procédures stockées.

Dans ce didacticiel, nous allons examiner comment étendre un schéma DataTable s pour inclure des colonnes supplémentaires. En raison de la fragilité du TableAdapter lors de l’utilisation d’instructions SQL ad hoc, dans ce didacticiel, nous allons utiliser des procédures stockées. Pour plus d’informations sur la configuration d’un TableAdapter pour l’utilisation de procédures stockées, consultez [création de procédures stockées pour les TableAdapters de DataSet typés](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) et [utilisation de procédures stockées existantes pour les didacticiels de TableAdapters de datasets typés](https://data-access/tutorials/using-existing-stored-procedures-for-the-typed-dataset-amp-rsquo-s-tableadapters-cs) .

## <a name="step-1-adding-apricequartilecolumn-to-theproductsdatatable"></a>Étape 1 : ajout d’une colonne`PriceQuartile`à la`ProductsDataTable`

Dans le didacticiel *création de nouvelles procédures stockées pour les TableAdapters de DataSet typé* , nous avons créé un DataSet typé nommé `NorthwindWithSprocs`. Ce jeu de données contient actuellement deux DataTables : `ProductsDataTable` et `EmployeesDataTable`. L' `ProductsTableAdapter` présente les trois méthodes suivantes :

- `GetProducts`-la requête principale, qui retourne tous les enregistrements de la table `Products`
- `GetProductsByCategoryID(categoryID)`-retourne tous les produits avec la *CategoryID*spécifiée.
- `GetProductByProductID(productID)` : renvoie le produit spécifique avec le *ProductID*spécifié.

La requête principale et les deux autres méthodes retournent toutes le même jeu de champs de données, à savoir toutes les colonnes de la table `Products`. Il n’y a pas de sous-requêtes corrélées, ni de `JOIN` qui extraient les données associées des tables `Categories` ou `Suppliers`. Par conséquent, la `ProductsDataTable` a une colonne correspondante pour chaque champ de la table `Products`.

Pour ce didacticiel, nous allons ajouter une méthode au `ProductsTableAdapter` nommé `GetProductsWithPriceQuartile` qui retourne tous les produits. En plus des champs de données de produit standard, `GetProductsWithPriceQuartile` inclura également un champ de données `PriceQuartile` qui indique le niveau d’activité du quartile du prix du produit. Par exemple, les produits dont le prix est inférieur ou supérieur à 25% auront une valeur `PriceQuartile` de 1, tandis que ceux dont les prix tombent dans les 25% inférieurs auront la valeur 4. Avant de nous soucier de la création de la procédure stockée pour retourner ces informations, nous devons tout d’abord mettre à jour le `ProductsDataTable` pour inclure une colonne qui contiendra le `PriceQuartile` résultats lorsque la méthode `GetProductsWithPriceQuartile` est utilisée.

Ouvrez le jeu de données `NorthwindWithSprocs`, puis cliquez avec le bouton droit sur l' `ProductsDataTable`. Choisissez Ajouter dans le menu contextuel, puis choisissez colonne.

[![ajouter une nouvelle colonne au ProductsDataTable](adding-additional-datatable-columns-cs/_static/image2.png)](adding-additional-datatable-columns-cs/_static/image1.png)

**Figure 1**: ajouter une nouvelle colonne au `ProductsDataTable` ([cliquez pour afficher l’image en taille réelle](adding-additional-datatable-columns-cs/_static/image3.png))

Cette opération ajoute une nouvelle colonne au DataTable nommé Column1 de type `System.String`. Nous devons mettre à jour ce nom de colonne s en PriceQuartile et son type à `System.Int32`, car il sera utilisé pour contenir un nombre compris entre 1 et 4. Sélectionnez la colonne qui vient d’être ajoutée dans le `ProductsDataTable` et, dans la Fenêtre Propriétés, affectez à la propriété `Name` la valeur PriceQuartile et à la propriété `DataType` la valeur `System.Int32`.

[![définir les propriétés de nom et de type de données de la nouvelle colonne](adding-additional-datatable-columns-cs/_static/image5.png)](adding-additional-datatable-columns-cs/_static/image4.png)

**Figure 2**: définir les propriétés des nouvelles colonnes `Name` et `DataType` ([cliquez pour afficher l’image en taille réelle](adding-additional-datatable-columns-cs/_static/image6.png))

Comme le montre la figure 2, il existe des propriétés supplémentaires qui peuvent être définies, par exemple si les valeurs de la colonne doivent être uniques, si la colonne est une colonne à incrémentation automatique, si les valeurs `NULL` de la base de données sont autorisées ou non, et ainsi de suite. Laissez ces valeurs définies sur leurs valeurs par défaut.

## <a name="step-2-creating-thegetproductswithpricequartilemethod"></a>Étape 2 : création de la méthode`GetProductsWithPriceQuartile`

Maintenant que la `ProductsDataTable` a été mise à jour pour inclure la colonne `PriceQuartile`, nous sommes prêts à créer la méthode `GetProductsWithPriceQuartile`. Commencez par cliquer avec le bouton droit sur le TableAdapter et choisissez Ajouter une requête dans le menu contextuel. L’Assistant Configuration de requêtes TableAdapter s’affiche pour la première fois, qui nous invite à indiquer si vous souhaitez utiliser des instructions SQL ad hoc ou une procédure stockée nouvelle ou existante. Étant donné que nous ne disposons pas encore d’une procédure stockée qui retourne les données du prix du quartile, autorisez le TableAdapter à créer cette procédure stockée pour nous. Sélectionnez l’option créer une nouvelle procédure stockée, puis cliquez sur suivant.

[![demander à l’Assistant TableAdapter de créer la procédure stockée pour nous](adding-additional-datatable-columns-cs/_static/image8.png)](adding-additional-datatable-columns-cs/_static/image7.png)

**Figure 3**: demander à l’Assistant TableAdapter de créer la procédure stockée pour nous ([cliquez pour afficher l’image en taille réelle](adding-additional-datatable-columns-cs/_static/image9.png))

Dans l’écran suivant, illustré à la figure 4, l’Assistant nous demande le type de requête à ajouter. Étant donné que la méthode `GetProductsWithPriceQuartile` retourne toutes les colonnes et tous les enregistrements de la table `Products`, sélectionnez l’option SELECT qui retourne des lignes, puis cliquez sur suivant.

[![notre requête sera une instruction SELECT qui retourne plusieurs lignes](adding-additional-datatable-columns-cs/_static/image11.png)](adding-additional-datatable-columns-cs/_static/image10.png)

**Figure 4**: notre requête sera une instruction `SELECT` qui retourne plusieurs lignes ([cliquez pour afficher l’image en taille réelle](adding-additional-datatable-columns-cs/_static/image12.png))

Nous sommes ensuite invités à entrer la requête `SELECT`. Entrez la requête suivante dans l’Assistant :

[!code-sql[Main](adding-additional-datatable-columns-cs/samples/sample1.sql)]

La requête ci-dessus utilise la nouvelle [fonction`NTILE`](https://msdn.microsoft.com/library/ms175126.aspx) de SQL Server 2005 s pour diviser les résultats en quatre groupes, où les groupes sont déterminés par les valeurs de `UnitPrice` triées dans l’ordre décroissant.

Malheureusement, le Générateur de requêtes ne sait pas comment analyser le mot clé `OVER` et affiche une erreur lors de l’analyse de la requête ci-dessus. Par conséquent, entrez la requête ci-dessus directement dans la zone de texte de l’Assistant sans utiliser l’Générateur de requêtes.

> [!NOTE]
> Pour plus d’informations sur les autres fonctions de classement de NTILE et SQL Server 2005, consultez [renvoi de résultats classés avec Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml) et la [section fonctions de classement](https://msdn.microsoft.com/library/ms189798.aspx) de la [documentation en ligne de SQL Server 2005](https://msdn.microsoft.com/library/ms189798.aspx).

Après avoir entré la requête `SELECT` et cliqué sur suivant, l’Assistant nous invite à fournir un nom pour la procédure stockée qu’il va créer. Nommez la nouvelle procédure stockée `Products_SelectWithPriceQuartile`, puis cliquez sur suivant.

[![nom de la procédure stockée Products_SelectWithPriceQuartile](adding-additional-datatable-columns-cs/_static/image14.png)](adding-additional-datatable-columns-cs/_static/image13.png)

**Figure 5**: nommer la procédure stockée `Products_SelectWithPriceQuartile` ([cliquez pour afficher l’image en taille réelle](adding-additional-datatable-columns-cs/_static/image15.png))

Enfin, nous sommes invités à nommer les méthodes TableAdapter. Laissez les cases à cocher remplir un DataTable et retourner un DataTable activée, puis nommez les méthodes `FillWithPriceQuartile` et `GetProductsWithPriceQuartile`.

[![nommez les méthodes TableAdapter s et cliquez sur terminer](adding-additional-datatable-columns-cs/_static/image17.png)](adding-additional-datatable-columns-cs/_static/image16.png)

**Figure 6**: nommer les méthodes des TableAdapter s et cliquer sur Terminer ([Cliquer pour afficher l’image en taille réelle](adding-additional-datatable-columns-cs/_static/image18.png))

Avec la requête `SELECT` spécifiée et la procédure stockée et les méthodes TableAdapter nommées, cliquez sur Terminer pour terminer l’Assistant. À ce stade, vous pouvez recevoir un avertissement ou deux de l’Assistant, indiquant que la construction ou l’instruction SQL `OVER` n’est pas prise en charge. Ces avertissements peuvent être ignorés.

Une fois l’Assistant terminé, le TableAdapter doit inclure les méthodes `FillWithPriceQuartile` et `GetProductsWithPriceQuartile` et la base de données doit inclure une procédure stockée nommée `Products_SelectWithPriceQuartile`. Prenez un moment pour vérifier que le TableAdapter contient effectivement cette nouvelle méthode et que la procédure stockée a été correctement ajoutée à la base de données. Lorsque vous vérifiez la base de données, si vous ne voyez pas la procédure stockée, essayez de cliquer avec le bouton droit sur le dossier procédures stockées, puis choisissez actualiser.

![Vérifier qu’une nouvelle méthode a été ajoutée au TableAdapter](adding-additional-datatable-columns-cs/_static/image19.png)

**Figure 7**: vérifier qu’une nouvelle méthode a été ajoutée au TableAdapter

[![vérifier que la base de données contient la procédure stockée Products_SelectWithPriceQuartile](adding-additional-datatable-columns-cs/_static/image21.png)](adding-additional-datatable-columns-cs/_static/image20.png)

**Figure 8**: vérifier que la base de données contient la procédure stockée `Products_SelectWithPriceQuartile` ([cliquez pour afficher l’image en taille réelle](adding-additional-datatable-columns-cs/_static/image22.png))

> [!NOTE]
> L’un des avantages de l’utilisation des procédures stockées au lieu des instructions SQL ad hoc est que la réexécution de l’Assistant Configuration de TableAdapter ne modifiera pas les listes de colonnes de procédures stockées. Pour vérifier cela, cliquez avec le bouton droit sur le TableAdapter, choisissez l’option configurer dans le menu contextuel pour démarrer l’Assistant, puis cliquez sur Terminer pour le terminer. Ensuite, accédez à la base de données et affichez la procédure stockée `Products_SelectWithPriceQuartile`. Notez que la liste des colonnes n’a pas été modifiée. Si nous avions utilisé des instructions SQL ad hoc, la réexécution de l’Assistant Configuration de TableAdapter aurait rétabli la liste des colonnes de cette requête pour qu’elle corresponde à la liste des colonnes de requête principale, supprimant ainsi l’instruction NTILE de la requête utilisée par la méthode `GetProductsWithPriceQuartile`.

Lorsque la méthode `GetProductsWithPriceQuartile` de la couche d’accès aux données est appelée, le TableAdapter exécute la procédure stockée `Products_SelectWithPriceQuartile` et ajoute une ligne au `ProductsDataTable` pour chaque enregistrement retourné. Les champs de données retournés par la procédure stockée sont mappés aux colonnes `ProductsDataTable` s. Comme il existe un champ de données `PriceQuartile` renvoyé par la procédure stockée, sa valeur est assignée à la colonne `ProductsDataTable` s `PriceQuartile`.

Pour les méthodes TableAdapter dont les requêtes ne retournent pas de champ de données `PriceQuartile`, la valeur de `PriceQuartile` colonne s est la valeur spécifiée par sa propriété `DefaultValue`. Comme le montre la figure 2, cette valeur est définie sur `DBNull`, la valeur par défaut. Si vous préférez une autre valeur par défaut, il vous suffit de définir la propriété `DefaultValue` en conséquence. Assurez-vous simplement que la valeur `DefaultValue` est valide étant donné la colonne s `DataType` (c’est-à-dire, `System.Int32` pour la colonne `PriceQuartile`).

À ce stade, nous avons effectué les étapes nécessaires pour ajouter une colonne supplémentaire à un DataTable. Pour vérifier que cette colonne supplémentaire fonctionne comme prévu, créez une page ASP.NET qui affiche chaque nom de produit, Price et Price quartile. Avant cela, cependant, nous devons tout d’abord mettre à jour la couche de logique métier pour inclure une méthode qui appelle la méthode de `GetProductsWithPriceQuartile` DAL. Nous allons mettre à jour la couche BLL suivant, à l’étape 3, puis créer la page ASP.NET à l’étape 4.

## <a name="step-3-augmenting-the-business-logic-layer"></a>Étape 3 : augmentation de la couche de logique métier

Avant d’utiliser la nouvelle méthode `GetProductsWithPriceQuartile` de la couche de présentation, nous devons d’abord ajouter une méthode correspondante à la couche BLL. Ouvrez le fichier de classe `ProductsBLLWithSprocs` et ajoutez le code suivant :

[!code-csharp[Main](adding-additional-datatable-columns-cs/samples/sample2.cs)]

À l’instar des autres méthodes de récupération de données dans `ProductsBLLWithSprocs`, la méthode `GetProductsWithPriceQuartile` appelle simplement la couche DAL correspondant `GetProductsWithPriceQuartile` méthode et retourne ses résultats.

## <a name="step-4-displaying-the-price-quartile-information-in-an-aspnet-web-page"></a>Étape 4 : affichage des informations sur le prix du quartile dans une page Web ASP.NET

À l’aide de l’ajout de la couche BLL, nous sommes prêts à créer une page ASP.NET qui indique le quartile tarifaire pour chaque produit. Ouvrez la page `AddingColumns.aspx` dans le dossier `AdvancedDAL` et faites glisser un contrôle GridView de la boîte à outils vers le concepteur, en affectant à sa propriété `ID` la valeur `Products`. À partir de la balise active GridView s, liez-le à un nouvel ObjectDataSource nommé `ProductsDataSource`. Configurez le ObjectDataSource pour utiliser la méthode de `GetProductsWithPriceQuartile` de la classe `ProductsBLLWithSprocs`. Étant donné qu’il s’agit d’une grille en lecture seule, définissez les listes déroulantes dans les onglets mettre à jour, insérer et supprimer sur (aucun).

[![configurer ObjectDataSource pour utiliser la classe ProductsBLLWithSprocs](adding-additional-datatable-columns-cs/_static/image24.png)](adding-additional-datatable-columns-cs/_static/image23.png)

**Figure 9**: configurer ObjectDataSource pour utiliser la classe `ProductsBLLWithSprocs` ([cliquez pour afficher l’image en taille réelle](adding-additional-datatable-columns-cs/_static/image25.png))

[![récupérer les informations sur le produit à partir de la méthode GetProductsWithPriceQuartile](adding-additional-datatable-columns-cs/_static/image27.png)](adding-additional-datatable-columns-cs/_static/image26.png)

**Figure 10**: récupérer des informations sur le produit à partir de la méthode `GetProductsWithPriceQuartile` ([cliquez pour afficher l’image en taille réelle](adding-additional-datatable-columns-cs/_static/image28.png))

À l’issue de l’exécution de l’Assistant Configuration de la source de données, Visual Studio ajoute automatiquement un BoundField ou un CheckBoxField au GridView pour chacun des champs de données retournés par la méthode. L’un de ces champs de données est `PriceQuartile`, qui est la colonne que nous avons ajoutée au `ProductsDataTable` à l’étape 1.

Modifiez les champs GridView s, en supprimant tous les `ProductName`, `UnitPrice`et `PriceQuartile` BoundFields. Configurez l' `UnitPrice` BoundField pour mettre en forme sa valeur en tant que devise et pour que les `UnitPrice` et `PriceQuartile` BoundFields alignés à droite et au centre, respectivement. Enfin, mettez à jour les propriétés de `HeaderText` BoundFields restantes en, Price et Price quartile, respectivement. Activez également la case à cocher Activer le tri à partir de la balise active GridView s.

Une fois ces modifications effectuées, les balises déclaratives GridView et ObjectDataSource doivent ressembler à ce qui suit :

[!code-aspx[Main](adding-additional-datatable-columns-cs/samples/sample3.aspx)]

La figure 11 illustre cette page lorsqu’elle est visitée via un navigateur. Notez que, initialement, les produits sont classés par prix dans l’ordre décroissant, avec chaque produit affecté à une valeur de `PriceQuartile` appropriée. Bien entendu, ces données peuvent être triées en fonction d’autres critères avec la valeur de la colonne du quartile tarifaire qui reflète toujours le classement du produit en ce qui concerne le prix (voir figure 12).

[![les produits sont classés par prix](adding-additional-datatable-columns-cs/_static/image30.png)](adding-additional-datatable-columns-cs/_static/image29.png)

**Figure 11**: les produits sont classés par prix ([cliquez pour afficher l’image en taille réelle](adding-additional-datatable-columns-cs/_static/image31.png))

[![les produits sont classés en fonction de leur nom](adding-additional-datatable-columns-cs/_static/image33.png)](adding-additional-datatable-columns-cs/_static/image32.png)

**Figure 12**: les produits sont classés selon leur nom ([cliquez pour afficher l’image en taille réelle](adding-additional-datatable-columns-cs/_static/image34.png))

> [!NOTE]
> Avec quelques lignes de code, nous pourrions augmenter le GridView afin qu’il colore les lignes de produits en fonction de leur valeur de `PriceQuartile`. Il est possible que les produits soient colorés dans le premier quartile en vert clair, dans le second quartile en jaune clair, etc. Je vous encourage à prendre un moment pour ajouter cette fonctionnalité. Si vous avez besoin d’un actualisateur pour la mise en forme d’un GridView, consultez le didacticiel sur la [mise en forme personnalisée basée](../custom-formatting/custom-formatting-based-upon-data-cs.md) sur les données.

## <a name="an-alternative-approach---creating-another-tableadapter"></a>Une autre approche : création d’un autre TableAdapter

Comme nous l’avons vu dans ce didacticiel, lors de l’ajout d’une méthode à un TableAdapter qui retourne des champs de données autres que ceux de la requête principale, nous pouvons ajouter des colonnes correspondantes au DataTable. Toutefois, une telle approche fonctionne bien uniquement s’il existe un petit nombre de méthodes dans le TableAdapter qui retournent des champs de données différents et si ces champs de données de remplacement ne varient pas trop de la requête principale.

Au lieu d’ajouter des colonnes au DataTable, vous pouvez ajouter un autre TableAdapter au DataSet qui contient les méthodes du premier TableAdapter qui retournent des champs de données différents. Pour ce didacticiel, au lieu d’ajouter la colonne `PriceQuartile` à la `ProductsDataTable` (où elle est utilisée uniquement par la méthode `GetProductsWithPriceQuartile`), nous aurions pu ajouter un TableAdapter supplémentaire au DataSet nommé `ProductsWithPriceQuartileTableAdapter` qui utilisait la procédure stockée `Products_SelectWithPriceQuartile` comme requête principale. Les pages ASP.NET qui devaient recevoir des informations sur le produit avec le quartile Price utilisent la `ProductsWithPriceQuartileTableAdapter`, tandis que celles qui ne pouvaient pas continuer à utiliser le `ProductsTableAdapter`.

En ajoutant un nouveau TableAdapter, les DataTables restent internis et leurs colonnes reflètent précisément les champs de données retournés par leurs méthodes TableAdapter s. Toutefois, des TableAdapters supplémentaires peuvent introduire des tâches et des fonctionnalités répétitives. Par exemple, si les pages ASP.NET qui affichaient la colonne `PriceQuartile` étaient également nécessaires pour assurer la prise en charge de l’insertion, de la mise à jour et de la suppression, les propriétés `InsertCommand`, `UpdateCommand`et `DeleteCommand` de la `ProductsWithPriceQuartileTableAdapter` doivent être correctement configurées. Bien que ces propriétés reflètent le `ProductsTableAdapter` s, cette configuration introduit une étape supplémentaire. En outre, il existe deux façons de mettre à jour, de supprimer ou d’ajouter un produit à la base de données, par le biais des classes `ProductsTableAdapter` et `ProductsWithPriceQuartileTableAdapter`.

Le téléchargement de ce didacticiel comprend une classe `ProductsWithPriceQuartileTableAdapter` dans le jeu de données `NorthwindWithSprocs` qui illustre cette approche alternative.

## <a name="summary"></a>Récapitulatif

Dans la plupart des scénarios, toutes les méthodes d’un TableAdapter retournent le même jeu de champs de données, mais il peut arriver qu’une méthode particulière ou deux doive retourner un champ supplémentaire. Par exemple, dans le [maître/détail à l’aide d’une liste à puces d’enregistrements maîtres avec un didacticiel détails sur DataList](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) , nous avons ajouté une méthode au `CategoriesTableAdapter` qui, en plus des champs de données de la requête principale, a retourné un champ `NumberOfProducts` qui a signalé le nombre de produits associés à chaque catégorie. Dans ce didacticiel, nous avons examiné l’ajout d’une méthode dans la `ProductsTableAdapter` qui a retourné un champ `PriceQuartile` en plus des champs de données de la requête principale. Pour capturer des champs de données supplémentaires retournés par les méthodes TableAdapter s, nous devons ajouter des colonnes correspondantes au DataTable.

Si vous prévoyez d’ajouter manuellement des colonnes au DataTable, il est recommandé que le TableAdapter utilise des procédures stockées. Si le TableAdapter utilise des instructions SQL ad hoc, chaque fois que l’Assistant Configuration de TableAdapter est exécuté, toutes les listes de champs de données de méthode reprennent les champs de données retournés par la requête principale. Ce problème ne s’étend pas aux procédures stockées, c’est pourquoi il est recommandé et utilisé dans ce didacticiel.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel étaient Randy Schmidt, Jacky Goor, Bernadette Leigh et Hilton Giesenow. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](updating-the-tableadapter-to-use-joins-cs.md)
> [Suivant](working-with-computed-columns-cs.md)
