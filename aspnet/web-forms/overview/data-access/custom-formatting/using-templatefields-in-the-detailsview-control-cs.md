---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-cs
title: Utilisation de TemplateFields dans le contrôle DetailsViewC#() | Microsoft Docs
author: rick-anderson
description: Les mêmes fonctionnalités TemplateFields disponibles avec le contrôle GridView sont également disponibles avec le contrôle DetailsView. Dans ce didacticiel, nous allons afficher un produit...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 83efb21f-b231-446e-9356-f4c6cbcc6713
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 55c667800a9fb19d200bcf4b68e2c59ca4ef5d0e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74622235"
---
# <a name="using-templatefields-in-the-detailsview-control-c"></a>Utilisation de TemplateFields dans le contrôle DetailsView (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_13_CS.exe) ou [Télécharger le PDF](using-templatefields-in-the-detailsview-control-cs/_static/datatutorial13cs1.pdf)

> Les mêmes fonctionnalités TemplateFields disponibles avec le contrôle GridView sont également disponibles avec le contrôle DetailsView. Dans ce didacticiel, nous allons afficher un produit à la fois à l’aide d’un contrôle DetailsView contenant TemplateFields.

## <a name="introduction"></a>Introduction

Le TemplateField offre une plus grande souplesse dans le rendu des données que les contrôles BoundField, CheckBoxField, HyperLinkField et autres contrôles de champ de données. Dans le [didacticiel précédent](using-templatefields-in-the-gridview-control-cs.md) , nous avons examiné l’utilisation du TemplateField dans un GridView pour :

- Afficher plusieurs valeurs de champ de données dans une colonne. Plus précisément, les champs `FirstName` et `LastName` ont été combinés en une seule colonne GridView.
- Utilisez un autre contrôle Web pour exprimer une valeur de champ de données. Nous avons vu comment afficher la valeur `HiredDate` à l’aide d’un contrôle calendrier.
- Affichez les informations d’État en fonction des données sous-jacentes. Alors que la table `Employees` ne contient pas de colonne qui retourne le nombre de jours pendant lesquels un employé a travaillé sur le travail, nous avons pu afficher ces informations dans l’exemple GridView dans le didacticiel précédent avec l’utilisation d’un TemplateField et d’une méthode de mise en forme.

Les mêmes fonctionnalités TemplateFields disponibles avec le contrôle GridView sont également disponibles avec le contrôle DetailsView. Dans ce didacticiel, nous allons afficher un produit à la fois à l’aide d’un DetailsView qui contient deux TemplateFields. Le premier TemplateField combinera les champs de données `UnitPrice`, `UnitsInStock`et `UnitsOnOrder` en une seule ligne DetailsView. Le deuxième TemplateField affiche la valeur du champ `Discontinued`, mais utilise une méthode de mise en forme pour afficher « Oui » si `Discontinued` est `true`, et « non » dans le cas contraire.

[![deux TemplateFields sont utilisés pour personnaliser l’affichage](using-templatefields-in-the-detailsview-control-cs/_static/image2.png)](using-templatefields-in-the-detailsview-control-cs/_static/image1.png)

**Figure 1**: deux TemplateFields sont utilisés pour personnaliser l’affichage ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-detailsview-control-cs/_static/image3.png))

Commençons !

## <a name="step-1-binding-the-data-to-the-detailsview"></a>Étape 1 : liaison des données au contrôle DetailsView

Comme indiqué dans le didacticiel précédent, lors de l’utilisation de TemplateFields, il est souvent plus facile de commencer par créer le contrôle DetailsView contenant simplement BoundFields, puis d’ajouter de nouveaux TemplateFields ou de convertir le BoundFields existant en TemplateFields en fonction des besoins. . Par conséquent, commencez ce didacticiel en ajoutant un DetailsView à la page par le biais du concepteur et en le liant à un ObjectDataSource qui retourne la liste des produits. Ces étapes vont créer un DetailsView avec BoundFields pour chacun des champs de valeur non booléenne du produit et un CheckBoxField pour le champ de valeur booléenne (abandonné).

