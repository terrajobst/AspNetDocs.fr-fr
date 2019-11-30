---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
title: Filtrage maître/détail avec un contrôle DropDownListC#() | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous voyons comment afficher des rapports maître/détail dans une page Web unique à l’aide de DropDownList pour afficher les enregistrements « maîtres » et un contrôle DataList à disp...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: 07fa47ae-e491-4a2f-b265-d342b9ddef46
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: 8289f46fd6d0143802269d5c6196a4c40db9378c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74631090"
---
# <a name="masterdetail-filtering-with-a-dropdownlist-c"></a>Filtrage maître/détail avec une DropDownList (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_33_CS.exe) ou [Télécharger le PDF](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/datatutorial33cs1.pdf)

> Dans ce didacticiel, nous voyons comment afficher des rapports maître/détail dans une page Web unique à l’aide de DropDownList pour afficher les enregistrements « maîtres » et un contrôle DataList pour afficher les « détails ».

## <a name="introduction"></a>Introduction

Le rapport maître/détail, que nous avons créé pour la première fois à l’aide d’un GridView dans le [filtrage maître/détail précédent avec un didacticiel DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) , commence par montrer un ensemble d’enregistrements « maîtres ». L’utilisateur peut ensuite accéder à l’un des enregistrements principaux, ce qui affiche les détails de l’enregistrement maître. Les rapports maître/détail constituent un choix idéal pour visualiser les relations un-à-plusieurs et pour afficher des informations détaillées à partir de tables « larges » plus particulièrement (celles qui ont beaucoup de colonnes). Nous avons exploré comment implémenter des rapports maître/détail à l’aide des contrôles GridView et DetailsView dans les didacticiels précédents. Dans ce didacticiel et les deux suivants, nous allons réexaminer ces concepts, mais nous nous concentrons sur l’utilisation des contrôles DataList et Repeater à la place.

Dans ce didacticiel, nous allons examiner l’utilisation d’un contrôle DropDownList pour contenir les enregistrements « principaux », avec les enregistrements « détails » affichés dans un contrôle DataList.

## <a name="step-1-adding-the-masterdetail-tutorial-web-pages"></a>Étape 1 : ajout des pages Web du didacticiel maître/détail

Avant de commencer ce didacticiel, commençons par prendre un moment pour ajouter le dossier et les pages ASP.NET dont nous aurons besoin pour ce didacticiel et les deux suivantes traitant des rapports maître/détail à l’aide des contrôles DataList et Repeater. Commencez par créer un nouveau dossier dans le projet nommé `DataListRepeaterFiltering`. Ensuite, ajoutez les cinq pages ASP.NET suivantes à ce dossier, en faisant en sorte qu’elles soient toutes configurées pour utiliser la page maître `Site.master`:

- `Default.aspx`
- `FilterByDropDownList.aspx`
- `CategoryListMaster.aspx`
- `ProductsForCategoryDetails.aspx`
- `CategoriesAndProducts.aspx`

![Créer un dossier DataListRepeaterFiltering et ajouter les pages du didacticiel ASP.NET](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image1.png)

**Figure 1**: créer un dossier `DataListRepeaterFiltering` et ajouter les pages du didacticiel ASP.net

Ensuite, ouvrez la page `Default.aspx`, puis faites glisser le contrôle utilisateur `SectionLevelTutorialListing.ascx` à partir du dossier `UserControls` sur l’aire de conception. Ce contrôle utilisateur, que nous avons créé dans le didacticiel sur les [pages maîtres et la navigation](../introduction/master-pages-and-site-navigation-cs.md) dans les sites, énumère le plan du site et affiche les didacticiels de la section actuelle dans une liste à puces.

[![ajouter le contrôle utilisateur SectionLevelTutorialListing. ascx à default. aspx](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image3.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image2.png)

**Figure 2**: ajouter le contrôle utilisateur `SectionLevelTutorialListing.ascx` à `Default.aspx` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image4.png))

Pour que la liste à puces affiche les didacticiels maître/détail que nous allons créer, vous devez les ajouter au plan du site. Ouvrez le fichier `Web.sitemap` et ajoutez le balisage suivant après le balisage de nœud de plan de site « affichage des données avec le contrôle DataList et Repeater » :

[!code-xml[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample1.xml)]

![Mettre à jour le plan du site pour inclure les nouvelles pages ASP.NET](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image5.png)

**Figure 3**: mettre à jour le plan du site pour inclure les nouvelles pages ASP.net

## <a name="step-2-displaying-the-categories-in-a-dropdownlist"></a>Étape 2 : affichage des catégories dans un DropDownList

