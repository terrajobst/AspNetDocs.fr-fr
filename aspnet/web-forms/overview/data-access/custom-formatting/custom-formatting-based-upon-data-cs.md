---
uid: web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-cs
title: Mise en forme personnalisée basée sur lesC#données () | Microsoft Docs
author: rick-anderson
description: L’ajustement du format de GridView, DetailsView ou FormView basé sur les données qui lui sont liées peut être accompli de plusieurs façons. Dans ce didacticiel, nous allons l...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 871a4574-f89c-4214-b786-79253ed3653b
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-cs
msc.type: authoredcontent
ms.openlocfilehash: d8f3fa337eda0ceed041475ecb52f8b378b9fbba
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78549314"
---
# <a name="custom-formatting-based-upon-data-c"></a>Mise en forme personnalisée basée sur des données (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_11_CS.exe) ou [Télécharger le PDF](custom-formatting-based-upon-data-cs/_static/datatutorial11cs1.pdf)

> L’ajustement du format de GridView, DetailsView ou FormView basé sur les données qui lui sont liées peut être accompli de plusieurs façons. Dans ce didacticiel, nous allons examiner comment effectuer une mise en forme liée aux données via l’utilisation des gestionnaires d’événements DataBound et RowDataBound.

## <a name="introduction"></a>Introduction

L’apparence des contrôles GridView, DetailsView et FormView peut être personnalisée par le biais d’une multitude de propriétés liées au style. Les propriétés telles que `CssClass`, `Font`, `BorderWidth`, `BorderStyle`, `BorderColor`, `Width`et `Height`, entre autres, déterminent l’apparence générale du contrôle rendu. Les propriétés, y compris `HeaderStyle`, `RowStyle`, `AlternatingRowStyle`et d’autres, permettent d’appliquer ces mêmes paramètres de style à des sections particulières. De même, ces paramètres de style peuvent être appliqués au niveau du champ.

Toutefois, dans de nombreux scénarios, les spécifications de mise en forme dépendent de la valeur des données affichées. Par exemple, pour attirer l’attention sur les produits en rupture de stock, un rapport répertoriant les informations sur les produits peut affecter la couleur jaune à la couleur d’arrière-plan pour les produits dont les champs `UnitsInStock` et `UnitsOnOrder` sont égaux à 0. Pour mettre en évidence les produits les plus chers, nous pouvons souhaiter afficher les prix de ces produits qui coûtent plus de $75,00 en caractères gras.

L’ajustement du format de GridView, DetailsView ou FormView basé sur les données qui lui sont liées peut être accompli de plusieurs façons. Dans ce didacticiel, nous allons examiner comment effectuer une mise en forme liée aux données via l’utilisation des gestionnaires d’événements `DataBound` et `RowDataBound`. Dans le didacticiel suivant, nous allons explorer une autre approche.

## <a name="using-the-detailsview-controlsdataboundevent-handler"></a>Utilisation du gestionnaire d’événements`DataBound`du contrôle DetailsView

Lorsque les données sont liées à un DetailsView, soit à partir d’un contrôle de source de données, soit par le biais de l’assignation par programmation de données à la propriété `DataSource` du contrôle et de l’appel de sa méthode `DataBind()`, la séquence d’étapes suivante se produit :

1. L’événement `DataBinding` du contrôle Web des données est déclenché.
2. Les données sont liées au contrôle Web de données.
3. L’événement `DataBound` du contrôle Web des données est déclenché.

La logique personnalisée peut être injectée immédiatement après les étapes 1 et 3 à l’aide d’un gestionnaire d’événements. En créant un gestionnaire d’événements pour l’événement `DataBound`, nous pouvons déterminer par programmation les données qui ont été liées au contrôle Web de données et ajuster la mise en forme en fonction des besoins. Pour illustrer cela, créons un DetailsView qui répertorie des informations générales sur un produit, mais affiche la valeur `UnitPrice` dans une ***police en gras et en italique*** si elle dépasse $75,00.

## <a name="step-1-displaying-the-product-information-in-a-detailsview"></a>Étape 1 : affichage des informations sur le produit dans un DetailsView

