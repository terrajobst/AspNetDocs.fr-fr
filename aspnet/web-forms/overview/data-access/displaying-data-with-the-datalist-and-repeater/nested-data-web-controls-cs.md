---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-cs
title: Contrôles Web de données imbriquéesC#() | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons découvrir comment utiliser un répéteur imbriqué dans un autre répéteur. Les exemples illustrent comment remplir le répéteur interne à la fois d...
ms.author: riande
ms.date: 09/13/2006
ms.assetid: ad3cb0ec-26cf-42d7-b81b-184a34ec9f86
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 8ef15bebb2c29976274b0cca1d6ace434ccc55ce
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74640264"
---
# <a name="nested-data-web-controls-c"></a>Contrôles web de données imbriquées (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_32_CS.exe) ou [Télécharger le PDF](nested-data-web-controls-cs/_static/datatutorial32cs1.pdf)

> Dans ce didacticiel, nous allons découvrir comment utiliser un répéteur imbriqué dans un autre répéteur. Les exemples illustrent comment remplir le répéteur interne à la fois de façon déclarative et par programme.

## <a name="introduction"></a>Introduction

En plus de la syntaxe statique HTML et DataBinding, les modèles peuvent également inclure des contrôles Web et des contrôles utilisateur. Ces contrôles Web peuvent avoir leurs propriétés assignées par le biais de la syntaxe déclarative, DataBinding, ou être accessibles par programme dans les gestionnaires d’événements côté serveur appropriés.

En incorporant des contrôles dans un modèle, l’apparence et l’expérience utilisateur peuvent être personnalisées et améliorées sur. Par exemple, dans le didacticiel [utilisation de TemplateFields dans le contrôle GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) , nous avons vu comment personnaliser l’affichage des GridView s en ajoutant un contrôle de calendrier dans un TemplateField pour afficher la date d’embauche d’un employé. dans [Ajout de contrôles de validation aux interfaces de modification et d’insertion](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) et [Personnalisation des](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) didacticiels sur l’interface de modification des données, nous avons vu comment personnaliser les interfaces de modification et d’insertion en ajoutant des contrôles de validation, des zones de texte, des DropDownList et d’autres contrôles Web.

Les modèles peuvent également contenir d’autres contrôles Web de données. Autrement dit, nous pouvons avoir un contrôle DataList qui contient un autre contrôle DataList (ou Repeater ou GridView ou DetailsView, etc.) dans ses modèles. Le défi avec une telle interface consiste à lier les données appropriées au contrôle Web des données internes. Il existe plusieurs approches disponibles, allant des options déclaratives utilisant ObjectDataSource à celles de programmation.

Dans ce didacticiel, nous allons découvrir comment utiliser un répéteur imbriqué dans un autre répéteur. Le répétiteur externe contient un élément pour chaque catégorie dans la base de données, affichant le nom et la description de la catégorie. Chaque répéteur interne d’élément de catégorie affiche des informations pour chaque produit appartenant à cette catégorie (voir figure 1) dans une liste à puces. Nos exemples illustrent comment remplir le répéteur interne à la fois de façon déclarative et par programme.

[![chaque catégorie, ainsi que ses produits, sont répertoriés](nested-data-web-controls-cs/_static/image2.png)](nested-data-web-controls-cs/_static/image1.png)

**Figure 1**: chaque catégorie, ainsi que ses produits, sont répertoriés ([cliquez pour afficher l’image en taille réelle](nested-data-web-controls-cs/_static/image3.png))

## <a name="step-1-creating-the-category-listing"></a>Étape 1 : création de la liste des catégories

Lors de la création d’une page qui utilise des contrôles Web de données imbriqués, je trouve qu’il est utile de concevoir, de créer et de tester d’abord le contrôle Web de données le plus à l’extérieur, sans même vous soucier du contrôle imbriqué interne. Par conséquent, commençons par parcourir les étapes nécessaires pour ajouter un Repeater à la page qui répertorie le nom et la description de chaque catégorie.

Commencez par ouvrir la page `NestedControls.aspx` dans le dossier `DataListRepeaterBasics` et ajoutez un contrôle Repeater à la page, en affectant à sa propriété `ID` la valeur `CategoryList`. À partir de la balise active de Repeater s, choisissez de créer un ObjectDataSource nommé `CategoriesDataSource`.