Notre rapport maître/détail répertorie les catégories dans un contrôle DropDownList, avec les produits de l’élément de liste sélectionnés affichés plus en détail dans la page d’un contrôle DataList. La première tâche devant nous, est alors de faire en sorte que les catégories s’affichent dans un DropDownList. Commencez par ouvrir la page `FilterByDropDownList.aspx` dans le dossier `DataListRepeaterFiltering` et faites glisser un contrôle DropDownList de la boîte à outils vers le concepteur de la page. Ensuite, définissez la propriété `ID` de DropDownList sur `Categories`. Cliquez sur le lien choisir la source de données à partir de la balise active de DropDownList, puis créez un ObjectDataSource nommé `CategoriesDataSource`.

[![ajouter un nouvel ObjectDataSource nommé CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image7.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image6.png)

**Figure 4**: ajouter un nouvel ObjectDataSource nommé `CategoriesDataSource` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image8.png))

Configurez le nouvel ObjectDataSource de telle sorte qu’il appelle la méthode `GetCategories()` de la classe `CategoriesBLL`. Après avoir configuré l’ObjectDataSource, nous devons toujours spécifier le champ de source de données à afficher dans le DropDownList et celui qui doit être associé comme valeur pour chaque élément de liste. Utilisez le champ `CategoryName` comme affichage et `CategoryID` comme valeur pour chaque élément de liste.

[![que le DropDownList affiche le champ CategoryName et utilise CategoryID comme valeur](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image10.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image9.png)

**Figure 5**: afficher le champ `CategoryName` dans le champ et utiliser `CategoryID` comme valeur ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image11.png))

À ce stade, nous disposons d’un contrôle DropDownList qui est rempli avec les enregistrements de la table `Categories` (tous sont exécutés en environ six secondes). La figure 6 montre notre progression jusqu’à présent dans un navigateur.

[![une liste déroulante répertorie les catégories actuelles](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image13.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image12.png)

**Figure 6**: une liste déroulante répertorie les catégories actuelles ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image14.png))

## <a name="step-2-adding-the-products-datalist"></a>Étape 2 : ajout de la DataList Products

La dernière étape de notre rapport maître/détail consiste à répertorier les produits associés à la catégorie sélectionnée. Pour ce faire, ajoutez un contrôle DataList à la page et créez un nouvel ObjectDataSource nommé `ProductsByCategoryDataSource`. Faire en sorte que le contrôle `ProductsByCategoryDataSource` récupère ses données à partir de la méthode `GetProductsByCategoryID(categoryID)` de la classe `ProductsBLL`. Étant donné que ce rapport maître/détail est en lecture seule, choisissez l’option (aucune) dans les onglets insérer, mettre à jour et supprimer.

[![sélectionnez la méthode GetProductsByCategoryID (categoryID)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image16.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image15.png)

**Figure 7**: sélectionner la méthode `GetProductsByCategoryID(categoryID)` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image17.png))

Après avoir cliqué sur suivant, l’Assistant ObjectDataSource nous invite à entrer la source de la valeur du paramètre *`categoryID`* de la méthode `GetProductsByCategoryID(categoryID)`. Pour utiliser la valeur de l’élément `categories` DropDownList sélectionné, définissez la source de paramètre sur Control et ControlID sur `Categories`.

[![affectez à la valeur du paramètre categoryID la valeur des catégories DropDownList](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image19.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image18.png)

**Figure 8**: définir le paramètre *`categoryID`* sur la valeur de la `Categories` DropDownList ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image20.png))

À la fin de l’Assistant Configuration de la source de données, Visual Studio génère automatiquement une `ItemTemplate` pour la DataList qui affiche le nom et la valeur de chaque champ de données. Nous allons améliorer la DataList pour qu’elle utilise à la place un `ItemTemplate` qui affiche uniquement le nom, la catégorie, le fournisseur, la quantité par unité et le prix du produit, ainsi qu’un `SeparatorTemplate` qui injecte un élément `<hr>` entre chaque élément. Je vais utiliser le `ItemTemplate` à partir d’un exemple dans le didacticiel [affichage des données avec les contrôles DataList et Repeater](../displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs.md) , mais n’hésitez pas à utiliser le balisage de modèle que vous trouvez le plus visuel.

Après avoir apporté ces modifications, votre contrôle DataList et le balisage de l’ObjectDataSource doivent ressembler à ce qui suit :

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample2.aspx)]

Prenez un moment pour consulter notre progression dans un navigateur. Lors de la première visite de la page, les produits appartenant à la catégorie sélectionnée (boissons) sont affichés (comme illustré à la figure 9), mais la modification de la classe DropDownList ne met pas à jour les données. Cela est dû au fait qu’une publication doit se produire pour que le contrôle DataList soit mis à jour. Pour ce faire, nous pouvons définir la propriété de `AutoPostBack` de la propriété DropDownList sur `true` ou ajouter un contrôle Web Button à la page. Pour ce didacticiel, j’ai choisi de définir la propriété de `AutoPostBack` de la propriété DropDownList sur `true`.

Les figures 9 et 10 illustrent le rapport maître/détail en action.