Ouvrez la page `CustomColors.aspx` dans le dossier `CustomFormatting`, faites glisser un contrôle DetailsView de la boîte à outils vers le concepteur, définissez sa valeur de propriété `ID` sur `ExpensiveProductsPriceInBoldItalic`et liez-le à un nouveau contrôle ObjectDataSource qui appelle la méthode `ProductsBLL` de la classe `GetProducts()`. Les étapes détaillées pour y parvenir sont omises ici par souci de concision puisque nous les avons examinés en détail dans les didacticiels précédents.

Une fois que vous avez lié ObjectDataSource au contrôle DetailsView, prenez un moment pour modifier la liste de champs. J’ai choisi de supprimer les `ProductID`, `SupplierID`, `CategoryID`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`et `Discontinued` BoundFields, puis renommé et reformaté les BoundFields restants. J’ai également supprimé les paramètres `Width` et `Height`. Étant donné que le contrôle DetailsView n’affiche qu’un seul enregistrement, nous devons activer la pagination afin de permettre à l’utilisateur final d’afficher tous les produits. Pour ce faire, activez la case à cocher Activer la pagination dans la balise active du contrôle DetailsView.

[![activez la case à cocher Activer la pagination dans la balise active du contrôle DetailsView](custom-formatting-based-upon-data-cs/_static/image2.png)](custom-formatting-based-upon-data-cs/_static/image1.png)

**Figure 1**: cochez la case Activer la pagination dans la balise active du contrôle DetailsView ([cliquez pour afficher l’image en taille réelle](custom-formatting-based-upon-data-cs/_static/image3.png))

Une fois ces modifications effectuées, le balisage DetailsView est le suivant :

[!code-aspx[Main](custom-formatting-based-upon-data-cs/samples/sample1.aspx)]

Prenez un moment pour tester cette page dans votre navigateur.

[![le contrôle DetailsView affiche un produit à la fois](custom-formatting-based-upon-data-cs/_static/image5.png)](custom-formatting-based-upon-data-cs/_static/image4.png)

**Figure 2**: le contrôle DetailsView affiche un produit à la fois ([cliquez pour afficher l’image en taille réelle](custom-formatting-based-upon-data-cs/_static/image6.png))

## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Étape 2 : détermination par programmation de la valeur des données dans le gestionnaire d’événements DataBound

Pour afficher le prix dans une police en gras et en italique pour les produits dont la valeur `UnitPrice` dépasse $75,00, nous devons tout d’abord être en mesure de déterminer par programmation la valeur `UnitPrice`. Pour le contrôle DetailsView, cela peut être accompli dans le gestionnaire d’événements `DataBound`. Pour créer le gestionnaire d’événements, cliquez sur le contrôle DetailsView dans le concepteur, puis accédez à l’Fenêtre Propriétés. Appuyez sur F4 pour le faire apparaître, s’il n’est pas visible, ou accédez au menu Affichage et sélectionnez l’option de menu fenêtre Propriétés. Dans le Fenêtre Propriétés, cliquez sur l’icône représentant un éclair pour répertorier les événements du contrôle DetailsView. Ensuite, double-cliquez sur l’événement `DataBound` ou tapez le nom du gestionnaire d’événements que vous souhaitez créer.

![Créer un gestionnaire d’événements pour l’événement DataBound](custom-formatting-based-upon-data-cs/_static/image7.png)

**Figure 3**: créer un gestionnaire d’événements pour l’événement `DataBound`

Cela crée automatiquement le gestionnaire d’événements et vous permet d’atteindre la partie de code où elle a été ajoutée. À ce stade, vous verrez :

[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample2.cs)]

Les données liées au contrôle DetailsView sont accessibles via la propriété `DataItem`. Rappelez-vous que nous liantons nos contrôles à un DataTable fortement typé, qui est composé d’une collection d’instances DataRow fortement typées. Lorsque le DataTable est lié au DetailsView, le premier DataRow du DataTable est assigné à la propriété `DataItem` du contrôle DetailsView. Plus précisément, la propriété `DataItem` est assignée à un objet `DataRowView`. Nous pouvons utiliser la propriété `Row` de `DataRowView`pour obtenir l’accès à l’objet DataRow sous-jacent, qui est en fait une instance de `ProductsRow`. Une fois cette `ProductsRow` instance, nous pouvons prendre notre décision en inspectant simplement les valeurs de propriété de l’objet.

Le code suivant montre comment déterminer si la valeur `UnitPrice` liée au contrôle DetailsView est supérieure à $75,00 :

[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample3.cs)]

> [!NOTE]
> Étant donné que `UnitPrice` pouvez avoir une valeur `NULL` dans la base de données, nous vérifions tout d’abord que nous ne traitons pas une valeur de `NULL` avant d’accéder à la propriété `UnitPrice` de `ProductsRow`. Cette vérification est importante, car si nous tentons d’accéder à la propriété `UnitPrice` lorsqu’elle a une valeur de `NULL`, l’objet `ProductsRow` lèvera une [exception StrongTypingException](https://msdn.microsoft.com/library/system.data.strongtypingexception.aspx).

## <a name="step-3-formatting-the-unitprice-value-in-the-detailsview"></a>Étape 3 : mise en forme de la valeur UnitPrice dans le DetailsView

À ce stade, nous pouvons déterminer si la valeur `UnitPrice` liée à DetailsView a une valeur qui dépasse $75,00, mais nous avons encore vu comment ajuster par programmation la mise en forme du contrôle DetailsView en conséquence. Pour modifier la mise en forme d’une ligne entière dans le contrôle DetailsView, accédez par programmation à la ligne à l’aide de `DetailsViewID.Rows[index]`; pour modifier une cellule particulière, Access utilise `DetailsViewID.Rows[index].Cells[index]`. Une fois que nous avons une référence à la ligne ou à la cellule, nous pouvons ajuster son apparence en définissant ses propriétés associées au style.

Pour accéder à une ligne par programmation, vous devez connaître l’index de la ligne, qui commence à 0. La ligne `UnitPrice` est la cinquième ligne du DetailsView, en lui donnant un index de 4 et en la rendant accessible par programmation à l’aide de `ExpensiveProductsPriceInBoldItalic.Rows[4]`. À ce stade, nous aurions pu afficher le contenu de la totalité de la ligne dans une police en gras et en italique en utilisant le code suivant :

[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample4.cs)]

Toutefois, cela rendra à la *fois* l’étiquette (Price) et la valeur Bold et Italic. Si vous souhaitez que la valeur soit uniquement gras et italique, nous devons appliquer cette mise en forme à la deuxième cellule de la ligne, ce qui peut être accompli à l’aide des éléments suivants :

[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample5.cs)]

Puisque nos didacticiels, jusqu’à présent, ont utilisé des feuilles de style pour maintenir une séparation nette entre le balisage rendu et les informations liées au style, plutôt que de définir les propriétés de style spécifiques comme indiqué ci-dessus, utilisons plutôt une classe CSS. Ouvrez la feuille de style `Styles.css` et ajoutez une nouvelle classe CSS nommée `ExpensivePriceEmphasis` avec la définition suivante :

[!code-css[Main](custom-formatting-based-upon-data-cs/samples/sample6.css)]

Ensuite, dans le gestionnaire d’événements `DataBound`, affectez à la propriété `CssClass` de la cellule la valeur `ExpensivePriceEmphasis`. Le code suivant montre le gestionnaire d’événements `DataBound` dans son intégralité :

[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample7.cs)]

Lorsque vous affichez Chai, ce qui coûte moins de $75,00, le prix est affiché dans une police normale (voir figure 4). Toutefois, lors de l’affichage de Mishi Kobe Niku, qui a un prix de $97,00, le prix est affiché en gras et en italique (voir figure 5).

[les prix ![inférieurs à $75,00 sont affichés dans une police normale](custom-formatting-based-upon-data-cs/_static/image9.png)](custom-formatting-based-upon-data-cs/_static/image8.png)

**Figure 4**: les prix inférieurs à $75,00 sont affichés en police normale ([cliquez pour afficher l’image en taille réelle](custom-formatting-based-upon-data-cs/_static/image10.png))

[les prix des produits ![onéreux sont affichés en gras et en italique](custom-formatting-based-upon-data-cs/_static/image12.png)](custom-formatting-based-upon-data-cs/_static/image11.png)

**Figure 5**: les tarifs des produits chers s’affichent en caractères gras et en italique ([cliquez pour afficher l’image en taille réelle](custom-formatting-based-upon-data-cs/_static/image13.png))

## <a name="using-the-formview-controlsdataboundevent-handler"></a>Utilisation du gestionnaire d’événements`DataBound`du contrôle FormView

Les étapes permettant de déterminer les données sous-jacentes liées à un contrôle FormView sont identiques à celles d’un gestionnaire d’événements de création de `DataBound` DetailsView, castez la propriété `DataItem` au type d’objet approprié lié au contrôle et Déterminez comment procéder. Les fonctions FormView et DetailsView diffèrent, toutefois, de la façon dont l’apparence de leur interface utilisateur est mise à jour.

Le FormView ne contient pas de BoundFields et, par conséquent, ne dispose pas de la collection `Rows`. Au lieu de cela, un FormView est composé de modèles, qui peuvent contenir une combinaison de syntaxe statique HTML, de contrôles Web et de syntaxe DataBinding. L’ajustement du style d’un contrôle FormView implique généralement l’ajustement du style d’un ou plusieurs contrôles Web dans les modèles du FormView.

Pour illustrer cela, nous allons utiliser un FormView pour répertorier les produits comme dans l’exemple précédent, mais cette fois-ci, nous affichons uniquement le nom de produit et les unités en stock avec les unités en stock affichées dans une police rouge si elles sont inférieures ou égales à 10.

## <a name="step-4-displaying-the-product-information-in-a-formview"></a>Étape 4 : affichage des informations sur le produit dans un FormView

Ajoutez un FormView à la page de `CustomColors.aspx` sous le contrôle DetailsView et affectez à sa propriété `ID` la valeur `LowStockedProductsInRed`. Liez le contrôle FormView au contrôle ObjectDataSource créé à l’étape précédente. Cela permet de créer un `ItemTemplate`, `EditItemTemplate`et `InsertItemTemplate` pour le FormView. Supprimez le `EditItemTemplate` et `InsertItemTemplate` et simplifiez le `ItemTemplate` pour inclure uniquement les valeurs `ProductName` et `UnitsInStock`, chacune dans ses propres contrôles Label de nom approprié. Comme avec le contrôle DetailsView de l’exemple précédent, activez également la case à cocher Activer la pagination dans la balise active du FormView.

Une fois ces modifications apportées, le balisage de votre FormView doit ressembler à ce qui suit :

[!code-aspx[Main](custom-formatting-based-upon-data-cs/samples/sample8.aspx)]

Notez que le `ItemTemplate` contient les éléments suivants :

- **HTML statique** texte « Product : » et « units in stock : », ainsi que les éléments `<br />` et `<b>`.
- **Web contrôle** les deux contrôles Label, `ProductNameLabel` et `UnitsInStockLabel`.
- **Syntaxe DataBinding** la syntaxe `<%# Bind("ProductName") %>` et `<%# Bind("UnitsInStock") %>`, qui assigne les valeurs de ces champs aux propriétés de `Text` des contrôles Label.

