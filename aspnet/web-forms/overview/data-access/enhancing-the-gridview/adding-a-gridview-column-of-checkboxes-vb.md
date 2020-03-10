---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb
title: Ajout d’une colonne GridView de cases à cocher (VB) | Microsoft Docs
author: rick-anderson
description: Ce didacticiel explique comment ajouter une colonne de cases à cocher à un contrôle GridView pour fournir à l’utilisateur un moyen intuitif de sélectionner plusieurs lignes du G...
ms.author: riande
ms.date: 03/06/2007
ms.assetid: 39253d05-75c0-41c7-b9d4-a6c58ecf69ce
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: c620b2eac5844d4030c1309b45e7d6a72d1f386a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78607351"
---
# <a name="adding-a-gridview-column-of-checkboxes-vb"></a>Ajout d’une colonne GridView de cases à cocher (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_52_VB.exe) ou [Télécharger le PDF](adding-a-gridview-column-of-checkboxes-vb/_static/datatutorial52vb1.pdf)

> Ce didacticiel explique comment ajouter une colonne de cases à cocher à un contrôle GridView pour fournir à l’utilisateur un moyen intuitif de sélectionner plusieurs lignes du GridView.

## <a name="introduction"></a>Introduction

Dans le didacticiel précédent, nous avons examiné comment ajouter une colonne de cases d’option au GridView en vue de sélectionner un enregistrement particulier. Une colonne de cases d’option est une interface utilisateur appropriée lorsque l’utilisateur se limite à choisir au plus un élément de la grille. Toutefois, il peut arriver que nous puissions autoriser l’utilisateur à choisir un nombre arbitraire d’éléments dans la grille. Les clients de messagerie Web, par exemple, affichent généralement la liste des messages avec une colonne de cases à cocher. L’utilisateur peut sélectionner un nombre arbitraire de messages, puis effectuer une action, comme déplacer les courriers électroniques vers un autre dossier ou les supprimer.

Dans ce didacticiel, nous allons voir comment ajouter une colonne de cases à cocher et comment déterminer les cases à cocher qui ont été vérifiées lors de la publication (postback). En particulier, nous allons créer un exemple qui imite fidèlement l’interface utilisateur du client de messagerie basée sur le Web. Notre exemple inclut un contrôle GridView paginé qui répertorie les produits de la table de base de données `Products` avec une case à cocher dans chaque ligne (voir figure 1). Si vous cliquez sur un bouton supprimer les produits sélectionnés, les produits sélectionnés seront supprimés.

[![chaque ligne de produit comprend une case à cocher](adding-a-gridview-column-of-checkboxes-vb/_static/image1.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image1.png)

**Figure 1**: chaque ligne de produit comprend une case à cocher ([cliquez pour afficher l’image en plein écran](adding-a-gridview-column-of-checkboxes-vb/_static/image2.png))

## <a name="step-1-adding-a-paged-gridview-that-lists-product-information"></a>Étape 1 : ajout d’un contrôle GridView paginé qui répertorie les informations sur les produits

Avant de vous soucier de l’ajout d’une colonne de cases à cocher, commençons par s’intéresser à la liste des produits dans un GridView qui prend en charge la pagination. Commencez par ouvrir la page `CheckBoxField.aspx` dans le dossier `EnhancedGridView` et faites glisser un contrôle GridView de la boîte à outils vers le concepteur, en affectant à sa `ID` la valeur `Products`. Ensuite, choisissez de lier le contrôle GridView à un nouvel ObjectDataSource nommé `ProductsDataSource`. Configurez l’ObjectDataSource pour utiliser la classe `ProductsBLL`, en appelant la méthode `GetProducts()` pour retourner les données. Étant donné que ce GridView sera en lecture seule, définissez les listes déroulantes dans les onglets mettre à jour, insérer et supprimer sur (aucun).

[![créer un ObjectDataSource nommé ProductsDataSource](adding-a-gridview-column-of-checkboxes-vb/_static/image2.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image3.png)

**Figure 2**: créer un ObjectDataSource nommé `ProductsDataSource` ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-checkboxes-vb/_static/image4.png))

[![configurer ObjectDataSource pour récupérer des données à l’aide de la méthode GetProducts ()](adding-a-gridview-column-of-checkboxes-vb/_static/image3.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image5.png)

**Figure 3**: configurer ObjectDataSource pour récupérer des données à l’aide de la méthode `GetProducts()` ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-checkboxes-vb/_static/image6.png))

[![définir les listes déroulantes dans les onglets mettre à jour, insérer et supprimer sur (aucun)](adding-a-gridview-column-of-checkboxes-vb/_static/image4.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image7.png)

**Figure 4**: définir les listes déroulantes des onglets mettre à jour, insérer et supprimer sur (aucun) ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-checkboxes-vb/_static/image8.png))

À l’issue de l’exécution de l’Assistant Configuration de la source de données, Visual Studio crée automatiquement BoundColumns et CheckBoxColumn pour les champs de données liés au produit. Comme nous l’avons fait dans le didacticiel précédent, supprimez tout sauf les `ProductName`, `CategoryName`et `UnitPrice` BoundFields, et remplacez les propriétés de `HeaderText` par Product, Category et Price. Configurez le `UnitPrice` BoundField afin que sa valeur soit mise en forme en tant que devise. Configurez également le contrôle GridView pour prendre en charge la pagination en activant la case à cocher Activer la pagination dans la balise active.

