---
uid: web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb
title: Maître/détail utilisant un GridView maître sélectionnable avec un DetailView des détails (VB) | Microsoft Docs
author: rick-anderson
description: Ce didacticiel aura un GridView dont les lignes incluent le nom et le prix de chaque produit, ainsi qu’un bouton Sélectionner. Cliquer sur le bouton Sélectionner pour un particu...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 1d1a7c93-971d-4690-9c5e-dac0e5014a09
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb
msc.type: authoredcontent
ms.openlocfilehash: a953c00acc4c37fd563321477b6b21689d6e686c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74576278"
---
# <a name="masterdetail-using-a-selectable-master-gridview-with-a-details-detailview-vb"></a>Représentation maître/détail utilisant un GridView maître pouvant être sélectionné avec une DetailView des détails (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_10_VB.exe) ou [Télécharger le PDF](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/datatutorial10vb1.pdf)

> Ce didacticiel aura un GridView dont les lignes incluent le nom et le prix de chaque produit, ainsi qu’un bouton Sélectionner. Le fait de cliquer sur le bouton Sélectionner pour un produit particulier entraîne l’affichage de ses détails complets dans un contrôle DetailsView sur la même page.

## <a name="introduction"></a>Introduction

Dans le [didacticiel précédent](master-detail-filtering-across-two-pages-vb.md) , nous avons vu comment créer un rapport maître/détail à l’aide de deux pages Web : une page Web « Master », à partir de laquelle nous avons affiché la liste des fournisseurs. et une page Web « détails » qui répertorie les produits fournis par le fournisseur sélectionné. Ce format de rapport à deux pages peut être condensé dans une page. Ce didacticiel aura un GridView dont les lignes incluent le nom et le prix de chaque produit, ainsi qu’un bouton Sélectionner. Le fait de cliquer sur le bouton Sélectionner pour un produit particulier entraîne l’affichage de ses détails complets dans un contrôle DetailsView sur la même page.

[![cliquant sur le bouton Sélectionner affiche les détails du produit](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image2.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image1.png)

**Figure 1**: le fait de cliquer sur le bouton Sélectionner affiche les détails du produit ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image3.png))

## <a name="step-1-creating-a-selectable-gridview"></a>Étape 1 : création d’un contrôle GridView sélectionnable

Souvenez-vous que dans le rapport maître/détail sur deux pages, chaque enregistrement contient un lien hypertexte qui, lorsque vous cliquez dessus, l’a envoyé à la page de détails en passant la valeur `SupplierID` de la ligne sur laquelle l’utilisateur a cliqué dans la chaîne de chaîne. Un lien hypertexte de ce type a été ajouté à chaque ligne GridView à l’aide d’un HyperLinkField. Pour le rapport maître/détails d’une seule page, nous aurons besoin d’un bouton pour chaque ligne GridView qui, lorsque vous cliquez dessus, affiche les détails. Le contrôle GridView peut être configuré pour inclure un bouton Sélectionner pour chaque ligne qui provoque une publication et marque cette ligne comme [SelectedRow](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedrow.aspx)du GridView.

Commencez par ajouter un contrôle GridView à la page `DetailsBySelecting.aspx` dans le dossier `Filtering`, en affectant à sa propriété `ID` la valeur `ProductsGrid`. Ensuite, ajoutez un nouvel ObjectDataSource nommé `AllProductsDataSource` qui appelle la méthode `GetProducts()` de la classe `ProductsBLL`.

[![créer un ObjectDataSource nommé AllProductsDataSource](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image5.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image4.png)

**Figure 2**: créer un ObjectDataSource nommé `AllProductsDataSource` ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image6.png))

[![utiliser la classe ProductsBLL](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image8.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image7.png)

**Figure 3**: utiliser la classe `ProductsBLL` ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image9.png))

[![configurer ObjectDataSource pour appeler la méthode GetProducts ()](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image11.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image10.png)

**Figure 4**: configurer ObjectDataSource pour appeler la méthode `GetProducts()` ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image12.png))

Modifiez les champs du contrôle GridView en supprimant tous les `ProductName` et `UnitPrice` BoundFields. En outre, n’hésitez pas à personnaliser ces BoundFields en fonction des besoins, comme la mise en forme de la `UnitPrice` BoundField en tant que devise et la modification des propriétés de `HeaderText` du BoundFields. Ces étapes peuvent être effectuées graphiquement, en cliquant sur le lien modifier les colonnes de la balise active de GridView ou en configurant manuellement la syntaxe déclarative.

[![supprimer tout sauf les BoundFields ProductName et UnitPrice](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image14.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image13.png)

**Figure 5**: supprimer tout sauf les `ProductName` et `UnitPrice` BoundFields ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image15.png))