Ouvrez la page `DetailsViewTemplateField.aspx` et faites glisser un DetailsView de la boîte à outils vers le concepteur. À partir de la balise active du contrôle DetailsView, choisissez d’ajouter un nouveau contrôle ObjectDataSource qui appelle la méthode `GetProducts()` de la classe `ProductsBLL`.

[![ajouter un nouveau contrôle ObjectDataSource qui appelle la méthode GetProducts ()](using-templatefields-in-the-detailsview-control-cs/_static/image5.png)](using-templatefields-in-the-detailsview-control-cs/_static/image4.png)

**Figure 2**: ajouter un nouveau contrôle ObjectDataSource qui appelle la méthode `GetProducts()` ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-detailsview-control-cs/_static/image6.png))

Pour ce rapport, supprimez les `ProductID`, `SupplierID`, `CategoryID`et `ReorderLevel` BoundFields. Ensuite, réorganisez le BoundFields afin que les `CategoryName` et `SupplierName` BoundFields apparaissent immédiatement après le `ProductName` BoundField. N’hésitez pas à ajuster les propriétés de `HeaderText` et les propriétés de mise en forme pour le BoundFields comme vous le souhaitez. Comme avec le contrôle GridView, ces modifications au niveau de BoundField peuvent être effectuées via la boîte de dialogue champs (accessible en cliquant sur le lien modifier les champs dans la balise active du contrôle DetailsView) ou à l’aide de la syntaxe déclarative. Enfin, effacez les valeurs de propriété `Height` et `Width` du contrôle DetailsView pour permettre au contrôle DetailsView de se développer en fonction des données affichées et activez la case à cocher Activer la pagination dans la balise active.

Après avoir apporté ces modifications, le balisage déclaratif de votre contrôle DetailsView doit ressembler à ce qui suit :

[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample1.aspx)]

Prenez un moment pour afficher la page via un navigateur. À ce stade, vous devriez voir un seul produit listé (Chai) avec des lignes qui indiquent le nom du produit, la catégorie, le fournisseur, le prix, les unités en stock, les unités en commande et son état abandonné.

[![les détails du produit sont affichés à l’aide d’une série de BoundFields](using-templatefields-in-the-detailsview-control-cs/_static/image8.png)](using-templatefields-in-the-detailsview-control-cs/_static/image7.png)

**Figure 3**: les détails du produit sont affichés à l’aide d’une série de BoundFields ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-detailsview-control-cs/_static/image9.png))

## <a name="step-2-combining-the-price-units-in-stock-and-units-on-order-into-one-row"></a>Étape 2 : combinaison du prix, des unités en stock et des unités sur la commande en une seule ligne

Le contrôle DetailsView a une ligne pour les champs `UnitPrice`, `UnitsInStock`et `UnitsOnOrder`. Nous pouvons combiner ces champs de données en une seule ligne avec un TemplateField soit en ajoutant un nouveau TemplateField, soit en convertissant l’un des `UnitPrice`, `UnitsInStock`et `UnitsOnOrder` BoundFields existants en TemplateField. Bien que je préfère personnellement convertir les BoundFields existants, nous allons en ajouter un nouveau TemplateField.

Commencez par cliquer sur le lien modifier les champs dans la balise active du contrôle DetailsView pour afficher la boîte de dialogue champs. Ensuite, ajoutez un nouveau TemplateField et affectez à sa propriété `HeaderText` la valeur « Price and Inventory » et déplacez le nouveau TemplateField afin qu’il soit positionné au-dessus de l' `UnitPrice` BoundField.

[![ajouter un nouveau TemplateField au contrôle DetailsView](using-templatefields-in-the-detailsview-control-cs/_static/image11.png)](using-templatefields-in-the-detailsview-control-cs/_static/image10.png)

**Figure 4**: ajouter un nouveau TemplateField au contrôle DetailsView ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-detailsview-control-cs/_static/image12.png))

