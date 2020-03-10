---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-vb
title: Représentation de plusieurs enregistrements par ligne avec le contrôle DataList (VB) | Microsoft Docs
author: rick-anderson
description: Dans ce bref didacticiel, nous allons découvrir comment personnaliser la disposition de DataList par le biais de ses propriétés RepeatColumns et RepeatDirection.
ms.author: riande
ms.date: 09/13/2006
ms.assetid: f555c531-bf33-4699-9987-42dbfef23c1f
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 17283dae192896fbaa48f1d7fe49afdbaf4c9a02
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78611341"
---
# <a name="showing-multiple-records-per-row-with-the-datalist-control-vb"></a>Affichage de plusieurs enregistrements par ligne avec le contrôle DataList (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_31_VB.exe) ou [Télécharger le PDF](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/datatutorial31vb1.pdf)

> Dans ce bref didacticiel, nous allons découvrir comment personnaliser la disposition de DataList par le biais de ses propriétés RepeatColumns et RepeatDirection.

## <a name="introduction"></a>Introduction

Les exemples DataList que nous avons vus dans les deux derniers didacticiels ont rendu chaque enregistrement de sa source de données sous la forme d’une ligne dans un `<table>`HTML à une seule colonne. Bien qu’il s’agit du comportement DataList par défaut, il est très facile de personnaliser l’affichage de DataList de sorte que les éléments de source de données soient répartis dans une table à plusieurs colonnes et plusieurs lignes. En outre, il est possible d’afficher tous les éléments de source de données dans un contrôle DataList à une seule ligne et plusieurs colonnes.

Nous pouvons personnaliser la disposition de DataList par le biais de ses propriétés `RepeatColumns` et `RepeatDirection`, qui indiquent respectivement le nombre de colonnes rendues et si ces éléments sont disposés verticalement ou horizontalement. La figure 1, par exemple, montre un contrôle DataList qui affiche des informations sur les produits dans une table avec trois colonnes.