Let s ajoute également l’interface utilisateur pour supprimer les produits sélectionnés. Ajoutez un contrôle Web Button sous le contrôle GridView, en affectant à son `ID` la valeur `DeleteSelectedProducts` et sa propriété `Text` pour supprimer les produits sélectionnés. Au lieu de supprimer réellement les produits de la base de données, pour cet exemple, nous affichons simplement un message indiquant les produits qui auraient été supprimés. Pour ce faire, ajoutez un contrôle Web Label sous le bouton. Définissez son ID sur `DeleteResults`, effacez sa propriété `Text` et définissez ses propriétés `Visible` et `EnableViewState` sur `False`.

Après avoir apporté ces modifications, les balises déclaratives GridView, ObjectDataSource, Button et label s doivent ressembler à ce qui suit :

[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample1.aspx)]

Prenez un moment pour afficher la page dans un navigateur (voir figure 5). À ce stade, vous devez voir le nom, la catégorie et le prix des dix premiers produits.

[![le nom, la catégorie et le prix des dix premiers produits sont répertoriés](adding-a-gridview-column-of-checkboxes-vb/_static/image5.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image9.png)

**Figure 5**: le nom, la catégorie et le prix des dix premiers produits sont répertoriés ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-checkboxes-vb/_static/image10.png))

## <a name="step-2-adding-a-column-of-checkboxes"></a>Étape 2 : ajout d’une colonne de cases à cocher

Étant donné que ASP.NET 2,0 comprend un CheckBoxField, il peut être possible de l’utiliser pour ajouter une colonne de cases à un GridView. Malheureusement, ce n’est pas le cas, car CheckBoxField est conçu pour fonctionner avec un champ de données booléen. Autrement dit, pour pouvoir utiliser le CheckBoxField, nous devons spécifier le champ de données sous-jacent dont la valeur est consultée pour déterminer si la case à cocher rendue est activée. Nous ne pouvons pas utiliser CheckBoxField pour inclure simplement une colonne de cases à cocher désactivées.

Au lieu de cela, nous devons ajouter un TemplateField et ajouter un contrôle Web CheckBox à son `ItemTemplate`. Continuez et ajoutez un TemplateField au `Products` GridView et définissez-le sur le premier champ (à gauche). À partir de la balise active GridView s, cliquez sur le lien modifier les modèles, puis faites glisser un contrôle CheckBox Web de la boîte à outils vers le `ItemTemplate`. Définissez cette case à cocher `ID` propriété sur `ProductSelector`.

[![ajouter un contrôle Web CheckBox nommé ProductSelector à TemplateField s ItemTemplate](adding-a-gridview-column-of-checkboxes-vb/_static/image6.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image11.png)

**Figure 6**: ajouter un contrôle Web CheckBox nommé `ProductSelector` à l' `ItemTemplate` TemplateField s ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-checkboxes-vb/_static/image12.png))

Lorsque le contrôle Web TemplateField et CheckBox est ajouté, chaque ligne comprend désormais une case à cocher. La figure 7 illustre cette page, quand elle est affichée dans un navigateur, après l’ajout du TemplateField et de la case à cocher.

[![chaque ligne de produit comprend désormais une case à cocher](adding-a-gridview-column-of-checkboxes-vb/_static/image7.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image13.png)

**Figure 7**: chaque ligne de produit comprend désormais une case à cocher ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-checkboxes-vb/_static/image14.png))

## <a name="step-3-determining-what-checkboxes-were-checked-on-postback"></a>Étape 3 : détermination des cases à cocher qui ont été vérifiées lors de la publication

À ce stade, nous avons une colonne de cases à cocher, mais aucun moyen de déterminer les cases à cocher qui ont été vérifiées lors de la publication (postback). Lorsque l’utilisateur clique sur le bouton supprimer les produits sélectionnés, nous avons besoin de connaître les cases à cocher qui ont été vérifiées afin de supprimer ces produits.

La propriété GridView s [`Rows`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows.aspx) permet d’accéder aux lignes de données dans le GridView. Nous pouvons itérer au sein de ces lignes, accéder par programmation au contrôle CheckBox, puis consulter sa propriété `Checked` pour déterminer si la case à cocher a été sélectionnée.

Créez un gestionnaire d’événements pour l’événement `DeleteSelectedProducts` Button Web Control s `Click` et ajoutez le code suivant :

[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample2.vb)]

La propriété `Rows` retourne une collection d’instances `GridViewRow` qui dégroupent les lignes de données du contrôle GridView. La boucle `For Each` ici énumère cette collection. Pour chaque objet `GridViewRow`, la case à cocher lignes s est accessible par programmation à l’aide de `row.FindControl("controlID")`. Si la case à cocher est activée, les lignes correspondantes `ProductID` valeur sont récupérées à partir de la collection de `DataKeys`. Dans cet exercice, nous affichons simplement un message d’information dans l’étiquette `DeleteResults`, bien que dans une application de travail, nous ayons à présent un appel à la méthode `ProductsBLL` classe s `DeleteProduct(productID)`.