[![nommez le nouveau ObjectDataSource CategoriesDataSource](nested-data-web-controls-cs/_static/image5.png)](nested-data-web-controls-cs/_static/image4.png)

**Figure 2**: nommer le nouvel ObjectDataSource `CategoriesDataSource` ([cliquez pour afficher l’image en taille réelle](nested-data-web-controls-cs/_static/image6.png))

Configurez le ObjectDataSource afin qu’il extraie ses données de la méthode de `GetCategories` de la classe `CategoriesBLL`.

[![configurer ObjectDataSource pour utiliser la méthode GetCategories de la classe CategoriesBLL s](nested-data-web-controls-cs/_static/image8.png)](nested-data-web-controls-cs/_static/image7.png)

**Figure 3**: configurer ObjectDataSource pour utiliser la méthode `CategoriesBLL` classe s `GetCategories` ([cliquez pour afficher l’image en taille réelle](nested-data-web-controls-cs/_static/image9.png))

Pour spécifier le contenu du modèle Repeater s, vous devez accéder à la vue source et entrer manuellement la syntaxe déclarative. Ajoutez un `ItemTemplate` qui affiche le nom de la catégorie dans un élément `<h4>` et la description de la catégorie dans un élément paragraph (`<p>`). En outre, nous allons séparer chaque catégorie par une règle horizontale (`<hr>`). Après avoir apporté ces modifications, votre page doit contenir une syntaxe déclarative pour le Repeater et ObjectDataSource similaire à ce qui suit :

[!code-aspx[Main](nested-data-web-controls-cs/samples/sample1.aspx)]

La figure 4 illustre notre progression dans un navigateur.

[![chaque nom et description de catégorie est listé, séparé par une règle horizontale](nested-data-web-controls-cs/_static/image11.png)](nested-data-web-controls-cs/_static/image10.png)

**Figure 4**: chaque nom et description de catégorie est listé, séparé par une règle horizontale ([cliquez pour afficher l’image en taille réelle](nested-data-web-controls-cs/_static/image12.png))

## <a name="step-2-adding-the-nested-product-repeater"></a>Étape 2 : ajout du répéteur de produit imbriqué

Une fois la liste des catégories terminée, la tâche suivante consiste à ajouter un répéteur au `CategoryList` s `ItemTemplate` qui affiche des informations sur les produits appartenant à la catégorie appropriée. Il existe plusieurs façons de récupérer les données pour ce répéteur interne, dont deux nous allons explorer bientôt. Pour le moment, il suffit de créer le répéteur Products dans le `ItemTemplate``CategoryList` Repeater s. En particulier, laissez le répéteur de produit afficher chaque produit dans une liste à puces avec chaque élément de liste, y compris le nom et le prix du produit.

Pour créer ce répéteur, nous devons entrer manuellement la syntaxe déclarative et les modèles Inner Repeater s dans le `ItemTemplate``CategoryList` s. Ajoutez le balisage suivant dans le `ItemTemplate``CategoryList` Repeater :

[!code-aspx[Main](nested-data-web-controls-cs/samples/sample2.aspx)]

## <a name="step-3-binding-the-category-specific-products-to-the-productsbycategorylist-repeater"></a>Étape 3 : liaison des produits spécifiques aux catégories au répétiteur ProductsByCategoryList

Si vous accédez à la page par le biais d’un navigateur à ce stade, votre écran sera le même que dans la figure 4, car nous avons encore lié les données au répéteur. Il existe plusieurs façons de récupérer les enregistrements de produits appropriés et de les lier au répéteur, plus efficaces que d’autres. Le principal défi ici est de récupérer les produits appropriés pour la catégorie spécifiée.

Les données à lier au contrôle de répéteur interne sont accessibles de façon déclarative, par le biais d’un ObjectDataSource dans le `CategoryList` Repeater s `ItemTemplate`, ou par programme, à partir de la page code-behind de la page ASP.NET. De même, ces données peuvent être liées au répéteur interne de façon déclarative, par le biais de la propriété Inner Repeater s `DataSourceID` ou par le biais d’une syntaxe de liaison de données déclarative ou par programme, en référençant le Repeater interne dans le gestionnaire d’événements `CategoryList` Repeater s `ItemDataBound`, en définissant sa propriété `DataSource` par programmation et en appelant sa méthode `DataBind()`. Examinons chacune de ces approches.