## <a name="step-5-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Étape 5 : détermination par programmation de la valeur des données dans le gestionnaire d’événements DataBound

Une fois le balisage du FormView terminé, l’étape suivante consiste à déterminer par programmation si la valeur `UnitsInStock` est inférieure ou égale à 10. Cette opération s’effectue exactement de la même manière avec le FormView que dans le contrôle DetailsView. Commencez par créer un gestionnaire d’événements pour l’événement `DataBound` du FormView.

![Créer le gestionnaire d’événements DataBound](custom-formatting-based-upon-data-cs/_static/image14.png)

**Figure 6**: créer le gestionnaire d’événements `DataBound`

Dans le gestionnaire d’événements, effectuez un cast de la propriété `DataItem` du FormView en une instance `ProductsRow` et déterminez si la valeur `UnitsInPrice` est telle que nous devons l’afficher dans une police rouge.

[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample9.cs)]

## <a name="step-6-formatting-the-unitsinstocklabel-label-control-in-the-formviews-itemtemplate"></a>Étape 6 : mise en forme du contrôle Label UnitsInStockLabel dans le ItemTemplate du FormView

La dernière étape consiste à mettre en forme la valeur `UnitsInStock` affichée dans une police rouge si la valeur est inférieure ou égale à 10. Pour ce faire, nous devons accéder par programmation au contrôle `UnitsInStockLabel` dans le `ItemTemplate` et définir ses propriétés de style afin que son texte s’affiche en rouge. Pour accéder à un contrôle Web dans un modèle, utilisez la méthode `FindControl("controlID")` comme suit :