Étant donné que ce nouveau TemplateField contiendra les valeurs actuellement affichées dans le `UnitPrice`, `UnitsInStock`et `UnitsOnOrder` BoundFields, nous allons les supprimer.

La dernière tâche pour cette étape consiste à définir le balisage `ItemTemplate` pour le TemplateField Price and Inventory, qui peut être effectué par le biais de l’interface de modification de modèle du contrôle DetailsView dans le concepteur ou à l’aide de la syntaxe déclarative du contrôle. Comme avec le contrôle GridView, vous pouvez accéder à l’interface de modification de modèle du contrôle DetailsView en cliquant sur le lien modifier les modèles dans la balise active. À partir de là, vous pouvez sélectionner le modèle à modifier dans la liste déroulante, puis ajouter des contrôles Web à partir de la boîte à outils.

Pour ce didacticiel, commencez par ajouter un contrôle Label à la `ItemTemplate`du TemplateField Price and Inventory. Ensuite, cliquez sur le lien modifier les DataBindings à partir de la balise active du contrôle Web Label et liez la propriété `Text` au champ `UnitPrice`.

[![lier la propriété Text du contrôle Label au champ de données UnitPrice](using-templatefields-in-the-detailsview-control-cs/_static/image14.png)](using-templatefields-in-the-detailsview-control-cs/_static/image13.png)

**Figure 5**: lier la propriété `Text` de l’étiquette au champ de données `UnitPrice` ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-detailsview-control-cs/_static/image15.png))

## <a name="formatting-the-price-as-a-currency"></a>Mise en forme du prix en tant que devise

Avec cet ajout, le contrôle Label Web Control Price and Inventory TemplateField affiche désormais uniquement le prix du produit sélectionné. La figure 6 illustre une capture d’écran de notre progression jusqu’à présent dans un navigateur.

[![le TemplateField Price and Inventory indique le prix](using-templatefields-in-the-detailsview-control-cs/_static/image17.png)](using-templatefields-in-the-detailsview-control-cs/_static/image16.png)

**Figure 6**: le prix et le TemplateField de l’inventaire affichent le prix ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-detailsview-control-cs/_static/image18.png))

Notez que le prix du produit n’est pas mis en forme en tant que devise. Avec BoundField, la mise en forme est possible en affectant à la propriété `HtmlEncode` la valeur `false` et à la propriété `DataFormatString` la valeur `{0:formatSpecifier}`. Pour un TemplateField, toutefois, les instructions de mise en forme doivent être spécifiées dans la syntaxe de liaison de données ou à l’aide d’une méthode de mise en forme définie quelque part dans le code de l’application (par exemple, dans la classe code-behind de la page ASP.NET).

Pour spécifier la mise en forme de la syntaxe DataBinding utilisée dans le contrôle Web étiquette, revenez à la boîte de dialogue DataBindings en cliquant sur le lien modifier les DataBindings dans la balise active de l’étiquette. Vous pouvez taper les instructions de mise en forme directement dans la liste déroulante format ou sélectionner l’une des chaînes de format définies. Comme avec la propriété `DataFormatString` de BoundField, la mise en forme est spécifiée à l’aide de `{0:formatSpecifier}`.

Pour le champ `UnitPrice` utilisez la mise en forme de devise spécifiée en sélectionnant la valeur de liste déroulante appropriée ou en tapant `{0:C}` à la main.

[![formater le prix en tant que devise](using-templatefields-in-the-detailsview-control-cs/_static/image20.png)](using-templatefields-in-the-detailsview-control-cs/_static/image19.png)

**Figure 7**: mettre en forme le prix en tant que devise ([cliquez pour afficher l’image en plein écran](using-templatefields-in-the-detailsview-control-cs/_static/image21.png))

De façon déclarative, la spécification de mise en forme est indiquée en tant que deuxième paramètre dans les méthodes `Bind` ou `Eval`. Les paramètres que vous venez de faire dans le concepteur génèrent l’expression de liaison de liaison suivante dans le balisage déclaratif :

[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample2.aspx)]

## <a name="adding-the-remaining-data-fields-to-the-templatefield"></a>Ajout des champs de données restants au TemplateField