[![la DataList affiche trois produits par ligne](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image2.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image1.png)

**Figure 1**: DataList affiche trois produits par ligne ([cliquez pour afficher l’image en taille réelle](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image3.png))

En présentant plusieurs éléments de source de données par ligne, la DataList peut utiliser plus efficacement l’espace d’écran horizontal. Dans ce bref didacticiel, nous allons explorer ces deux propriétés DataList.

## <a name="step-1-displaying-product-information-in-a-datalist"></a>Étape 1 : affichage des informations sur le produit dans un contrôle DataList

Avant d’examiner les propriétés `RepeatColumns` et `RepeatDirection`, nous allons d’abord créer un contrôle DataList sur notre page qui répertorie les informations sur les produits à l’aide de la disposition de table à une seule colonne et à plusieurs lignes. Pour cet exemple, Let affiche le nom, la catégorie et le prix du produit à l’aide du balisage suivant :

[!code-html[Main](showing-multiple-records-per-row-with-the-datalist-control-vb/samples/sample1.html)]

Nous avons vu comment lier des données à un contrôle DataList dans les exemples précédents. je vais donc passer rapidement en revue ces étapes. Commencez par ouvrir la page `RepeatColumnAndDirection.aspx` dans le dossier `DataListRepeaterBasics` et faites glisser un contrôle DataList de la boîte à outils vers le concepteur. À partir de la balise active de DataList s, choisissez de créer un ObjectDataSource et configurez-le pour extraire ses données de la méthode `ProductsBLL` classe s `GetProducts`, en choisissant l’option (aucune) dans les onglets insertion, mise à jour et suppression de l’Assistant.

Une fois que vous avez créé et lié le nouvel ObjectDataSource au contrôle DataList, Visual Studio crée automatiquement un `ItemTemplate` qui affiche le nom et la valeur de chacun des champs de données de produit. Ajustez la `ItemTemplate` soit directement par le biais du balisage déclaratif, soit à partir de l’option modifier les modèles dans la balise active de DataList s afin qu’elle utilise le balisage indiqué ci-dessus, en remplaçant le *nom du produit*, le nom de la *catégorie*et le texte du *prix* par des contrôles Label qui utilisent la syntaxe DataBinding appropriée pour affecter des valeurs à leurs propriétés `Text`. Après la mise à jour de la `ItemTemplate`, le balisage déclaratif de votre page doit ressembler à ce qui suit :

[!code-aspx[Main](showing-multiple-records-per-row-with-the-datalist-control-vb/samples/sample2.aspx)]

Notez que j’ai inclus un spécificateur de format dans la syntaxe de liaison de données `Eval` pour le `UnitPrice`, en mettant en forme la valeur retournée en tant que Currency-`Eval("UnitPrice", "{0:C}").`

Prenez un moment pour accéder à votre page dans un navigateur. Comme le montre la figure 2, le contrôle DataList est rendu sous la forme d’une table de produits à une seule colonne, à plusieurs lignes.

[![par défaut, le contrôle DataList est restitué sous la forme d’une table à une seule colonne, à plusieurs lignes](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image5.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image4.png)

**Figure 2**: par défaut, le contrôle DataList est restitué sous la forme d’une table à une seule colonne, à plusieurs lignes ([cliquez pour afficher l’image en taille réelle](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image6.png))

## <a name="step-2-changing-the-datalist-s-layout-direction"></a>Étape 2 : modification du sens de la disposition de DataList s

Bien que le comportement par défaut pour le contrôle DataList consiste à disposer ses éléments verticalement dans une table à une seule colonne et à plusieurs lignes, ce comportement peut facilement être modifié via la propriété DataList s [`RepeatDirection`](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatdirection.aspx). La propriété `RepeatDirection` peut accepter l’une des deux valeurs possibles : `Horizontal` ou `Vertical` (valeur par défaut).

En remplaçant la propriété `RepeatDirection` `Vertical` par `Horizontal`, la DataList affiche ses enregistrements sur une seule ligne, en créant une colonne par élément de source de données. Pour illustrer cet effet, cliquez sur le contrôle DataList dans le concepteur, puis, dans le Fenêtre Propriétés, remplacez la propriété `RepeatDirection` de `Vertical` par `Horizontal`. Dans ce cas, le concepteur ajuste la disposition de DataList, en créant une interface à une seule ligne, à plusieurs colonnes (voir figure 3).

[![la propriété RepeatDirection détermine la disposition des éléments de DataList](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image8.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image7.png)

**Figure 3**: la propriété `RepeatDirection` détermine la disposition des éléments de DataList ([cliquez pour afficher l’image en taille réelle](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image9.png))

Lors de l’affichage de petites quantités de données, une table à une seule ligne et plusieurs colonnes peut être un moyen idéal de maximiser l’espace de l’écran. Toutefois, pour les volumes de données plus importants, une seule ligne nécessitera de nombreuses colonnes, qui pousse les éléments qui peuvent s’ajuster à l’écran à droite. La figure 4 montre les produits lorsqu’ils sont rendus dans un contrôle DataList à une seule ligne. Étant donné qu’il y a de nombreux produits (plus de 80), l’utilisateur doit faire défiler vers la droite pour afficher des informations sur chacun des produits.

[![pour les sources de données suffisamment volumineuses, une seule colonne DataList requiert un défilement horizontal](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image11.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image10.png)

**Figure 4**: pour les sources de données suffisamment volumineuses, une seule colonne DataList requiert un défilement horizontal ([cliquez pour afficher l’image en taille réelle](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image12.png))

## <a name="step-3-displaying-data-in-a-multi-column-multi-row-table"></a>Étape 3 : affichage des données dans une table à plusieurs colonnes et plusieurs lignes

Pour créer un contrôle DataList multiligne à plusieurs colonnes, nous devons définir la [propriété`RepeatColumns`](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatcolumns.aspx) sur le nombre de colonnes à afficher. Par défaut, la propriété `RepeatColumns` a la valeur 0, ce qui permet à la DataList d’afficher tous ses éléments dans une seule ligne ou colonne (en fonction de la valeur de la propriété `RepeatDirection`).

Pour notre exemple, nous allons afficher trois produits par ligne de table. Par conséquent, affectez à la propriété `RepeatColumns` la valeur 3. Après avoir apporté cette modification, prenez un moment pour afficher les résultats dans un navigateur. Comme le montre la figure 5, les produits sont désormais répertoriés dans une table à trois lignes et plusieurs lignes.

[![trois produits sont affichés par ligne](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image14.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image13.png)

**Figure 5**: trois produits sont affichés par ligne ([cliquez pour afficher l’image en taille réelle](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image15.png))

La propriété `RepeatDirection` affecte la disposition des éléments du contrôle DataList. La figure 5 montre les résultats avec la propriété `RepeatDirection` définie sur `Horizontal`. Notez que les trois premiers produits Chai, Chang et sirops sont disposés de gauche à droite, de haut en bas. Les trois produits suivants (à partir de chef Anton s Cajun assaisonnement) apparaissent dans une ligne au-dessous des trois premiers. Toutefois, si vous redéfinissez la propriété `RepeatDirection` sur `Vertical`, ces produits se présentent de haut en bas, de gauche à droite, comme le montre la figure 6.

[![ici, les produits sont disposés verticalement](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image17.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image16.png)

**Figure 6**: les produits sont disposés verticalement ([cliquez pour afficher l’image en taille réelle](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image18.png))

Le nombre de lignes affichées dans la table résultante dépend du nombre total d’enregistrements liés au contrôle DataList. Précisément, il s’agit du plafond du nombre total d’éléments de source de données divisés par la valeur de propriété `RepeatColumns`. Étant donné que la table `Products` contient actuellement 84 produits, ce qui est divisible par 3, il y a 28 lignes. Si le nombre d’éléments dans la source de données et la valeur de la propriété `RepeatColumns` ne sont pas divisibles, la dernière ligne ou colonne aura des cellules vides. Si la `RepeatDirection` est définie sur `Vertical`, alors la dernière colonne aura des cellules vides ; Si `RepeatDirection` est `Horizontal`, la dernière ligne aura les cellules vides.

## <a name="summary"></a>Récapitulatif

Par défaut, le contrôle DataList répertorie ses éléments dans une table à une seule colonne, à plusieurs lignes, qui imite la disposition d’un GridView avec un TemplateField unique. Bien que cette disposition par défaut soit acceptable, nous pouvons maximiser l’espace de l’écran en affichant plusieurs éléments de source de données par ligne. Pour ce faire, il suffit de définir la propriété DataList s `RepeatColumns` sur le nombre de colonnes à afficher par ligne. En outre, la propriété DataList s `RepeatDirection` peut être utilisée pour indiquer si le contenu de la table à plusieurs colonnes, à plusieurs lignes, doit être disposé horizontalement de gauche à droite, de haut en bas ou verticalement de haut en bas, de gauche à droite.

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel était John SURU. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](formatting-the-datalist-and-repeater-based-upon-data-vb.md)
> [Suivant](nested-data-web-controls-vb.md)
