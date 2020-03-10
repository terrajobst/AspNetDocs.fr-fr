---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs
title: Mise en forme des contrôles DataList et Repeater en fonction desC#données () | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons parcourir les exemples de la façon dont nous mettons en forme l’apparence des contrôles DataList et Repeater, soit en utilisant des fonctions de mise en forme avec...
ms.author: riande
ms.date: 09/13/2006
ms.assetid: 83e3d759-82b8-41e6-8d62-f0f4b3edec41
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 68de3450ed97fc7bd0efb27e089d9db8e3e85fb0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78611635"
---
# <a name="formatting-the-datalist-and-repeater-based-upon-data-c"></a>Mise en forme des contrôles DataList et Repeater en fonction des données (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_30_CS.exe) ou [Télécharger le PDF](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/datatutorial30cs1.pdf)

> Dans ce didacticiel, nous allons parcourir des exemples de la façon dont nous mettons en forme l’apparence des contrôles DataList et Repeater, soit en utilisant des fonctions de mise en forme dans les modèles, soit en gérant l’événement DataBound.

## <a name="introduction"></a>Introduction

Comme nous l’avons vu dans le didacticiel précédent, le contrôle DataList propose un certain nombre de propriétés liées au style qui affectent son apparence. En particulier, nous avons vu comment attribuer des classes CSS par défaut aux propriétés `HeaderStyle`, `ItemStyle`, `AlternatingItemStyle`et `SelectedItemStyle` de DataList. En plus de ces quatre propriétés, le contrôle DataList comprend un certain nombre d’autres propriétés liées au style, telles que `Font`, `ForeColor`, `BackColor`et `BorderWidth`, pour n’en nommer que quelques-uns. Le contrôle Repeater ne contient pas de propriétés liées au style. Tous les paramètres de style de ce type doivent être définis directement dans le balisage des modèles Repeater s.

Toutefois, bien souvent, la façon dont les données doivent être mises en forme dépend des données elles-mêmes. Par exemple, lorsque vous répertoriez des produits, nous pouvons souhaiter afficher les informations sur le produit dans une couleur de police gris clair si elle n’est plus disponible, ou vous pouvez mettre en surbrillance la valeur `UnitsInStock` si elle est égale à zéro. Comme nous l’avons vu dans les didacticiels précédents, GridView, DetailsView et FormView offrent deux méthodes distinctes pour mettre en forme leur apparence en fonction de leurs données :

- **L’événement `DataBound`** crée un gestionnaire d’événements pour l’événement `DataBound` approprié, qui se déclenche après la liaison des données à chaque élément (pour le GridView, il s’agissait de l’événement `RowDataBound` ; pour le contrôle DataList et Repeater, il s’agit de l’événement `ItemDataBound`). Dans ce gestionnaire d’événements, les données qui viennent d’être liées peuvent être examinées et les décisions de mise en forme prises. Nous avons examiné cette technique dans le didacticiel de [mise en forme personnalisée basé sur les données](../custom-formatting/custom-formatting-based-upon-data-cs.md) .
- **Mise en forme des fonctions dans les modèles** quand vous utilisez TemplateFields dans les contrôles DetailsView ou GridView, ou un modèle dans le contrôle FormView, nous pouvons ajouter une fonction de mise en forme à la classe code-behind de la page ASP.net s, à la couche de logique métier ou à toute autre bibliothèque de classes accessible à partir de l’application Web. Cette fonction de mise en forme peut accepter un nombre arbitraire de paramètres d’entrée, mais elle doit retourner le code HTML à afficher dans le modèle. Les fonctions de mise en forme ont d’abord été examinées dans le didacticiel [utilisation de TemplateFields dans le contrôle GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) .

Ces deux techniques de mise en forme sont disponibles avec les contrôles DataList et Repeater. Dans ce didacticiel, nous allons parcourir les exemples qui utilisent les deux techniques pour les deux contrôles.

## <a name="using-theitemdataboundevent-handler"></a>Utilisation du gestionnaire d’événements`ItemDataBound`

