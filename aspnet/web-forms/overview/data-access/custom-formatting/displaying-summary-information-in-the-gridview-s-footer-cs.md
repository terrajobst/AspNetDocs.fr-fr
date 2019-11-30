---
uid: web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs
title: Affichage des informations récapitulatives dans le pied de pageC#du GridView () | Microsoft Docs
author: rick-anderson
description: Les informations de résumé sont souvent affichées en bas du rapport dans une ligne de synthèse. Le contrôle GridView peut inclure une ligne de pied de page dans dont les cellules peuvent être pr...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: d50edc31-9286-4c6a-8635-be09e72752a4
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs
msc.type: authoredcontent
ms.openlocfilehash: b258a2bdeaea8da4e9c5c5d8043b167d94e1e817
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74617003"
---
# <a name="displaying-summary-information-in-the-gridviews-footer-c"></a>Affichage des informations récapitulatives dans le pied de page du GridView (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_15_CS.exe) ou [Télécharger le PDF](displaying-summary-information-in-the-gridview-s-footer-cs/_static/datatutorial15cs1.pdf)

> Les informations de résumé sont souvent affichées en bas du rapport dans une ligne de synthèse. Le contrôle GridView peut inclure une ligne de pied de page dans dont les cellules peuvent injecter des données agrégées par programmation. Dans ce didacticiel, nous allons voir comment afficher des données agrégées dans cette ligne de pied de page.

## <a name="introduction"></a>Introduction

En plus de voir les prix des produits, les unités en stock, les unités commandées et les niveaux de réorganisation, un utilisateur peut également être intéressé par des informations agrégées, telles que le prix moyen, le nombre total d’unités en stock, etc. Ces informations récapitulatives sont souvent affichées en bas du rapport dans une ligne de synthèse. Le contrôle GridView peut inclure une ligne de pied de page dans dont les cellules peuvent injecter des données agrégées par programmation.

Cette tâche présente trois défis :

1. Configuration du contrôle GridView pour afficher sa ligne de pied de page
2. Détermination des données de synthèse ; autrement dit, comment calculer le prix moyen ou le total des unités en stock ?
3. Injection des données de synthèse dans les cellules appropriées de la ligne de pied de page

Dans ce didacticiel, nous allons voir comment surmonter ces défis. Plus précisément, nous allons créer une page qui répertorie les catégories dans une liste déroulante avec les produits de la catégorie sélectionnée affichés dans un GridView. Le GridView inclut une ligne de pied de page qui indique le prix moyen et le nombre total d’unités en stock et sur la commande pour les produits de cette catégorie.