Avec l’ajout de ce gestionnaire d’événements, un clic sur le bouton supprimer les produits sélectionnés affiche maintenant le `ProductID` s des produits sélectionnés.

[![lorsque l’utilisateur clique sur le bouton supprimer les produits sélectionnés, les ProductIDs de produits sélectionnés sont répertoriés](adding-a-gridview-column-of-checkboxes-vb/_static/image8.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image15.png)

**Figure 8**: lorsque l’utilisateur clique sur le bouton supprimer les produits sélectionnés, les produits sélectionnés `ProductID` s sont répertoriés ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-checkboxes-vb/_static/image16.png))

## <a name="step-4-adding-check-all-and-uncheck-all-buttons"></a>Étape 4 : ajout de l’option vérifier tout et désélectionner tous les boutons

Si un utilisateur souhaite supprimer tous les produits de la page actuelle, il doit cocher chacune des dix cases à cocher. Nous pouvons vous aider à accélérer ce processus en ajoutant un bouton vérifier tout qui, lorsque vous cliquez dessus, sélectionne toutes les cases à cocher dans la grille. Un bouton Désélectionner tout serait également utile.

Ajoutez deux contrôles Web Button à la page, en les plaçant au-dessus du contrôle GridView. Définissez la première `ID` sur `CheckAll` et sa propriété `Text` pour vérifier tout ; Définissez le deuxième s `ID` sur `UncheckAll` et sa propriété `Text` pour désélectionner tout.

[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample3.aspx)]

Ensuite, créez une méthode dans la classe code-behind nommée `ToggleCheckState(checkState)` qui, lorsqu’elle est appelée, énumère la collection `Products` GridView s `Rows` et affecte à chaque propriété CheckBox s `Checked` la valeur de la propriété passée dans *checkState* .

[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample4.vb)]

Ensuite, créez `Click` gestionnaires d’événements pour les boutons `CheckAll` et `UncheckAll`. Dans le gestionnaire d’événements `CheckAll` s, appelez simplement `ToggleCheckState(True)`; dans `UncheckAll`, appelez `ToggleCheckState(False)`.

[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample5.vb)]

Avec ce code, le fait de cliquer sur le bouton vérifier tout provoque une publication et vérifie toutes les cases à cocher dans le contrôle GridView. De même, si vous cliquez sur décocher tout, vous désactivez toutes les cases à cocher. La figure 9 illustre l’écran après que le bouton vérifier tout a été activé.

[![cliquant sur le bouton cocher tout sélectionne toutes les cases à cocher](adding-a-gridview-column-of-checkboxes-vb/_static/image9.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image17.png)

**Figure 9**: Si vous cliquez sur le bouton Sélectionner tout, vous cochez toutes les cases ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-checkboxes-vb/_static/image18.png))

> [!NOTE]
> Lors de l’affichage d’une colonne de cases à cocher, l’une des méthodes permettant de sélectionner ou désélectionner toutes les cases à cocher est d’utiliser une case à cocher dans la ligne d’en-tête. En outre, la case à cocher Activer tout/décocher toutes les implémentations requiert une publication. Les cases à cocher peuvent être activées ou désactivées, mais entièrement par le biais d’un script côté client, fournissant ainsi une expérience utilisateur réactive. Pour explorer à l’aide d’une case à cocher ligne d’en-tête pour vérifier tout et décocher tout en détail, ainsi qu’une discussion sur l’utilisation des techniques côté client, consultez cocher cocher [toutes les cases d’un GridView à l’aide d’un script côté client et d’une case à cocher tout sélectionner](http://aspnet.4guysfromrolla.com/articles/053106-1.aspx).

## <a name="summary"></a>Récapitulatif

Dans les cas où vous devez permettre aux utilisateurs de choisir un nombre arbitraire de lignes à partir d’un GridView avant de continuer, l’ajout d’une colonne de cases à cocher est une option. Comme nous l’avons vu dans ce didacticiel, y compris une colonne de cases à cocher dans le GridView implique l’ajout d’un TemplateField avec un contrôle Web CheckBox. En utilisant un contrôle Web (par opposition à l’injection directe du balisage dans le modèle, comme nous l’avons fait dans le didacticiel précédent), ASP.NET se souvient automatiquement des cases à cocher qui étaient et n’ont pas été vérifiées dans la publication (postback). Nous pouvons également accéder par programmation aux cases à cocher du code pour déterminer si une case à cocher donnée est activée, ou pour modifier l’état activé.

Ce didacticiel et le dernier examinent l’ajout d’une colonne de sélecteur de lignes au contrôle GridView. Dans notre prochain didacticiel, nous allons examiner comment, avec un peu de travail, nous pouvons ajouter des fonctionnalités d’insertion au contrôle GridView.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Précédent](adding-a-gridview-column-of-radio-buttons-vb.md)
> [Suivant](inserting-a-new-record-from-the-gridview-s-footer-vb.md)