Lorsque les données sont liées à un contrôle DataList, soit à partir d’un contrôle de source de données, soit en affectant par programmation les données à la propriété Control s `DataSource` et en appelant sa méthode `DataBind()`, l’événement DataList s `DataBinding` est déclenché, la source de données est énumérée et chaque enregistrement de données est lié au contrôle DataList. Pour chaque enregistrement de la source de données, DataList crée un objet [`DataListItem`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.aspx) qui est ensuite lié à l’enregistrement actif. Au cours de ce processus, le contrôle DataList déclenche deux événements :

- **`ItemCreated`** se déclenche après la création du `DataListItem`
- **`ItemDataBound`** se déclenche après la liaison de l’enregistrement actif au `DataListItem`

Les étapes suivantes décrivent le processus de liaison de données pour le contrôle DataList.

1. L’événement DataList s [`DataBinding`](https://msdn.microsoft.com/library/system.web.ui.control.databinding.aspx) se déclenche
2. Les données sont liées au contrôle DataList  
  
   Pour chaque enregistrement dans la source de données 

    1. Créer un objet `DataListItem`
    2. Déclencher l' [événement`ItemCreated`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemcreated.aspx)
    3. Lier l’enregistrement au `DataListItem`
    4. Déclencher l' [événement`ItemDataBound`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemdatabound.aspx)
    5. Ajouter le `DataListItem` à la collection de `Items`

Lors de la liaison de données au contrôle Repeater, il progresse dans la même séquence d’étapes. La seule différence est qu’au lieu de créer des instances de `DataListItem`, le Repeater utilise [`RepeaterItem`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem(VS.80).aspx)s.

> [!NOTE]
> Le lecteur astucieux a peut-être remarqué une légère anomalie entre la séquence d’étapes qui s’écoulent lorsque le contrôle DataList et le Repeater sont liés aux données et lorsque le contrôle GridView est lié aux données. À la fin du processus de liaison de données, le GridView déclenche l’événement `DataBound` ; Toutefois, ni le contrôle DataList ni le contrôle Repeater n’ont un tel événement. Cela est dû au fait que les contrôles DataList et Repeater ont été recréés dans le délai de ASP.NET 1. x, avant que le modèle de gestionnaire d’événements pré-et postérieur au niveau de l’événement devienne courant.

Comme avec le GridView, une option de mise en forme basée sur les données consiste à créer un gestionnaire d’événements pour l’événement `ItemDataBound`. Ce gestionnaire d’événements inspecte les données qui viennent d’être liées au `DataListItem` ou `RepeaterItem` et affecte la mise en forme du contrôle en fonction des besoins.

Pour le contrôle DataList, les modifications de mise en forme pour l’élément entier peuvent être implémentées à l’aide des propriétés de style `DataListItem` s, qui incluent le `Font`standard, `ForeColor`, `BackColor`, `CssClass`, et ainsi de suite. Pour affecter la mise en forme de contrôles Web particuliers dans le modèle DataList, nous devons accéder par programmation et modifier le style de ces contrôles Web. Nous avons vu comment y parvenir à nouveau dans la *mise en forme personnalisée basée* sur le didacticiel sur les données. À l’instar du contrôle Repeater, la classe `RepeaterItem` n’a pas de propriétés liées au style ; par conséquent, toutes les modifications liées au style apportées à un `RepeaterItem` dans le gestionnaire d’événements `ItemDataBound` doivent être effectuées par le biais d’un programme qui accède aux contrôles Web et les met à jour dans le modèle.

Étant donné que la technique de mise en forme de `ItemDataBound` pour les contrôles DataList et Repeater est pratiquement identique, notre exemple se concentre sur l’utilisation de la DataList.

## <a name="step-1-displaying-product-information-in-the-datalist"></a>Étape 1 : affichage des informations sur le produit dans le contrôle DataList

Avant de vous soucier de la mise en forme, commençons par créer une page qui utilise un contrôle DataList pour afficher les informations sur le produit. Dans le [didacticiel précédent](displaying-data-with-the-datalist-and-repeater-controls-cs.md) , nous avons créé un contrôle DataList dont les `ItemTemplate` affichent chaque nom, catégorie, fournisseur, quantité par unité et tarif du produit. Commençons cette fonctionnalité ici dans ce didacticiel. Pour ce faire, vous pouvez recréer la DataList et son ObjectDataSource à partir de zéro, ou vous pouvez copier ces contrôles à partir de la page créée dans le didacticiel précédent (`Basics.aspx`) et les coller dans la page de ce didacticiel (`Formatting.aspx`).

Une fois que vous avez répliqué les fonctionnalités de DataList et ObjectDataSource de `Basics.aspx` dans `Formatting.aspx`, prenez un moment pour modifier la propriété DataList s `ID` de `DataList1` à un `ItemDataBoundFormattingExample`plus descriptif. Ensuite, affichez le contrôle DataList dans un navigateur. Comme le montre la figure 1, la seule différence de mise en forme entre chaque produit est que la couleur d’arrière-plan alterne.

[![les produits sont répertoriés dans le contrôle DataList](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image2.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image1.png)

**Figure 1**: les produits sont répertoriés dans le contrôle DataList ([cliquez pour afficher l’image en taille réelle](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image3.png))

Pour ce didacticiel, nous allons mettre en forme le contrôle DataList de sorte que tous les produits dont le prix est inférieur à $20,00 auront à la fois le nom et le prix unitaire mis en surbrillance en jaune.

## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-itemdatabound-event-handler"></a>Étape 2 : détermination par programmation de la valeur des données dans le gestionnaire d’événements ItemDataBound

Étant donné que seuls les produits dont le prix est inférieur à $20,00 auront la mise en forme personnalisée appliquée, nous devons être en mesure de déterminer le prix de chaque produit. Lors de la liaison de données à un contrôle DataList, DataList énumère les enregistrements dans sa source de données et, pour chaque enregistrement, crée une instance `DataListItem`, en liant l’enregistrement de la source de données au `DataListItem`. Une fois que les données d’enregistrement particulières ont été liées à l’objet de `DataListItem` actif, l’événement de `ItemDataBound` DataList s est déclenché. Nous pouvons créer un gestionnaire d’événements pour cet événement afin d’inspecter les valeurs des données pour le `DataListItem` actuel et, en fonction de ces valeurs, apporter les modifications nécessaires à la mise en forme.

Créez un événement `ItemDataBound` pour le contrôle DataList et ajoutez le code suivant :

[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample1.cs)]

Tandis que le concept et la sémantique du gestionnaire d’événements de la DataList `ItemDataBound` sont les mêmes que ceux utilisés par le gestionnaire d’événements `RowDataBound` GridView dans la *mise en forme personnalisée basée sur les données* , la syntaxe diffère légèrement. Lorsque l’événement `ItemDataBound` se déclenche, le `DataListItem` juste lié aux données est passé dans le gestionnaire d’événements correspondant via `e.Item` (au lieu de `e.Row`, comme avec le gestionnaire d’événements `RowDataBound` GridView s). Le gestionnaire d’événements DataList s `ItemDataBound` se déclenche pour *chaque* ligne ajoutée au contrôle DataList, y compris les lignes d’en-tête, les lignes de pied de page et les lignes de séparateur. Toutefois, les informations sur le produit sont uniquement liées aux lignes de données. Par conséquent, lors de l’utilisation de l’événement `ItemDataBound` pour inspecter les données liées au contrôle DataList, nous devons tout d’abord nous assurer que nous travaillons à nouveau avec un élément de données. Pour ce faire, vous pouvez vérifier la propriété `DataListItem` s [`ItemType`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.itemtype.aspx), qui peut avoir l’une des [huit valeurs suivantes](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listitemtype.aspx):

- `AlternatingItem`
- `EditItem`
- `Footer`
- `Header`
- `Item`
- `Pager`
- `SelectedItem`
- `Separator`

`Item` et `AlternatingItem``DataListItem` compensent les éléments de données DataList s. En supposant que nous travaillons avec un `Item` ou `AlternatingItem`, nous avons accès à l’instance de `ProductsRow` réelle qui était liée à la `DataListItem`actuelle. La propriété `DataListItem` s [`DataItem`](https://msdn.microsoft.com/system.web.ui.webcontrols.datalistitem.dataitem.aspx) contient une référence à l’objet `DataRowView`, dont la propriété `Row` fournit une référence à l’objet `ProductsRow` réel.

Ensuite, nous vérifions la propriété `ProductsRow` `UnitPrice` des instances. Étant donné que le champ Products table s `UnitPrice` autorise `NULL` valeurs, avant d’essayer d’accéder à la propriété `UnitPrice`, nous devons d’abord vérifier si elle a une valeur `NULL` à l’aide de la méthode `IsUnitPriceNull()`. Si la valeur `UnitPrice` n’est pas `NULL`, nous vérifions si elle est inférieure à $20,00. Si elle est effectivement inférieure à $20,00, nous devons ensuite appliquer la mise en forme personnalisée.

## <a name="step-3-highlighting-the-product-s-name-and-price"></a>Étape 3 : mise en surbrillance du nom et du prix du produit

Une fois que nous savons que le prix d’un produit est inférieur à $20,00, il ne reste plus qu’à mettre en évidence son nom et son prix. Pour ce faire, nous devons d’abord référencer par programmation les contrôles Label dans le `ItemTemplate` qui affichent le nom et le prix du produit. Ensuite, nous devons faire en sorte qu’ils affichent un arrière-plan jaune. Ces informations de mise en forme peuvent être appliquées en modifiant directement les étiquettes `BackColor` les propriétés (`LabelID.BackColor = Color.Yellow`); dans l’idéal, toutefois, toutes les questions relatives à l’affichage doivent être exprimées par le biais de feuilles de style en cascade. En fait, nous disposons déjà d’une feuille de style qui fournit la mise en forme souhaitée définie dans `Styles.css` - `AffordablePriceEmphasis`, qui a été créée et discutée dans le didacticiel de *mise en forme personnalisée basée sur les données* .

Pour appliquer la mise en forme, définissez simplement les deux contrôles Web label `CssClass` propriétés sur `AffordablePriceEmphasis`, comme illustré dans le code suivant :

[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample2.cs)]

Une fois le gestionnaire d’événements `ItemDataBound` terminé, revisitez la page `Formatting.aspx` dans un navigateur. Comme le montre la figure 2, les produits dont le prix est inférieur à $20,00 ont à la fois leur nom et leur prix mis en surbrillance.

[![les produits inférieurs à $20,00 sont mis en surbrillance](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image5.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image4.png)

**Figure 2**: les produits inférieurs à $20,00 sont mis en surbrillance ([cliquez pour afficher l’image en taille réelle](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image6.png))

> [!NOTE]
> Étant donné que la DataList est rendue sous la forme d’un `<table>`HTML, ses instances de `DataListItem` ont des propriétés liées au style qui peuvent être définies pour appliquer un style spécifique à l’élément entier. Par exemple, si nous voulons mettre en surbrillance la *totalité* de l’élément jaune lorsque son prix était inférieur à $20,00, nous pourrions avoir remplacé le code qui faisait référence aux étiquettes et définir leurs propriétés de `CssClass` avec la ligne de code suivante : `e.Item.CssClass = "AffordablePriceEmphasis"` (voir figure 3).

Les `RepeaterItem` qui composent le contrôle Repeater, mais don t offrent de telles propriétés au niveau du style. Par conséquent, l’application de la mise en forme personnalisée au Repeater requiert l’application de propriétés de style aux contrôles Web dans les modèles Repeater, comme nous l’avons fait dans la figure 2.

[![l’ensemble de l’article de produit est mis en surbrillance pour les produits sous $20,00](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image8.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image7.png)

**Figure 3**: la totalité de l’élément de produit est mise en surbrillance pour les produits sous $20,00 ([cliquez pour afficher l’image en taille réelle](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image9.png))

## <a name="using-formatting-functions-from-within-the-template"></a>Utilisation des fonctions de mise en forme à partir du modèle

Dans le didacticiel sur l' *utilisation de TemplateFields dans le contrôle GridView* , nous avons vu comment utiliser une fonction de mise en forme dans un contrôle de contenu GridView pour appliquer une mise en forme personnalisée basée sur les données liées aux lignes de GridView. Une fonction de mise en forme est une méthode qui peut être appelée à partir d’un modèle et retourne le code HTML à émettre à la place. Les fonctions de mise en forme peuvent résider dans la classe code-behind de la page ASP.NET ou peuvent être centralisées en fichiers de classe dans le dossier `App_Code` ou dans un projet de bibliothèque de classes distinct. Le déplacement de la fonction de mise en forme en dehors de la classe code-behind de la page ASP.NET est idéal si vous prévoyez d’utiliser la même fonction de mise en forme dans plusieurs pages ASP.NET ou dans d’autres applications Web ASP.NET.

Pour illustrer les fonctions de mise en forme, les informations sur le produit sont les suivantes : texte [abandonné] en regard du nom du produit s’il n’est plus disponible. En outre, les prix sont mis en surbrillance en jaune s’ils sont inférieurs à $20,00 (comme nous l’avons fait dans le `ItemDataBound` exemple de gestionnaire d’événements); Si le prix est de $20,00 ou supérieur, ne pas afficher le prix réel, mais à la place le texte, veuillez appeler pour obtenir un devis. La figure 4 montre une capture d’écran de la liste des produits avec ces règles de mise en forme appliquées.

[![pour les produits onéreux, le prix est remplacé par le texte, veuillez appeler pour obtenir un devis](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image11.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image10.png)

**Figure 4**: pour les produits onéreux, le prix est remplacé par le texte, veuillez appeler pour obtenir un devis ([cliquez pour afficher l’image en plein écran](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image12.png))

## <a name="step-1-create-the-formatting-functions"></a>Étape 1 : créer les fonctions de mise en forme

Pour cet exemple, nous avons besoin de deux fonctions de mise en forme, une qui affiche le nom du produit ainsi que le texte [abandonné], si nécessaire, et un autre qui affiche soit un prix mis en surbrillance s’il est inférieur à $20,00, soit un devis pour un tarif dans le cas contraire. Créons ces fonctions dans la classe code-behind de la page ASP.NET et nommez-les `DisplayProductNameAndDiscontinuedStatus` et `DisplayPrice`. Les deux méthodes doivent retourner le HTML à restituer sous forme de chaîne et doivent être marquées `Protected` (ou `Public`) pour pouvoir être appelées à partir de la partie de la syntaxe déclarative de la page ASP.NET. Voici le code de ces deux méthodes :

[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample3.cs)]

Notez que la méthode `DisplayProductNameAndDiscontinuedStatus` accepte les valeurs des champs de données `productName` et `discontinued` en tant que valeurs scalaires, tandis que la méthode `DisplayPrice` accepte une instance `ProductsRow` (plutôt qu’une valeur scalaire `unitPrice`). L’une ou l’autre approche fonctionnera. Toutefois, si la fonction de mise en forme fonctionne avec des valeurs scalaires qui peuvent contenir des valeurs de `NULL` de base de données (telles que `UnitPrice`. ni `ProductName` ni `Discontinued` n’autorisent les valeurs `NULL`), il convient de veiller à gérer ces entrées scalaires.

En particulier, le paramètre d’entrée doit être de type `Object`, car la valeur entrante peut être une `DBNull` instance au lieu du type de données attendu. En outre, un contrôle doit être effectué pour déterminer si la valeur entrante est une valeur de `NULL` de base de données. Autrement dit, si nous souhaitons que la méthode `DisplayPrice` accepte le prix en tant que valeur scalaire, nous devons utiliser le code suivant :

[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample4.cs)]

Notez que le paramètre d’entrée `unitPrice` est de type `Object` et que l’instruction conditionnelle a été modifiée pour s’assurer que `unitPrice` est `DBNull` ou non. En outre, étant donné que le paramètre d’entrée `unitPrice` est passé en tant que `Object`, il doit être converti en valeur décimale.

## <a name="step-2-calling-the-formatting-function-from-the-datalist-s-itemtemplate"></a>Étape 2 : appel de la fonction de mise en forme à partir du ItemTemplate de DataList

Avec les fonctions de mise en forme ajoutées à la classe code-behind de la page ASP.NET, il ne reste plus qu’à appeler ces fonctions de mise en forme à partir de la `ItemTemplate`de DataList. Pour appeler une fonction de mise en forme à partir d’un modèle, placez l’appel de fonction dans la syntaxe DataBinding :

[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample5.aspx)]

Dans DataList s `ItemTemplate` le contrôle Web `ProductNameLabel` label affiche actuellement le nom du produit en affectant à sa propriété `Text` le résultat de `<%# Eval("ProductName") %>`. Pour qu’elle affiche le nom plus le texte [Discontinued], si nécessaire, mettez à jour la syntaxe déclarative afin qu’elle affecte plutôt la propriété `Text` la valeur de la méthode `DisplayProductNameAndDiscontinuedStatus`. Dans ce cas, nous devons transmettre le nom du produit et les valeurs supprimées à l’aide de la syntaxe `Eval("columnName")`. `Eval` retourne une valeur de type `Object`, mais la méthode `DisplayProductNameAndDiscontinuedStatus` attend des paramètres d’entrée de type `String` et `Boolean`; par conséquent, nous devons effectuer un cast des valeurs retournées par la méthode `Eval` vers les types de paramètres d’entrée attendus, comme suit :

[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample6.aspx)]