[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample10.cs)]

Pour notre exemple, nous souhaitons accéder à un contrôle Label dont la valeur `ID` est `UnitsInStockLabel`, donc nous utiliserions :

[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample11.cs)]

Une fois que nous avons une référence par programmation au contrôle Web, nous pouvons modifier ses propriétés liées au style en fonction des besoins. Comme dans l’exemple précédent, j’ai créé une classe CSS dans `Styles.css` nommée `LowUnitsInStockEmphasis`. Pour appliquer ce style au contrôle Web Label, définissez sa propriété `CssClass` en conséquence.

[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample12.cs)]

> [!NOTE]
> La syntaxe de mise en forme d’un modèle qui accède par programmation au contrôle Web à l’aide de `FindControl("controlID")` puis la définition de ses propriétés liées au style peut également être utilisée lors de l’utilisation de [TemplateFields](https://msdn.microsoft.com/library/system.web.ui.webcontrols.templatefield(VS.80).aspx) dans les contrôles DetailsView ou GridView. Nous allons examiner TemplateFields dans le prochain didacticiel.

Les figures 7 affichent le FormView lorsque vous affichez un produit dont la valeur `UnitsInStock` est supérieure à 10, alors que le produit de la figure 8 a une valeur inférieure à 10.

[![pour les produits avec des unités suffisamment volumineuses en stock, aucune mise en forme personnalisée n’est appliquée.](custom-formatting-based-upon-data-cs/_static/image16.png)](custom-formatting-based-upon-data-cs/_static/image15.png)

**Figure 7**: pour les produits avec des unités suffisamment volumineuses en stock, aucune mise en forme personnalisée n’est appliquée ([cliquez pour afficher l’image en taille réelle](custom-formatting-based-upon-data-cs/_static/image17.png))

[![les unités en stock sont indiquées en rouge pour les produits dont la valeur est inférieure ou égale à 10](custom-formatting-based-upon-data-cs/_static/image19.png)](custom-formatting-based-upon-data-cs/_static/image18.png)

**Figure 8**: les unités en stock sont indiquées en rouge pour les produits dont la valeur est inférieure ou égale à 10 ([cliquez pour afficher l’image en plein écran](custom-formatting-based-upon-data-cs/_static/image20.png))

## <a name="formatting-with-the-gridviewsrowdataboundevent"></a>Mise en forme avec l’événement`RowDataBound`du GridView

Nous avons examiné précédemment la séquence d’étapes avec laquelle les contrôles DetailsView et FormView contrôlent la progression pendant la liaison de liaison. Examinons ces étapes une nouvelle fois en tant qu’actualisateur.

1. L’événement `DataBinding` du contrôle Web des données est déclenché.
2. Les données sont liées au contrôle Web de données.
3. L’événement `DataBound` du contrôle Web des données est déclenché.

Ces trois étapes simples suffisent pour DetailsView et FormView, car elles n’affichent qu’un seul enregistrement. Pour le GridView, qui affiche *tous les* enregistrements liés à celui-ci (pas seulement le premier), l’étape 2 est un peu plus complexe.

À l’étape 2, le GridView énumère la source de données et, pour chaque enregistrement, crée une instance de `GridViewRow` et y lie l’enregistrement actif. Pour chaque `GridViewRow` ajoutée au GridView, deux événements sont déclenchés :

- **`RowCreated`** se déclenche après la création du `GridViewRow`
- **`RowDataBound`** se déclenche après la liaison de l’enregistrement actif au `GridViewRow`.

Pour le GridView, la liaison de données est décrite de façon plus précise par la séquence d’étapes suivante :

1. L’événement `DataBinding` du GridView est déclenché.
2. Les données sont liées au GridView.   
  
   Pour chaque enregistrement dans la source de données 

    1. Créer un objet `GridViewRow`
    2. Déclencher l’événement `RowCreated`
    3. Lier l’enregistrement au `GridViewRow`
    4. Déclencher l’événement `RowDataBound`
    5. Ajouter le `GridViewRow` à la collection de `Rows`
3. L’événement `DataBound` du GridView est déclenché.

Pour personnaliser le format des enregistrements individuels de GridView, nous devons créer un gestionnaire d’événements pour l’événement `RowDataBound`. Pour illustrer cela, nous allons ajouter un GridView à la page `CustomColors.aspx` qui répertorie le nom, la catégorie et le prix de chaque produit, en mettant en surbrillance les produits dont le prix est inférieur à $10,00 avec une couleur d’arrière-plan jaune.

## <a name="step-7-displaying-product-information-in-a-gridview"></a>Étape 7 : affichage des informations sur le produit dans un GridView

Ajoutez un contrôle GridView sous le contrôle FormView de l’exemple précédent et affectez à sa propriété `ID` la valeur `HighlightCheapProducts`. Étant donné que nous avons déjà un ObjectDataSource qui retourne tous les produits de la page, liez le contrôle GridView à ce. Enfin, modifiez le BoundFields du contrôle GridView pour inclure uniquement les noms, les catégories et les prix des produits. Une fois ces modifications apportées, le balisage de GridView doit ressembler à ceci :

[!code-aspx[Main](custom-formatting-based-upon-data-cs/samples/sample13.aspx)]

La figure 9 illustre notre progression jusqu’à ce point lorsque vous l’affichez dans un navigateur.

[![le contrôle GridView répertorie le nom, la catégorie et le prix de chaque produit](custom-formatting-based-upon-data-cs/_static/image22.png)](custom-formatting-based-upon-data-cs/_static/image21.png)

**Figure 9**: le contrôle GridView répertorie le nom, la catégorie et le prix de chaque produit ([cliquez pour afficher l’image en taille réelle](custom-formatting-based-upon-data-cs/_static/image23.png))

## <a name="step-8-programmatically-determining-the-value-of-the-data-in-the-rowdatabound-event-handler"></a>Étape 8 : détermination par programmation de la valeur des données dans le gestionnaire d’événements RowDataBound

Lorsque le `ProductsDataTable` est lié au GridView, ses instances de `ProductsRow` sont énumérées et pour chaque `ProductsRow` une `GridViewRow` est créée. La propriété `DataItem` de `GridViewRow`est assignée à la `ProductRow`particulière, après laquelle le gestionnaire d’événements `RowDataBound` de GridView est déclenché. Pour déterminer la valeur de `UnitPrice` pour chaque produit lié au GridView, nous devons ensuite créer un gestionnaire d’événements pour l’événement de `RowDataBound` du GridView. Dans ce gestionnaire d’événements, nous pouvons inspecter la valeur `UnitPrice` pour le `GridViewRow` actuel et prendre une décision de mise en forme pour cette ligne.

Ce gestionnaire d’événements peut être créé à l’aide de la même série d’étapes que pour FormView et DetailsView.

![Créer un gestionnaire d’événements pour l’événement RowDataBound de GridView](custom-formatting-based-upon-data-cs/_static/image24.png)

**Figure 10**: créer un gestionnaire d’événements pour l’événement `RowDataBound` du GridView

La création du gestionnaire d’événements de cette manière entraîne l’ajout automatique du code suivant à la partie de code de la page ASP.NET :

[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample14.cs)]