## <a name="accessing-the-data-declaratively-with-an-objectdatasource-control-and-theitemdataboundevent-handler"></a>Accès aux données de façon déclarative à l’aide d’un contrôle ObjectDataSource et du gestionnaire d’événements`ItemDataBound`

Étant donné que nous avons largement utilisé ObjectDataSource dans cette série de didacticiels, le choix le plus naturel pour accéder aux données de cet exemple consiste à respecter l’ObjectDataSource. La classe `ProductsBLL` a une méthode `GetProductsByCategoryID(categoryID)` qui retourne des informations sur les produits qui appartiennent au *`categoryID`* spécifié. Par conséquent, nous pouvons ajouter un ObjectDataSource aux `CategoryList` Repeater s `ItemTemplate` et le configurer pour accéder à ses données à partir de cette méthode de classe s.

Malheureusement, le Repeater n’autorise pas la modification de ses modèles via le Mode Création donc nous devons ajouter la syntaxe déclarative pour ce contrôle ObjectDataSource à la main. La syntaxe suivante montre le `ItemTemplate` `CategoryList` Repeater s après l’ajout de ce nouvel ObjectDataSource (`ProductsByCategoryDataSource`) :

[!code-aspx[Main](nested-data-web-controls-cs/samples/sample3.aspx)]

Lorsque vous utilisez l’approche ObjectDataSource, nous devons définir la propriété `ProductsByCategoryList` Repeater s `DataSourceID` sur la `ID` de l’ObjectDataSource (`ProductsByCategoryDataSource`). Notez également que notre ObjectDataSource a un élément `<asp:Parameter>` qui spécifie la valeur *`categoryID`* qui sera passée à la méthode `GetProductsByCategoryID(categoryID)`. Mais comment spécifier cette valeur ? Idéalement, nous sommes en mesure de définir simplement la propriété `DefaultValue` de l’élément `<asp:Parameter>` à l’aide de la syntaxe DataBinding, comme suit :

[!code-aspx[Main](nested-data-web-controls-cs/samples/sample4.aspx)]

Malheureusement, la syntaxe de liaison de liaison n’est valide que dans les contrôles qui ont un événement `DataBinding`. La classe `Parameter` ne dispose pas d’un tel événement et, par conséquent, la syntaxe ci-dessus est illégale et génère une erreur d’exécution.

Pour définir cette valeur, vous devez créer un gestionnaire d’événements pour l’événement `CategoryList` Repeater s `ItemDataBound`. Rappelez-vous que l’événement `ItemDataBound` se déclenche une fois pour chaque élément lié au répéteur. Par conséquent, chaque fois que cet événement est déclenché pour le répéteur externe, nous pouvons affecter la valeur actuelle `CategoryID` au paramètre `ProductsByCategoryDataSource` ObjectDataSource s `CategoryID`.

Créez un gestionnaire d’événements pour l’événement `CategoryList` Repeater s `ItemDataBound` à l’aide du code suivant :

[!code-csharp[Main](nested-data-web-controls-cs/samples/sample5.cs)]

Ce gestionnaire d’événements commence par s’assurer que nous retraitons un élément de données plutôt que l’en-tête, le pied de page ou l’élément de séparateur. Ensuite, nous référençons l’instance de `CategoriesRow` réelle qui vient d’être liée à la `RepeaterItem`actuelle. Enfin, nous référençons l’ObjectDataSource dans le `ItemTemplate` et affectons sa valeur de paramètre `CategoryID` à la `CategoryID` du `RepeaterItem`actuel.

Avec ce gestionnaire d’événements, le `ProductsByCategoryList` Repeater dans chaque `RepeaterItem` est lié à ces produits dans la catégorie `RepeaterItem` s. La figure 5 illustre une capture d’écran de la sortie obtenue.

[![le répéteur externe répertorie chaque catégorie ; la partie interne répertorie les produits pour cette catégorie](nested-data-web-controls-cs/_static/image14.png)](nested-data-web-controls-cs/_static/image13.png)

**Figure 5**: le répéteur externe répertorie chaque catégorie ; la partie interne répertorie les produits pour cette catégorie ([cliquez pour afficher l’image en taille réelle](nested-data-web-controls-cs/_static/image15.png))

## <a name="accessing-the-products-by-category-data-programmatically"></a>Accès aux produits par catégorie données par programmation