[![informations récapitulatives sont affichées dans la ligne de pied de page du GridView](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image2.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image1.png)

**Figure 1**: les informations de résumé sont affichées dans la ligne de pied de page du GridView ([cliquez pour afficher l’image en taille réelle](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image3.png))

Ce didacticiel, avec sa catégorie d’interface maître/détail de produits, s’appuie sur les concepts abordés dans le précédent [filtrage maître/détail avec un didacticiel DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) . Si vous n’avez pas encore travaillé dans le didacticiel précédent, veuillez le faire avant de continuer avec celui-ci.

## <a name="step-1-adding-the-categories-dropdownlist-and-products-gridview"></a>Étape 1 : ajout des catégories DropDownList et Products GridView

Avant d’ajouter des informations de synthèse au pied de page du GridView, nous allons tout d’abord créer simplement le rapport maître/détail. Une fois cette première étape terminée, nous verrons comment inclure des données de synthèse.

Commencez par ouvrir la page `SummaryDataInFooter.aspx` dans le dossier `CustomFormatting`. Ajoutez un contrôle DropDownList et affectez à son `ID` la valeur `Categories`. Ensuite, cliquez sur le lien choisir la source de données à partir de la balise active de DropDownList et choisissez d’ajouter un nouvel ObjectDataSource nommé `CategoriesDataSource` qui appelle la méthode de `GetCategories()` de la classe `CategoriesBLL`.

[![ajouter un nouvel ObjectDataSource nommé CategoriesDataSource](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image5.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image4.png)

**Figure 2**: ajouter un nouvel ObjectDataSource nommé `CategoriesDataSource` ([cliquez pour afficher l’image en taille réelle](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image6.png))

[![que ObjectDataSource appelle la méthode GetCategories () de la classe CategoriesBLL](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image8.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image7.png)

**Figure 3**: demander à ObjectDataSource d’appeler la méthode `GetCategories()` de la classe `CategoriesBLL` ([cliquez pour afficher l’image en taille réelle](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image9.png))

Après la configuration de l’ObjectDataSource, l’Assistant vous renvoie à l’Assistant Configuration de source de données DropDownList à partir duquel nous devons spécifier la valeur du champ de données à afficher et celle qui doit correspondre à la valeur de la `ListItem` s du DropDownList. Affichez le champ `CategoryName` et utilisez le `CategoryID` comme valeur.

[![utiliser les champs CategoryName et CategoryID comme texte et valeur pour les ListItems, respectivement](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image11.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image10.png)

**Figure 4**: utiliser les champs `CategoryName` et `CategoryID` comme `Text` et `Value` pour la `ListItem` s, respectivement ([cliquez pour afficher l’image en taille réelle](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image12.png))

À ce stade, nous disposons d’un DropDownList (`Categories`) qui répertorie les catégories dans le système. Nous devons maintenant ajouter un GridView qui répertorie les produits appartenant à la catégorie sélectionnée. Avant cela, cependant, prenez un moment pour vérifier la case à cocher Activer AutoPostBack dans la balise active de DropDownList. Comme indiqué dans le didacticiel *maître/détail de filtrage avec DropDownList* , en affectant à la propriété `AutoPostBack` de DropDownList la valeur `true` la page est publiée chaque fois que la valeur DropDownList est modifiée. Cela entraînera l’actualisation du contrôle GridView, en présentant les produits de la catégorie nouvellement sélectionnée. Si la propriété `AutoPostBack` est définie sur `false` (valeur par défaut), la modification de la catégorie n’entraîne pas de publication et par conséquent ne met pas à jour les produits listés.

[![activez la case à cocher Activer AutoPostBack dans la balise active de DropDownList](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image14.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image13.png)

**Figure 5**: activer la case à cocher Activer AutoPostBack dans la balise active de DropDownList ([cliquez pour afficher l’image en taille réelle](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image15.png))

Ajoutez un contrôle GridView à la page afin d’afficher les produits pour la catégorie sélectionnée. Définissez le `ID` du GridView sur `ProductsInCategory` et liez-le à un nouvel ObjectDataSource nommé `ProductsInCategoryDataSource`.

[![ajouter un nouvel ObjectDataSource nommé ProductsInCategoryDataSource](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image17.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image16.png)

**Figure 6**: ajouter un nouvel ObjectDataSource nommé `ProductsInCategoryDataSource` ([cliquez pour afficher l’image en taille réelle](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image18.png))

Configurez l’ObjectDataSource afin qu’il appelle la méthode `GetProductsByCategoryID(categoryID)` de la classe `ProductsBLL`.

[![que ObjectDataSource appelle la méthode GetProductsByCategoryID (categoryID)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image20.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image19.png)

**Figure 7**: demander à ObjectDataSource d’appeler la méthode `GetProductsByCategoryID(categoryID)` ([cliquez pour afficher l’image en taille réelle](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image21.png))

Étant donné que la méthode `GetProductsByCategoryID(categoryID)` prend un paramètre d’entrée, à l’étape finale de l’Assistant, nous pouvons spécifier la source de la valeur de paramètre. Pour afficher ces produits à partir de la catégorie sélectionnée, le paramètre doit être extrait de la `Categories` DropDownList.

[![d’accéder à la valeur du paramètre categoryID à partir du DropDownList des catégories sélectionnées](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image23.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image22.png)

**Figure 8**: obtenir la valeur du paramètre *`categoryID`* à partir du DropDownList des catégories sélectionnées ([cliquez pour afficher l’image en taille réelle](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image24.png))

Une fois l’Assistant terminé, le contrôle GridView aura un BoundField pour chacune des propriétés de produit. Nous allons nettoyer ces BoundFields afin que seuls les `ProductName`, `UnitPrice`, `UnitsInStock`et `UnitsOnOrder` BoundFields soient affichés. N’hésitez pas à ajouter des paramètres au niveau du champ aux BoundFields restants (comme la mise en forme de la `UnitPrice` comme une devise). Après avoir apporté ces modifications, le balisage déclaratif de GridView doit ressembler à ce qui suit :

[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample1.aspx)]

À ce stade, nous disposons d’un rapport maître/détail entièrement fonctionnel qui indique le nom, le prix unitaire, les unités en stock et les unités pour les produits appartenant à la catégorie sélectionnée.

[![d’accéder à la valeur du paramètre categoryID à partir du DropDownList des catégories sélectionnées](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image26.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image25.png)

**Figure 9**: obtenir la valeur du paramètre *`categoryID`* à partir du DropDownList des catégories sélectionnées ([cliquez pour afficher l’image en taille réelle](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image27.png))

## <a name="step-2-displaying-a-footer-in-the-gridview"></a>Étape 2 : affichage d’un pied de page dans le GridView

Le contrôle GridView peut afficher à la fois une ligne d’en-tête et de pied de page. Ces lignes sont affichées en fonction des valeurs des propriétés `ShowHeader` et `ShowFooter`, respectivement, avec `ShowHeader` par défaut `true` et `ShowFooter` à `false`. Pour inclure un pied de page dans le GridView, définissez simplement sa propriété `ShowFooter` sur `true`.

[![affectez la valeur true à la propriété ShowFooter de GridView.](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image29.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image28.png)

**Figure 10**: définir la propriété `ShowFooter` de GridView sur `true` ([cliquez pour afficher l’image en taille réelle](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image30.png))

La ligne de pied de page contient une cellule pour chacun des champs définis dans le GridView ; Toutefois, ces cellules sont vides par défaut. Prenez un moment pour voir notre progression dans un navigateur. Avec la propriété `ShowFooter` maintenant définie sur `true`, le contrôle GridView contient une ligne de pied de page vide.

[![le contrôle GridView contient désormais une ligne de pied de page](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image32.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image31.png)

**Figure 11**: le contrôle GridView comprend désormais une ligne de pied de page ([cliquez pour afficher l’image en taille réelle](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image33.png))

La ligne de pied de page de la figure 11 ne se distingue pas, car elle a un arrière-plan blanc. Créons une classe CSS `FooterStyle` dans `Styles.css` qui spécifie un arrière-plan rouge foncé, puis configurez le fichier d’apparence `GridView.skin` dans le thème `DataWebControls` pour assigner cette classe CSS à la propriété `FooterStyle`de l' `CssClass` du contrôle GridView. Si vous avez besoin d’effectuer une mise en forme sur des apparences et des thèmes, reportez-vous au didacticiel [affichage des données avec ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) .

Commencez par ajouter la classe CSS suivante à `Styles.css`:

[!code-css[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample2.css)]

La classe CSS `FooterStyle` est semblable à celle de la classe `HeaderStyle`, bien que la couleur d’arrière-plan du `HeaderStyle`soit plus foncée et que son texte s’affiche dans une police en gras. En outre, le texte du pied de page est aligné à droite, tandis que le texte de l’en-tête est centré.

Ensuite, pour associer cette classe CSS à chaque pied de page de GridView, ouvrez le fichier `GridView.skin` dans le thème `DataWebControls` et définissez la propriété `CssClass` de `FooterStyle`. Après cet ajout, le balisage du fichier doit ressembler à ce qui suit :

[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample3.aspx)]

Comme le montre la capture d’écran ci-dessous, cette modification rend le pied de page plus clairement.

[![la ligne de pied de page du GridView a maintenant une couleur d’arrière-plan rougeâtre](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image35.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image34.png)

**Figure 12**: la ligne de pied de page du GridView a maintenant une couleur d’arrière-plan rougeâtre ([cliquez pour afficher l’image en taille réelle](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image36.png))

## <a name="step-3-computing-the-summary-data"></a>Étape 3 : calcul des données de synthèse

Le pied de page du GridView étant affiché, le défi suivant est de savoir comment calculer les données de synthèse. Il existe deux façons de calculer ces informations d’agrégation :

1. À l’aide d’une requête SQL, nous pourrions émettre une requête supplémentaire à la base de données pour calculer les données de synthèse d’une catégorie particulière. SQL comprend un certain nombre de fonctions d’agrégation, ainsi qu’une clause `GROUP BY` pour spécifier les données sur lesquelles les données doivent être résumées. La requête SQL suivante permet de rétablir les informations nécessaires :  

    [!code-sql[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample4.sql)]

    Bien entendu, vous ne souhaitez pas émettre cette requête directement à partir de la page `SummaryDataInFooter.aspx`, mais plutôt en créant une méthode dans le `ProductsTableAdapter` et le `ProductsBLL`.
2. Calculez ces informations au fur et à mesure qu’elles sont ajoutées au contrôle GridView, comme indiqué dans la [page mise en forme personnalisée basée](custom-formatting-based-upon-data-cs.md) sur les données, le gestionnaire d’événements `RowDataBound` du GridView se déclenche une fois pour chaque ligne ajoutée au contrôle GridView après avoir été lié aux données. En créant un gestionnaire d’événements pour cet événement, nous pouvons conserver un total cumulé des valeurs que vous souhaitez agréger. Une fois que la dernière ligne de données a été liée au GridView, nous avons les totaux et les informations nécessaires pour calculer la moyenne.

J’utilise généralement la deuxième approche, car elle enregistre un voyage dans la base de données et l’effort nécessaire pour implémenter la fonctionnalité de résumé dans la couche d’accès aux données et la couche de logique métier, mais l’une ou l’autre approche suffirait. Pour ce didacticiel, nous allons utiliser la deuxième option et effectuer le suivi du total cumulé à l’aide du gestionnaire d’événements `RowDataBound`.

Créez un gestionnaire d’événements `RowDataBound` pour le contrôle GridView en sélectionnant le contrôle GridView dans le concepteur, en cliquant sur l’icône représentant un éclair dans le Fenêtre Propriétés et en double-cliquant sur l’événement `RowDataBound`. Cela permet de créer un nouveau gestionnaire d’événements nommé `ProductsInCategory_RowDataBound` dans la classe code-behind de la page `SummaryDataInFooter.aspx`.

[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample5.cs)]

Pour maintenir un total cumulé, nous devons définir des variables en dehors de l’étendue du gestionnaire d’événements. Créez les quatre variables suivantes au niveau de la page :

- `_totalUnitPrice`, de type `decimal`
- `_totalNonNullUnitPriceCount`, de type `int`
- `_totalUnitsInStock`, de type `int`
- `_totalUnitsOnOrder`, de type `int`

Ensuite, écrivez le code pour incrémenter ces trois variables pour chaque ligne de données rencontrée dans le gestionnaire d’événements `RowDataBound`.

[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample6.cs)]

Le gestionnaire d’événements `RowDataBound` démarre en vous assurant que nous traitons un DataRow. Une fois cette opération établie, l' `Northwind.ProductsRow` instance qui vient d’être liée à l’objet `GridViewRow` dans `e.Row` est stockée dans la variable `product`. Ensuite, l’exécution des variables totales est incrémentée par les valeurs correspondantes du produit en cours (en supposant qu’elles ne contiennent pas de base de données `NULL` valeur). Nous effectuons le suivi du total des `UnitPrice` en cours d’exécution et du nombre d’enregistrements non`NULL` `UnitPrice`, car le prix moyen est le quotient de ces deux nombres.

## <a name="step-4-displaying-the-summary-data-in-the-footer"></a>Étape 4 : affichage des données de synthèse dans le pied de page

Avec les données de synthèse totalisées, la dernière étape consiste à les afficher dans la ligne de pied de page du GridView. Cette tâche peut également être accomplie par programme par le biais du gestionnaire d’événements `RowDataBound`. Rappelez-vous que le gestionnaire d’événements `RowDataBound` se déclenche pour *chaque* ligne liée au GridView, y compris la ligne de pied de page. Par conséquent, nous pouvons augmenter notre gestionnaire d’événements pour afficher les données dans la ligne de pied de page à l’aide du code suivant :

[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample7.cs)]

Étant donné que la ligne de pied de page est ajoutée au contrôle GridView après l’ajout de toutes les lignes de données, nous pouvons être sûrs que, lorsque nous sommes prêts à afficher les données de synthèse dans le pied de page, le nombre total d’exécutions est terminé. La dernière étape consiste à définir ces valeurs dans les cellules du pied de page.

Pour afficher du texte dans une cellule de pied de page particulière, utilisez `e.Row.Cells[index].Text = value`, où l’indexation `Cells` commence à 0. Le code suivant calcule le prix moyen (le prix total divisé par le nombre de produits) et l’affiche, ainsi que le nombre total d’unités en stock et en unités sur la commande dans les cellules de pied de page appropriées du contrôle GridView.

[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample8.cs)]

La figure 13 illustre le rapport une fois que ce code a été ajouté. Notez la manière dont le `ToString("c")` entraîne une mise en forme des informations de synthèse des prix moyens comme une devise.

[![la ligne de pied de page du GridView a maintenant une couleur d’arrière-plan rougeâtre](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image38.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image37.png)

**Figure 13**: la ligne de pied de page du GridView a maintenant une couleur d’arrière-plan rougeâtre ([cliquez pour afficher l’image en taille réelle](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image39.png))

## <a name="summary"></a>Récapitulatif

L’affichage des données de synthèse est une exigence courante pour les rapports et le contrôle GridView facilite l’inclusion de telles informations dans sa ligne de pied de page. La ligne de pied de page s’affiche lorsque la propriété `ShowFooter` du GridView est définie sur `true` et que le texte de ses cellules peut être défini par programme via le gestionnaire d’événements `RowDataBound`. Le calcul des données de synthèse peut être effectué en interrogeant à nouveau la base de données ou en utilisant le code de la classe code-behind de la page ASP.NET pour calculer par programmation les données de synthèse.

Ce didacticiel conclut notre examen de la mise en forme personnalisée avec les contrôles GridView, DetailsView et FormView. Notre prochain didacticiel lance l’exploration de l’insertion, de la mise à jour et de la suppression de données à l’aide de ces mêmes contrôles.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Précédent](using-the-formview-s-templates-cs.md)
> [Suivant](custom-formatting-based-upon-data-vb.md)