Lorsque l’événement `RowDataBound` se déclenche, le gestionnaire d’événements est passé comme deuxième paramètre un objet de type `GridViewRowEventArgs`, qui a une propriété nommée `Row`. Cette propriété retourne une référence à la `GridViewRow` qui était uniquement liée aux données. Pour accéder à l’instance `ProductsRow` liée au `GridViewRow` nous utilisons la propriété `DataItem` comme suit :

[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample15.cs)]

Lorsque vous utilisez le gestionnaire d’événements `RowDataBound`, il est important de garder à l’esprit que le contrôle GridView est composé de différents types de lignes et que cet événement est déclenché pour *tous les* types de lignes. Le type d’un `GridViewRow`peut être déterminé par sa propriété `RowType` et peut avoir l’une des valeurs possibles :

- `DataRow` une ligne liée à un enregistrement à partir du `DataSource` du GridView
- `EmptyDataRow` la ligne affichée si le `DataSource` du GridView est vide
- `Footer` la ligne de pied de page ; indiqué si la propriété `ShowFooter` de GridView a la valeur `true`
- `Header` la ligne d’en-tête ; indique si la propriété ShowHeader du contrôle GridView est définie sur `true` (valeur par défaut)
- `Pager` pour GridView qui implémente la pagination, la ligne qui affiche l’interface de pagination
- `Separator` pas utilisé pour le GridView, mais utilisé par les propriétés `RowType` pour les contrôles DataList et Repeater, deux contrôles Web de données que nous aborderons dans les prochains didacticiels