Au lieu d’utiliser un ObjectDataSource pour récupérer les produits de la catégorie actuelle, nous pourrions créer une méthode dans la classe code-behind de la page ASP.NET s (ou dans le dossier `App_Code` ou dans un projet de bibliothèque de classes distinct) qui retourne l’ensemble de produits approprié lorsqu’il est passé dans une `CategoryID`. Imaginez que nous avions une telle méthode dans la classe code-behind de la page ASP.NET s et qu’elle était nommée `GetProductsInCategory(categoryID)`. Une fois cette méthode en place, nous pourrions lier les produits de la catégorie actuelle au répéteur interne à l’aide de la syntaxe déclarative suivante :

[!code-aspx[Main](nested-data-web-controls-cs/samples/sample6.aspx)]

La propriété Repeater s `DataSource` utilise la syntaxe DataBinding pour indiquer que ses données proviennent de la méthode `GetProductsInCategory(categoryID)`. Étant donné que `Eval("CategoryID")` retourne une valeur de type `Object`, nous castons l’objet en un `Integer` avant de le passer à la méthode `GetProductsInCategory(categoryID)`. Notez que le `CategoryID` accessible via la syntaxe DataBinding est le `CategoryID` dans le Repeater *externe* (`CategoryList`), celui qui est lié aux enregistrements dans la table `Categories`. Par conséquent, nous savons que `CategoryID` ne peut pas être une valeur de `NULL` de base de données, c’est pourquoi nous pouvons convertier la méthode de `Eval` sans vérifier si nous reprenons un `DBNull`.

Avec cette approche, nous devons créer la méthode `GetProductsInCategory(categoryID)` et faire en sorte qu’elle récupère l’ensemble de produits approprié en fonction du *`categoryID`* fourni. Pour ce faire, il suffit de retourner le `ProductsDataTable` retourné par la méthode `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)`. Créons la méthode `GetProductsInCategory(categoryID)` dans la classe code-behind pour notre page de `NestedControls.aspx`. Pour ce faire, utilisez le code suivant :

[!code-csharp[Main](nested-data-web-controls-cs/samples/sample7.cs)]

Cette méthode crée simplement une instance de la méthode `ProductsBLL` et retourne les résultats de la méthode `GetProductsByCategoryID(categoryID)`. Notez que la méthode doit être marquée `Public` ou `Protected`; Si la méthode est marquée `Private`, elle ne sera pas accessible à partir du balisage déclaratif de la page ASP.NET.

Après avoir apporté ces modifications pour utiliser cette nouvelle technique, prenez un moment pour afficher la page dans un navigateur. La sortie doit être identique à la sortie lorsque vous utilisez l’approche ObjectDataSource et `ItemDataBound` gestionnaire d’événements (reportez-vous à la figure 5 pour voir une capture d’écran).

> [!NOTE]
> Il peut sembler que charge crée la méthode `GetProductsInCategory(categoryID)` dans la classe code-behind ASP.NET page s. Après tout, cette méthode crée simplement une instance de la classe `ProductsBLL` et retourne les résultats de sa méthode `GetProductsByCategoryID(categoryID)`. Pourquoi ne pas simplement appeler cette méthode directement à partir de la syntaxe DataBinding dans le Repeater interne, comme : `DataSource='<%# ProductsBLL.GetProductsByCategoryID((int)(Eval("CategoryID"))) %>'`? Bien que cette syntaxe ne fonctionne pas avec notre implémentation actuelle de la classe `ProductsBLL` (étant donné que la méthode `GetProductsByCategoryID(categoryID)` est une méthode d’instance), vous pouvez modifier `ProductsBLL` pour inclure une méthode `GetProductsByCategoryID(categoryID)` statique ou faire en sorte que la classe inclue une méthode `Instance()` statique pour retourner une nouvelle instance de la classe `ProductsBLL`.

Bien que ces modifications éliminent le besoin de la méthode `GetProductsInCategory(categoryID)` dans la classe code-behind de la page ASP.NET, la méthode de la classe code-behind nous offre plus de flexibilité dans l’utilisation des données récupérées, comme nous le verrons bientôt.

## <a name="retrieving-all-of-the-product-information-at-once"></a>Récupération de toutes les informations sur le produit en même temps

Les deux techniques précédemment déplacées que nous avons examinées récupèrent ces produits pour la catégorie actuelle en effectuant un appel à la méthode de la classe `ProductsBLL` s `GetProductsByCategoryID(categoryID)` (la première approche effectuée par le biais d’un ObjectDataSource, la seconde via la méthode `GetProductsInCategory(categoryID)` dans la classe code-behind). Chaque fois que cette méthode est appelée, la couche de logique métier appelle la couche d’accès aux données, qui interroge la base de données avec une instruction SQL qui retourne les lignes de la table `Products` dont le champ `CategoryID` correspond au paramètre d’entrée fourni.