Le balisage final du GridView est le suivant :

[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample1.aspx)]

Ensuite, nous devons marquer le contrôle GridView comme sélectionnable, ce qui ajoute un bouton Sélectionner à chaque ligne. Pour ce faire, activez simplement la case à cocher Activer la sélection dans la balise active de GridView.

[![rendre les lignes de GridView sélectionnables](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image17.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image16.png)

**Figure 6**: rendre les lignes du GridView sélectionnables ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image18.png))

La vérification de l’option Activer la sélection ajoute un CommandField au `ProductsGrid` GridView avec sa propriété `ShowSelectButton` définie sur true. Cela génère un bouton Sélectionner pour chaque ligne du contrôle GridView, comme le montre la figure 6. Par défaut, les boutons de sélection sont rendus sous la forme LinkButtons, mais vous pouvez utiliser des boutons ou des ImageButtons à la place de la propriété `ButtonType` de CommandField.

[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample2.aspx)]

Lorsqu’un clic est effectué sur le bouton de sélection d’une ligne GridView, une publication est effectuée et la propriété `SelectedRow` du GridView est mise à jour. En plus de la propriété `SelectedRow`, le contrôle GridView fournit les propriétés [SelectedIndex](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex%28VS.80%29.aspx), [SelectedValue](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue%28VS.80%29.aspx)et [SelectedDataKey](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selecteddatakey%28VS.80%29.aspx) . La propriété `SelectedIndex` retourne l’index de la ligne sélectionnée, tandis que les propriétés `SelectedValue` et `SelectedDataKey` retournent des valeurs en fonction de la [propriété DataKeyNames](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames%28VS.80%29.aspx)de GridView.

La propriété `DataKeyNames` est utilisée pour associer une ou plusieurs valeurs de champ de données à chaque ligne et est couramment utilisée pour attribuer de manière unique des informations d’identification à partir des données sous-jacentes à chaque ligne GridView. La propriété `SelectedValue` retourne la valeur du premier champ de données `DataKeyNames` pour la ligne sélectionnée où comme la propriété `SelectedDataKey` retourne l’objet `DataKey` de la ligne sélectionnée, qui contient toutes les valeurs des champs de clé de données spécifiés pour cette ligne.

La propriété `DataKeyNames` est automatiquement définie sur le ou les champs de données d’identification unique lorsque vous liez une source de données à un GridView, DetailsView ou FormView via le concepteur. Bien que cette propriété soit définie automatiquement pour nous dans les didacticiels précédents, les exemples fonctionnaient sans la propriété `DataKeyNames` spécifiée. Toutefois, pour le GridView sélectionnable dans ce didacticiel, ainsi que pour les prochains didacticiels dans lesquels nous examinerons l’insertion, la mise à jour et la suppression, la propriété `DataKeyNames` doit être définie correctement. Prenez un moment pour vous assurer que la propriété de `DataKeyNames` de votre GridView est définie sur `ProductID`.

Nous allons voir notre progression jusqu’à présent dans un navigateur. Notez que le contrôle GridView répertorie le nom et le prix de tous les produits, ainsi qu’un contrôle de sélection. Le fait de cliquer sur le bouton Sélectionner entraîne une publication (postback). À l’étape 2, nous verrons comment un DetailsView répond à cette publication en affichant les détails du produit sélectionné.

[![chaque ligne de produit contient un LinkButton Select](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image20.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image19.png)

**Figure 7**: chaque ligne de produit contient un LinkButton Select ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image21.png))

## <a name="highlighting-the-selected-row"></a>Mise en surbrillance de la ligne sélectionnée

Le `ProductsGrid` GridView a une propriété `SelectedRowStyle` qui peut être utilisée pour dicter le style visuel de la ligne sélectionnée. Utilisé correctement, cela peut améliorer l’expérience de l’utilisateur en indiquant plus clairement la ligne du GridView actuellement sélectionnée. Pour ce didacticiel, nous allons mettre en surbrillance la ligne sélectionnée à l’aide d’un arrière-plan jaune.

Comme pour nos didacticiels précédents, essayons de conserver les paramètres esthétiques définis comme des classes CSS. Par conséquent, créez une nouvelle classe CSS dans `Styles.css` nommée `SelectedRowStyle`.

[!code-css[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample3.css)]

Pour appliquer cette classe CSS à la propriété `SelectedRowStyle` de *tous les* GridViews de notre série de didacticiels, modifiez l’apparence `GridView.skin` dans le thème `DataWebControls` pour inclure les paramètres de `SelectedRowStyle` comme indiqué ci-dessous :