[![lors de la première visite de la page, les produits de boisson s’affichent](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image22.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image21.png)

**Figure 9**: lors de la première visite de la page, les produits de la boisson s’affichent ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image23.png))

[![la sélection d’un nouveau produit (produit) entraîne automatiquement une publication (PostBack), la mise à jour du contrôle DataList](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image25.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image24.png)

**Figure 10**: la sélection d’un nouveau produit (produit) entraîne automatiquement une publication (postback), en mettant à jour la DataList ([cliquez pour afficher l’image en plein écran](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image26.png))

## <a name="adding-a----choose-a-category----list-item"></a>Ajout d’une « --choisir une catégorie-- » élément de liste

Lorsque vous accédez pour la première fois à la page de `FilterByDropDownList.aspx`, le premier élément de liste (boissons) des catégories DropDownList est sélectionné par défaut, indiquant les produits de boisson dans le contrôle DataList. Dans le didacticiel *maître/détail avec un didacticiel DropDownList* , nous avons ajouté une option « --choisir une catégorie-- » au contrôle DropDownList qui a été sélectionné par défaut et, lorsque vous sélectionnez cette option, *tous* les produits de la base de données sont affichés. Une telle approche était gérable lors de la liste des produits dans un GridView, car chaque ligne de produit a pris une petite quantité d’écran. Avec la DataList, toutefois, les informations de chaque produit consomment un plus grand bloc de l’écran. Nous allons toujours ajouter une option « --choisir une catégorie-- » et l’avoir sélectionnée par défaut, mais au lieu d’afficher tous les produits quand elle est sélectionnée, nous allons la configurer pour qu’elle n’affiche aucun produit.

Pour ajouter un nouvel élément de liste au DropDownList, accédez à la Fenêtre Propriétés et cliquez sur les ellipses dans la propriété `Items`. Ajoutez un nouvel élément de liste avec le `Text` « --choisissez une catégorie-- » et le `0``Value`.

![Ajouter une](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image27.png)

**Figure 11**: ajouter un « --choisir une catégorie-- » élément de liste

Vous pouvez également ajouter l’élément de liste en ajoutant le balisage suivant au DropDownList :

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample3.aspx)]

En outre, nous devons définir le `AppendDataBoundItems` du contrôle DropDownList sur `true`, car s’il est défini sur `false` (valeur par défaut), lorsque les catégories sont liées au DropDownList de l’ObjectDataSource, elles remplacent tous les éléments de liste ajoutés manuellement.

![Affectez la valeur true à la propriété AppendDataBoundItems](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image28.png)

**Figure 12**: définir la propriété `AppendDataBoundItems` sur true

La raison pour laquelle nous avons choisi la valeur `0` pour l’élément de liste « --choisir une catégorie-- » est parce qu’il n’y a aucune catégorie dans le système avec une valeur de `0`. par conséquent, aucun enregistrement de produit n’est renvoyé quand l’élément de liste « --choisir une catégorie-- » est sélectionné. Pour confirmer cela, prenez un moment pour accéder à la page via un navigateur. Comme le montre la figure 13, lors de l’affichage initial de la page, l’élément de liste « --choisir une catégorie-- » est sélectionné et aucun produit n’est affiché.

[![lorsque le](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image30.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image29.png)

**Figure 13**: quand l’élément de liste « --choisir une catégorie-- » est sélectionné, aucun produit n’est affiché ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image31.png))

Si vous préférez afficher *tous* les produits lorsque l’option « --choisir une catégorie-- » est sélectionnée, utilisez la valeur `-1` à la place. Le lecteur astucieux rappelle le *filtrage maître/détail à l’aide d’un didacticiel DropDownList* . nous avons mis à jour la méthode `GetProductsByCategoryID(categoryID)` de la classe `ProductsBLL` afin que, si une *`categoryID`* valeur de `-1` ait été passée, tous les enregistrements de produit ont été retournés.

## <a name="summary"></a>Récapitulatif

Lors de l’affichage de données hiérarchiques, il est souvent utile de présenter les données à l’aide de rapports maître/détail, à partir desquels l’utilisateur peut commencer à utiliser les données du haut de la hiérarchie et descendre en détail dans les détails. Dans ce didacticiel, nous avons examiné la création d’un rapport maître/détail simple indiquant les produits d’une catégorie sélectionnée. Pour ce faire, utilisez un contrôle DropDownList pour la liste des catégories et un contrôle DataList pour les produits appartenant à la catégorie sélectionnée.

Dans le didacticiel suivant, nous allons examiner la séparation des enregistrements maître et détails sur deux pages. Dans la première page, une liste d’enregistrements « maîtres » s’affiche, avec un lien pour afficher les détails. Si vous cliquez sur le lien, l’utilisateur verra la deuxième page qui affichera les détails de l’enregistrement maître sélectionné.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à...

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel était Randy Schmidt. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Suivant](master-detail-filtering-acess-two-pages-datalist-cs.md)