À ce stade, nous avons affiché et mis en forme le champ de données `UnitPrice` dans le TemplateField Price and Inventory, mais vous devez toujours afficher les champs `UnitsInStock` et `UnitsOnOrder`. Nous allons les afficher sur une ligne située au-dessous du prix et entre parenthèses. À partir de l’interface de modification de modèle dans le concepteur, vous pouvez ajouter un tel balisage en positionnant votre curseur dans le modèle et en tapant simplement le texte à afficher. Cette balise peut également être entrée directement dans la syntaxe déclarative.

Ajoutez le balisage statique, les contrôles d’étiquette Web et la syntaxe de liaison de données pour que le TemplateField Price et Inventory affichent les informations de prix et d’inventaire comme suit :

*Prix*  
(**En stock/sur commande :** *UnitsInStock* / *UnitsOnOrder*)

Après l’exécution de cette tâche, le balisage déclaratif de DetailsView doit ressembler à ce qui suit :

[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample3.aspx)]

Avec ces modifications, nous avons consolidé le prix et les informations d’inventaire dans une seule ligne DetailsView.

[![les informations de prix et d’inventaire sont affichées sur une seule ligne](using-templatefields-in-the-detailsview-control-cs/_static/image23.png)](using-templatefields-in-the-detailsview-control-cs/_static/image22.png)

**Figure 8**: les informations de prix et d’inventaire sont affichées sur une seule ligne ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-detailsview-control-cs/_static/image24.png))

## <a name="step-3-customizing-the-discontinued-field-information"></a>Étape 3 : personnalisation des informations sur les champs abandonnés

La colonne `Discontinued` de la table `Products` est une valeur de bit qui indique si le produit a été supprimé. Lors de la liaison d’un DetailsView (ou GridView) à un contrôle de source de données, les champs de valeur booléennes, comme `Discontinued`, sont implémentés en tant que CheckBoxFields, tandis que les champs de valeur non booléennes, comme `ProductID`, `ProductName`, etc., sont implémentés en tant que BoundFields. Le CheckBoxField est rendu sous la forme d’une case à cocher désactivée si la valeur du champ de données est true et désactivée dans le cas contraire.

Au lieu d’afficher le CheckBoxField, nous pouvons être amenés à afficher le texte indiquant si le produit n’est plus disponible. Pour ce faire, nous pouvons supprimer CheckBoxField du contrôle DetailsView, puis ajouter un BoundField dont la propriété `DataField` a été définie sur `Discontinued`. Prenez un moment pour effectuer cette opération. Après cette modification, le contrôle DetailsView affiche le texte « true » pour les produits abandonnés et « false » pour les produits toujours actifs.

[![les chaînes true et false sont utilisées pour afficher l’État abandonné](using-templatefields-in-the-detailsview-control-cs/_static/image26.png)](using-templatefields-in-the-detailsview-control-cs/_static/image25.png)

**Figure 9**: les chaînes true et false sont utilisées pour afficher l’État abandonné ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-detailsview-control-cs/_static/image27.png))

Imaginez que nous n’avions pas besoin d’utiliser les chaînes « true » ou « false », mais « YES » et « NO » à la place. Une telle personnalisation peut être effectuée à l’aide d’un TemplateField et d’une méthode de mise en forme. Une méthode de mise en forme peut prendre n’importe quel nombre de paramètres d’entrée, mais elle doit retourner le HTML (sous forme de chaîne) pour injecter dans le modèle.

Ajoutez une méthode de mise en forme à la classe code-behind de la page `DetailsViewTemplateField.aspx` nommée `DisplayDiscontinuedAsYESorNO` qui accepte une valeur booléenne comme paramètre d’entrée et retourne une chaîne. Comme indiqué dans le didacticiel précédent, cette méthode *doit* être marquée comme `protected` ou `public` afin d’être accessible à partir du modèle.

[!code-csharp[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample4.cs)]