Étant donné que les lignes `EmptyDataRow`, `Header`, `Footer`et `Pager` ne sont pas associées à un enregistrement `DataSource`, elles auront toujours une valeur `null` pour leur propriété `DataItem`. Pour cette raison, avant d’essayer de travailler avec la propriété `DataItem` de l' `GridViewRow`actuel, nous devons tout d’abord nous assurer que nous avons affaire à un `DataRow`. Pour ce faire, vous pouvez vérifier la propriété `RowType` de l' `GridViewRow`comme suit :

[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample16.cs)]

## <a name="step-9-highlighting-the-row-yellow-when-the-unitprice-value-is-less-than-1000"></a>Étape 9 : mise en surbrillance de la ligne jaune lorsque la valeur UnitPrice est inférieure à $10,00

La dernière étape consiste à mettre en surbrillance par programmation le `GridViewRow` entier si la valeur `UnitPrice` de cette ligne est inférieure à $10,00. La syntaxe permettant d’accéder aux lignes ou aux cellules d’un contrôle GridView est identique à celle du `GridViewID.Rows[index]` DetailsView pour accéder à la ligne entière, `GridViewID.Rows[index].Cells[index]` pour accéder à une cellule particulière. Toutefois, lorsque le gestionnaire d’événements `RowDataBound` déclenche l' `GridViewRow` lié aux données, il n’a pas encore été ajouté à la collection de `Rows` du contrôle GridView. Par conséquent, vous ne pouvez pas accéder à l’instance de `GridViewRow` actuelle à partir du gestionnaire d’événements `RowDataBound` à l’aide de la collection Rows.