Pour afficher le prix, nous pouvons simplement définir la propriété `UnitPriceLabel` label s `Text` sur la valeur retournée par la méthode `DisplayPrice`, comme nous l’avons fait pour afficher le nom du produit et le texte [abandonné]. Toutefois, au lieu de passer le `UnitPrice` en tant que paramètre d’entrée scalaire, nous transmettons la totalité de l’instance de `ProductsRow` :

[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample7.aspx)]

Avec les appels aux fonctions de mise en forme en place, prenez un moment pour voir notre progression dans un navigateur. Votre écran doit ressembler à la figure 5, avec les produits abandonnés, y compris le texte [abandonné] et les produits dont le coût est supérieur à $20,00, dont le prix est remplacé par le texte veuillez appeler pour obtenir un devis.

[![pour les produits onéreux, le prix est remplacé par le texte, veuillez appeler pour obtenir un devis](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image14.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image13.png)

**Figure 5**: pour les produits onéreux, le prix est remplacé par le texte, veuillez appeler pour obtenir un devis ([cliquez pour afficher l’image en plein écran](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image15.png))

## <a name="summary"></a>Récapitulatif

La mise en forme du contenu d’un contrôle DataList ou Repeater basé sur les données peut être accomplie à l’aide de deux techniques. La première technique consiste à créer un gestionnaire d’événements pour l’événement `ItemDataBound`, qui se déclenche lorsque chaque enregistrement de la source de données est lié à un nouveau `DataListItem` ou `RepeaterItem`. Dans le gestionnaire d’événements `ItemDataBound`, les données de l’élément actuel peuvent être examinées, puis la mise en forme peut être appliquée au contenu du modèle ou, pour `DataListItem` s, à l’élément entier lui-même.

Une mise en forme personnalisée peut également être réalisée par le biais de fonctions de mise en forme. Une fonction de mise en forme est une méthode qui peut être appelée à partir des modèles DataList ou Repeater, qui retourne le code HTML à émettre à la place. Souvent, le code HTML retourné par une fonction de mise en forme est déterminé par les valeurs qui sont liées à l’élément actuel. Ces valeurs peuvent être passées à la fonction de mise en forme en tant que valeurs scalaires ou en passant la totalité de l’objet lié à l’élément (par exemple, l’instance `ProductsRow`).

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel étaient Yaakov Ellis, Randy Schmidt et Liz Shulok. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](displaying-data-with-the-datalist-and-repeater-controls-cs.md)
> [Suivant](showing-multiple-records-per-row-with-the-datalist-control-cs.md)
