---
uid: web-forms/overview/data-access/working-with-batched-data/batch-deleting-vb
title: Suppression par lots (VB) | Microsoft Docs
author: rick-anderson
description: Découvrez comment supprimer plusieurs enregistrements de base de données en une seule opération. Dans la couche d’interface utilisateur, nous créons un GridView amélioré créé dans une TUT antérieure...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: 4fb72f75-32ab-4bf7-a764-be20367be726
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-deleting-vb
msc.type: authoredcontent
ms.openlocfilehash: 0974a16764eee2ef03cf36b4b15f9ef41f99982b
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74621759"
---
# <a name="batch-deleting-vb"></a>Suppression par lots (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_65_VB.zip) ou [Télécharger le PDF](batch-deleting-vb/_static/datatutorial65vb1.pdf)

> Découvrez comment supprimer plusieurs enregistrements de base de données en une seule opération. Dans la couche d’interface utilisateur, nous créons un GridView amélioré créé dans un didacticiel précédent. Dans la couche d’accès aux données, nous encapsulons les multiples opérations de suppression dans une transaction pour vous assurer que toutes les suppressions ont été effectuées correctement ou que toutes les suppressions sont annulées.

## <a name="introduction"></a>Introduction

Le [didacticiel précédent](batch-updating-vb.md) a abordé la création d’une interface de modification par lot à l’aide d’un GridView entièrement modifiable. Dans les situations où les utilisateurs modifient souvent un grand nombre d’enregistrements à la fois, une interface de modification par lot nécessite beaucoup moins de publications et de commutateurs de contexte clavier à souris, ce qui améliore l’efficacité des utilisateurs finaux. Cette technique est également utile pour les pages où les utilisateurs peuvent supprimer de nombreux enregistrements en une seule fois.

Toute personne qui a utilisé un client de messagerie en ligne est déjà familiarisé avec l’une des interfaces de suppression de lot les plus courantes : une case à cocher dans chaque ligne d’une grille avec un bouton supprimer tous les éléments sélectionnés correspondant (voir la figure 1). Ce didacticiel est plutôt concis, car nous avons déjà fait tout le travail des didacticiels précédents pour créer l’interface Web et une méthode pour supprimer une série d’enregistrements en une seule opération atomique. Dans le didacticiel [Ajout d’une colonne GridView de cases à cocher](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md) , nous avons créé un GridView avec une colonne de cases à cocher et dans le didacticiel [modifications de la base de données dans un](wrapping-database-modifications-within-a-transaction-vb.md) didacticiel de transaction, nous avons créé une méthode dans la couche BLL qui utiliserait une transaction pour supprimer une `List<T>` de valeurs `ProductID`. Dans ce didacticiel, nous allons créer et fusionner nos expériences précédentes pour créer un exemple de suppression de lot fonctionnel.

[![chaque ligne contient une case à cocher](batch-deleting-vb/_static/image1.gif)](batch-deleting-vb/_static/image1.png)

**Figure 1**: chaque ligne contient une case à cocher ([cliquez pour afficher l’image en taille réelle](batch-deleting-vb/_static/image2.png))

## <a name="step-1-creating-the-batch-deleting-interface"></a>Étape 1 : création de l’interface de suppression d’un lot

Étant donné que nous avons déjà créé l’interface de suppression de lots dans le didacticiel [Ajout d’une colonne GridView de cases à cocher](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md) , nous pouvons simplement la copier dans `BatchDelete.aspx` plutôt que de la créer à partir de zéro. Commencez par ouvrir la page `BatchDelete.aspx` dans le dossier `BatchData` et la page `CheckBoxField.aspx` dans le dossier `EnhancedGridView`. Dans la page `CheckBoxField.aspx`, accédez à la vue source et copiez le balisage entre les balises `<asp:Content>`, comme illustré à la figure 2.

[![copier la balise déclarative de CheckBoxField. aspx dans le presse-papiers](batch-deleting-vb/_static/image2.gif)](batch-deleting-vb/_static/image3.png)