Au lieu de `GridViewID.Rows[index]`, nous pouvons référencer l’instance de `GridViewRow` actuelle dans le gestionnaire d’événements `RowDataBound` à l’aide de `e.Row`. Autrement dit, pour mettre en surbrillance l’instance de `GridViewRow` actuelle à partir du gestionnaire d’événements `RowDataBound`, nous utilisons :

[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample17.cs)]

Plutôt que de définir directement la propriété de `BackColor` du `GridViewRow`, nous allons utiliser des classes CSS. J’ai créé une classe CSS nommée `AffordablePriceEmphasis` qui définit la couleur d’arrière-plan sur jaune. Le gestionnaire d’événements `RowDataBound` terminés est le suivant :

[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample18.cs)]

[![les produits les plus chers sont mis en surbrillance en jaune](custom-formatting-based-upon-data-cs/_static/image26.png)](custom-formatting-based-upon-data-cs/_static/image25.png)

**Figure 11**: les produits les plus chers sont mis en surbrillance en jaune ([cliquez pour afficher l’image en taille réelle](custom-formatting-based-upon-data-cs/_static/image27.png))

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, nous avons vu comment mettre en forme les contrôles GridView, DetailsView et FormView en fonction des données liées au contrôle. Pour ce faire, nous avons créé un gestionnaire d’événements pour les événements `DataBound` ou `RowDataBound`, où les données sous-jacentes ont été examinées avec une modification de la mise en forme, si nécessaire. Pour accéder aux données liées à un contrôle DetailsView ou FormView, nous utilisons la propriété `DataItem` dans le gestionnaire d’événements `DataBound` ; pour un GridView, la propriété `DataItem` de chaque instance `GridViewRow` contient les données liées à cette ligne, qui sont disponibles dans le gestionnaire d’événements `RowDataBound`.

La syntaxe pour ajuster par programmation la mise en forme du contrôle Web des données dépend du contrôle Web et de la façon dont les données à mettre en forme sont affichées. Pour les contrôles DetailsView et GridView, les lignes et les cellules sont accessibles par un index ordinal. Pour le FormView, qui utilise des modèles, la méthode `FindControl("controlID")` est couramment utilisée pour rechercher un contrôle Web dans le modèle.

Dans le didacticiel suivant, nous allons examiner comment utiliser des modèles avec les contrôles GridView et DetailsView. En outre, nous verrons une autre technique pour personnaliser la mise en forme en fonction des données sous-jacentes.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel étaient E.R. Gilmore, Denis Patterson et Dan Jagers. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](using-templatefields-in-the-gridview-control-cs.md)