Cette méthode vérifie le paramètre d’entrée (`discontinued`) et retourne « Oui » s’il s’agit d' `true`, « non » dans le cas contraire.

> [!NOTE]
> Dans la méthode de mise en forme examinée dans le didacticiel précédent, rappelez-vous que nous passons un champ de données qui peut contenir `NULL` s et, par conséquent, vous deviez vérifier si la valeur de la propriété de `HiredDate` de l’employé avait une valeur de `NULL` de base de données avant d’accéder à la propriété `HiredDate` de `EmployeesRow`. Cette vérification n’est pas nécessaire ici, car la `Discontinued` colonne ne peut jamais avoir de valeurs de `NULL` de base de données affectées. En outre, c’est pourquoi la méthode peut accepter un paramètre d’entrée booléen plutôt que d’accepter une `ProductsRow` instance ou un paramètre de type `object`.

Une fois cette méthode de mise en forme terminée, il ne reste plus qu’à l’appeler à partir de la `ItemTemplate`du TemplateField. Pour créer le TemplateField, supprimez le `Discontinued` BoundField et ajoutez un nouveau TemplateField ou convertissez le `Discontinued` BoundField en TemplateField. Ensuite, à partir de la vue de balisage déclaratif, modifiez le TemplateField pour qu’il contienne simplement un ItemTemplate qui appelle la méthode `DisplayDiscontinuedAsYESorNO`, en passant la valeur de la propriété `Discontinued` de l’instance `ProductRow` actuelle. Vous pouvez y accéder via la méthode `Eval`. Plus précisément, le balisage du TemplateField doit ressembler à ceci :

[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample5.aspx)]

Cela entraînera l’appel de la méthode `DisplayDiscontinuedAsYESorNO` lors du rendu du contrôle DetailsView, en passant la valeur de `Discontinued` de l’instance `ProductRow`. Étant donné que la méthode `Eval` retourne une valeur de type `object`, mais que la méthode `DisplayDiscontinuedAsYESorNO` attend un paramètre d’entrée de type `bool`, nous transmettons la valeur de retour des méthodes `Eval` en `bool`. La méthode `DisplayDiscontinuedAsYESorNO` retourne alors « oui » ou « non », en fonction de la valeur qu’elle reçoit. La valeur retournée est celle qui est affichée dans cette ligne DetailsView (voir figure 10).

[![valeur Oui ou aucune valeur n’est maintenant affichée dans la ligne supprimée](using-templatefields-in-the-detailsview-control-cs/_static/image29.png)](using-templatefields-in-the-detailsview-control-cs/_static/image28.png)

**Figure 10**: les valeurs Oui ou non s’affichent maintenant dans la ligne supprimée ([cliquez pour afficher l’image en taille réelle](using-templatefields-in-the-detailsview-control-cs/_static/image30.png))

## <a name="summary"></a>Récapitulatif

Le TemplateField dans le contrôle DetailsView offre une plus grande souplesse dans l’affichage des données que ce qui est disponible avec les autres contrôles de champ et est idéal pour les situations où :

- Plusieurs champs de données doivent être affichés dans une colonne GridView
- Les données sont mieux exprimées à l’aide d’un contrôle Web plutôt que du texte brut
- La sortie dépend des données sous-jacentes, telles que l’affichage des métadonnées ou le reformatage des données.

Bien que les TemplateFields offrent une plus grande souplesse dans le rendu des données sous-jacentes du contrôle DetailsView, la sortie du contrôle DetailsView est toujours un peu boxy, car chaque champ est rendu sous la forme d’une ligne dans un `<table>`HTML.

Le contrôle FormView offre une plus grande souplesse dans la configuration de la sortie rendue. Le FormView ne contient pas de champs, mais juste une série de modèles (`ItemTemplate`, `EditItemTemplate`, `HeaderTemplate`, etc.). Nous verrons comment utiliser le FormView pour mieux contrôler la disposition affichée dans le prochain didacticiel.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel était Dan Jagers. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](using-templatefields-in-the-gridview-control-cs.md)
> [Suivant](using-the-formview-s-templates-cs.md)