**Figure 2**: copier la balise déclarative de `CheckBoxField.aspx` dans le presse-papiers ([cliquez pour afficher l’image en taille réelle](batch-deleting-vb/_static/image4.png))

Ensuite, accédez à la vue source dans `BatchDelete.aspx` et collez le contenu du presse-papiers dans les balises `<asp:Content>`. Vous pouvez également copier et coller le code à partir de la classe code-behind dans `CheckBoxField.aspx.vb` dans la classe code-behind de `BatchDelete.aspx.vb` (le bouton de `DeleteSelectedProducts` `Click` gestionnaire d’événements, la méthode `ToggleCheckState` et les gestionnaires d’événements `Click` pour les boutons `CheckAll` et `UncheckAll`). Après avoir copié ce contenu, la classe code-behind de la page s de `BatchDelete.aspx` doit contenir le code suivant :

[!code-vb[Main](batch-deleting-vb/samples/sample1.vb)]

Après avoir copié sur le balisage déclaratif et le code source, prenez un moment pour tester `BatchDelete.aspx` en l’affichant dans un navigateur. Vous devez voir un GridView qui répertorie les dix premiers produits dans un GridView avec chaque ligne qui répertorie le nom, la catégorie et le prix du produit, ainsi qu’une case à cocher. Il doit y avoir trois boutons : tout sélectionner, désélectionner tout et supprimer les produits sélectionnés. Le fait de cliquer sur le bouton Sélectionner tout sélectionne toutes les cases à cocher et décocher toutes les cases. Le fait de cliquer sur supprimer les produits sélectionnés affiche un message qui répertorie les valeurs de `ProductID` des produits sélectionnés, mais ne supprime pas réellement les produits.

[![l’interface de CheckBoxField. aspx a été déplacée vers BatchDeleting. aspx](batch-deleting-vb/_static/image3.gif)](batch-deleting-vb/_static/image5.png)

**Figure 3**: l’Interface de `CheckBoxField.aspx` a été déplacée vers `BatchDeleting.aspx` ([cliquez pour afficher l’image en taille réelle](batch-deleting-vb/_static/image6.png))

## <a name="step-2-deleting-the-checked-products-using-transactions"></a>Étape 2 : suppression des produits vérifiés à l’aide de transactions

Avec l’interface de suppression par lot correctement copiée sur `BatchDeleting.aspx`, il ne reste plus qu’à mettre à jour le code afin que le bouton supprimer les produits sélectionnés supprime les produits vérifiés à l’aide de la méthode `DeleteProductsWithTransaction` de la classe `ProductsBLL`. Cette méthode, ajoutée dans les [modifications d’encapsulation de la base de données dans un](wrapping-database-modifications-within-a-transaction-vb.md) didacticiel de transaction, accepte comme entrée une `List(Of T)` de `ProductID` valeurs et supprime chaque `ProductID` correspondante dans l’étendue d’une transaction.

Le gestionnaire d’événements `DeleteSelectedProducts` Button s `Click` utilise actuellement la boucle `For Each` suivante pour itérer au sein de chaque ligne GridView :

[!code-vb[Main](batch-deleting-vb/samples/sample2.vb)]

Pour chaque ligne, le contrôle Web case à cocher `ProductSelector` est référencé par programmation. Si elle est activée, les `ProductID` de ligne sont récupérées à partir de la collection de `DataKeys` et la propriété `DeleteResults` de l’étiquette s `Text` est mise à jour pour inclure un message indiquant que la ligne a été sélectionnée pour être supprimée.

Le code ci-dessus ne supprime pas réellement les enregistrements, car l’appel à la méthode `ProductsBLL` classe s `Delete` est commenté. Si cette logique de suppression devait être appliquée, le code supprimerait les produits, mais pas au sein d’une opération atomique. Autrement dit, si les premières suppressions de la séquence ont réussi, mais qu’une autre a échoué (peut-être en raison d’une violation de contrainte de clé étrangère), une exception est levée, mais les produits déjà supprimés restent supprimés.