[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample4.aspx)]

Avec cet ajout, la ligne GridView sélectionnée est maintenant mise en surbrillance avec une couleur d’arrière-plan jaune.

[![personnaliser l’apparence de la ligne sélectionnée à l’aide de la propriété SelectedRowStyle du contrôle GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image23.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image22.png)

**Figure 8**: personnaliser l’apparence de la ligne sélectionnée à l’aide de la propriété `SelectedRowStyle` du GridView ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image24.png))

## <a name="step-2-displaying-the-selected-products-details-in-a-detailsview"></a>Étape 2 : affichage des détails du produit sélectionné dans un DetailsView

Une fois l' `ProductsGrid` GridView terminée, il ne reste plus qu’à ajouter un DetailsView qui affiche des informations sur le produit sélectionné. Ajoutez un contrôle DetailsView au-dessus de GridView et créez un nouvel ObjectDataSource nommé `ProductDetailsDataSource`. Comme nous voulons que ce DetailsView affiche des informations particulières sur le produit sélectionné, configurez le `ProductDetailsDataSource` pour utiliser la méthode `GetProductByProductID(productID)` de la classe `ProductsBLL`.

[![invoquer la méthode GetProductByProductID (productID) de la classe ProductsBLL](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image26.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image25.png)

**Figure 9**: appeler la méthode `GetProductByProductID(productID)` de la classe `ProductsBLL` ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image27.png))

La valeur du paramètre *`productID`* est obtenue à partir de la propriété `SelectedValue` du contrôle GridView. Comme nous l’avons vu précédemment, la propriété `SelectedValue` de GridView retourne la première valeur de clé de données pour la ligne sélectionnée. Par conséquent, il est impératif que la propriété de `DataKeyNames` de GridView soit définie sur `ProductID`, afin que la valeur de `ProductID` de la ligne sélectionnée soit retournée par `SelectedValue`.

[![définir le paramètre productID sur la propriété SelectedValue du GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image29.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image28.png)

**Figure 10**: définir le paramètre *`productID`* sur la propriété `SelectedValue` du contrôle GridView ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image30.png))

Une fois que le `productDetailsDataSource` ObjectDataSource a été configuré correctement et lié au contrôle DetailsView, ce didacticiel est terminé. Lorsque la page est visitée pour la première fois, aucune ligne n’est sélectionnée, donc la propriété `SelectedValue` du GridView retourne `Nothing`. Étant donné qu’il n’existe aucun produit avec une valeur de `ProductID` `NULL`, aucun enregistrement n’est retourné par la méthode `GetProductByProductID(productID)`, ce qui signifie que le DetailsView n’est pas affiché (voir figure 11). Lorsque vous cliquez sur le bouton de sélection d’une ligne GridView, une publication est effectuée et le DetailsView est actualisé. Cette fois, la propriété `SelectedValue` de GridView retourne le `ProductID` de la ligne sélectionnée, la méthode `GetProductByProductID(productID)` retourne une `ProductsDataTable` avec les informations relatives à ce produit et le DetailsView affiche ces détails (voir figure 12).

[![lors de la première visite, seul le contrôle GridView est affiché](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image32.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image31.png)

**Figure 11**: lors de la première visite, seul le contrôle GridView s’affiche ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image33.png))

[![lorsque vous sélectionnez une ligne, les détails du produit s’affichent.](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image35.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image34.png)

**Figure 12**: lors de la sélection d’une ligne, les détails du produit s’affichent ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image36.png))

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel et les trois didacticiels précédents, nous avons vu un certain nombre de techniques permettant d’afficher les rapports maître/détail. Dans ce didacticiel, nous avons examiné à l’aide d’un GridView sélectionnable pour héberger les enregistrements principaux et un DetailsView pour afficher des détails sur l’enregistrement maître sélectionné sur la même page. Dans les didacticiels précédents, nous avons vu comment afficher des rapports maître/détails à l’aide de DropDownList et afficher des enregistrements maîtres sur une page Web et des enregistrements de détails sur une autre.

Ce didacticiel conclut notre examen des rapports maître/détail. À partir du prochain didacticiel, nous allons commencer à explorer la mise en forme personnalisée avec les contrôles GridView, DetailsView et FormView. Nous verrons comment personnaliser l’apparence de ces contrôles en fonction des données qui leur sont liées, comment synthétiser des données dans le pied de page du GridView et comment utiliser des modèles pour obtenir un degré de contrôle accru sur la disposition.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel était Hilton Giesenow. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](master-detail-filtering-across-two-pages-vb.md)