Étant donné *n* catégories dans le système, cette approche reporte *n* + 1 appels à la base de données une requête de base de données pour obtenir toutes les catégories, puis *n* appels pour obtenir les produits spécifiques à chaque catégorie. Toutefois, nous pouvons récupérer toutes les données nécessaires dans deux appels de base de données, un appel pour obtenir toutes les catégories et une autre pour obtenir tous les produits. Une fois que nous disposons de tous les produits, nous pouvons filtrer ces produits afin que seuls les produits correspondant à la `CategoryID` actuelle soient liés à ce répéteur interne de catégorie.

Pour fournir cette fonctionnalité, nous devons uniquement apporter une légère modification à la méthode `GetProductsInCategory(categoryID)` dans notre classe code-behind de la page ASP.NET. Au lieu de retourner aveuglément les résultats de la méthode de `GetProductsByCategoryID(categoryID)` de la classe `ProductsBLL`, nous pouvons à la place accéder tout d’abord à *tous* les produits (s’ils n’ont pas encore été consultés), puis retourner uniquement la vue filtrée des produits en fonction de la `CategoryID`transmise.

[!code-csharp[Main](nested-data-web-controls-cs/samples/sample8.cs)]

Notez l’ajout de la variable au niveau de la page, `allProducts`. Elle contient des informations sur tous les produits et est remplie la première fois que la méthode `GetProductsInCategory(categoryID)` est appelée. Après vous être assuré que l’objet `allProducts` a été créé et rempli, la méthode filtre les résultats de DataTable, de telle sorte que seules les lignes dont `CategoryID` correspond à la `CategoryID` spécifiée sont accessibles. Cette approche réduit le nombre de fois où la base de données est accessible de *N* + 1 à deux.

Cette amélioration n’introduit aucune modification dans le balisage rendu de la page et ne remet pas non plus moins d’enregistrements que l’autre approche. Cela réduit simplement le nombre d’appels à la base de données.

> [!NOTE]
> Il est possible que la réduction du nombre d’accès à la base de données améliore les performances de manière intuitive. Toutefois, cela n’est peut-être pas le cas. Si vous avez un grand nombre de produits dont `CategoryID` est `NULL`, par exemple, l’appel à la méthode `GetProducts` retourne un certain nombre de produits qui ne sont jamais affichés. En outre, le retour de tous les produits peut s’avérer inutile si vous n’avez affiché qu’un sous-ensemble des catégories, ce qui peut être le cas si vous avez implémenté la pagination.

Comme toujours, lorsqu’il s’agit d’analyser les performances de deux techniques, la seule mesure plaisanter consiste à exécuter des tests contrôlés adaptés aux scénarios de cas courants de votre application.

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, nous avons vu comment imbriquer un contrôle Web de données dans un autre, en examinant en particulier comment avoir un répéteur externe pour afficher un élément pour chaque catégorie avec un répéteur interne répertoriant les produits pour chaque catégorie dans une liste à puces. Le principal défi de la création d’une interface utilisateur imbriquée réside dans l’accès et la liaison des données correctes au contrôle Web de données internes. Il existe une variété de techniques disponibles, dont deux ont été examinées dans ce didacticiel. La première approche examinée utilisait un ObjectDataSource dans le contrôle Web de données externe s `ItemTemplate` qui était lié au contrôle Web des données internes par le biais de sa propriété `DataSourceID`. La deuxième technique a accédé aux données via une méthode dans la classe code-behind de la page ASP.NET. Cette méthode peut ensuite être liée à la propriété interne Data Web s `DataSource` par le biais de la syntaxe DataBinding.

Tandis que l’interface utilisateur imbriquée examinée dans ce didacticiel utilisait un Repeater imbriqué dans un Repeater, ces techniques peuvent être étendues aux autres contrôles Web de données. Vous pouvez imbriquer un Repeater dans un GridView, un GridView dans un contrôle DataList, et ainsi de suite.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel étaient Zack Jones et Liz Shulok. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](showing-multiple-records-per-row-with-the-datalist-control-cs.md)
> [Suivant](displaying-data-with-the-datalist-and-repeater-controls-vb.md)