Pour garantir l’atomicité, nous devons à la place utiliser la méthode `ProductsBLL` classe s `DeleteProductsWithTransaction`. Étant donné que cette méthode accepte une liste de valeurs `ProductID`, nous devons tout d’abord compiler cette liste à partir de la grille, puis la passer en tant que paramètre. Nous créons d’abord une instance d’une `List(Of T)` de type `Integer`. Dans l' `For Each` boucle, nous devons ajouter les produits sélectionnés `ProductID` valeurs à cette `List(Of T)`. Après la boucle, ce `List(Of T)` doit être passé à la méthode `DeleteProductsWithTransaction` de la classe `ProductsBLL`. Mettez à jour le gestionnaire d’événements du bouton `DeleteSelectedProducts` `Click` à l’aide du code suivant :

[!code-vb[Main](batch-deleting-vb/samples/sample3.vb)]

Le code mis à jour crée une `List(Of T)` de type `Integer` (`productIDsToDelete`) et le remplit avec les valeurs `ProductID` à supprimer. Après la `For Each` boucle, si au moins un produit est sélectionné, la méthode `ProductsBLL` classe s `DeleteProductsWithTransaction` est appelée et reçoit cette liste. L’étiquette `DeleteResults` est également affichée et les données sont reliées au GridView (afin que les enregistrements récemment supprimés n’apparaissent plus sous forme de lignes dans la grille).

La figure 4 montre le GridView après qu’un certain nombre de lignes ont été sélectionnées pour la suppression. La figure 5 montre l’écran immédiatement après avoir cliqué sur le bouton supprimer les produits sélectionnés. Notez que dans la figure 5, les valeurs `ProductID` des enregistrements supprimés s’affichent dans l’étiquette sous le contrôle GridView et ces lignes ne sont plus dans le GridView.

[![les produits sélectionnés seront supprimés](batch-deleting-vb/_static/image4.gif)](batch-deleting-vb/_static/image7.png)

**Figure 4**: les produits sélectionnés seront supprimés ([cliquez pour afficher l’image en taille réelle](batch-deleting-vb/_static/image8.png))

[![les valeurs ProductID des produits supprimés sont répertoriées sous le contrôle GridView](batch-deleting-vb/_static/image5.gif)](batch-deleting-vb/_static/image9.png)

**Figure 5**: les valeurs des produits `ProductID` supprimés sont répertoriées sous le contrôle GridView ([cliquez pour afficher l’image en taille réelle](batch-deleting-vb/_static/image10.png))

> [!NOTE]
> Pour tester l’atomicité de la méthode `DeleteProductsWithTransaction`, ajoutez manuellement une entrée pour un produit dans la table `Order Details`, puis essayez de supprimer ce produit (ainsi que d’autres). Vous recevrez une violation de contrainte de clé étrangère lors de la tentative de suppression du produit avec une commande associée, mais notez comment les autres suppressions de produits sélectionnés sont annulées.

## <a name="summary"></a>Récapitulatif

La création d’une interface de suppression de lot implique l’ajout d’un GridView avec une colonne de cases à cocher et un contrôle Web bouton qui, lorsque vous cliquez dessus, supprime toutes les lignes sélectionnées comme une seule opération atomique. Dans ce didacticiel, nous avons créé une telle interface par accolant ensemble du travail effectué dans deux didacticiels précédents, en [ajoutant une colonne GridView de cases à cocher](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md) et en [encapsulant les modifications apportées à la base de données dans une transaction](wrapping-database-modifications-within-a-transaction-vb.md). Dans le premier didacticiel, nous avons créé un GridView avec une colonne de cases à cocher et, dans ce dernier, nous avons implémenté une méthode dans la couche BLL qui, lorsqu’elle a passé une `List(Of T)` de valeurs `ProductID`, les a supprimées dans l’étendue d’une transaction.

Dans le didacticiel suivant, nous allons créer une interface pour effectuer des insertions par lot.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel étaient Hilton Giesenow et Teresa Murphy. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](batch-updating-vb.md)
> [Suivant](batch-inserting-vb.md)
