---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-vb
title: Filtrage maître/détail avec un DropDownList (VB) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons voir comment afficher les enregistrements maîtres dans un contrôle DropDownList et les détails de l’élément de liste sélectionné dans un GridView.
ms.author: riande
ms.date: 03/31/2010
ms.assetid: ea44717e-ab2e-46cd-a692-e4a9c0de194c
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 62cd296a3f36e1779666a6b5db15b0ce2488d0e4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78528720"
---
# <a name="masterdetail-filtering-with-a-dropdownlist-vb"></a>Filtrage maître/détail avec une DropDownList (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_7_VB.exe) ou [Télécharger le PDF](master-detail-filtering-with-a-dropdownlist-vb/_static/datatutorial07vb1.pdf)

> Dans ce didacticiel, nous allons voir comment afficher les enregistrements maîtres dans un contrôle DropDownList et les détails de l’élément de liste sélectionné dans un GridView.

## <a name="introduction"></a>Présentation

Un type de rapport commun est le *rapport maître/détail*dans lequel le rapport commence en présentant un ensemble d’enregistrements « maîtres ». L’utilisateur peut ensuite accéder à l’un des enregistrements principaux, ce qui affiche les détails de l’enregistrement maître. Les rapports maître/détail constituent un choix idéal pour visualiser les relations un-à-plusieurs, comme un rapport affichant toutes les catégories, puis permettant à un utilisateur de sélectionner une catégorie particulière et d’afficher ses produits associés. En outre, les rapports maître/détail sont utiles pour afficher des informations détaillées à partir de tables « larges » (celles qui comportent un grand nombre de colonnes). Par exemple, le niveau « maître » d’un rapport maître/détail peut afficher uniquement le nom de produit et le prix unitaire des produits dans la base de données, et l’exploration d’un produit particulier affiche les champs de produit supplémentaires (catégorie, fournisseur, quantité par unité, etc.).

Il existe de nombreuses façons d’implémenter un rapport maître/détail. Sur ce point et les trois didacticiels suivants, nous allons examiner un large éventail de rapports maître/détail. Dans ce didacticiel, nous allons voir comment afficher les enregistrements maîtres dans un [contrôle DropDownList](https://msdn.microsoft.com/library/dtx91y0z.aspx) et les détails de l’élément de liste sélectionné dans un GridView. En particulier, le rapport maître/détail de ce didacticiel répertorie les informations sur les catégories et les produits.

## <a name="step-1-displaying-the-categories-in-a-dropdownlist"></a>Étape 1 : Affichage des catégories dans un DropDownList

Notre rapport maître/détail répertorie les catégories dans un contrôle DropDownList, avec les produits de l’élément de liste sélectionnés affichés plus en détail dans la page d’un GridView. La première tâche devant nous, est alors de faire en sorte que les catégories s’affichent dans un DropDownList. Ouvrez la page `FilterByDropDownList.aspx` dans le dossier `Filtering`, faites glisser un contrôle DropDownList de la boîte à outils vers le concepteur de la page, puis définissez sa propriété `ID` sur `Categories`. Ensuite, cliquez sur le lien choisir la source de données à partir de la balise active de DropDownList. L’Assistant Configuration de source de données s’affiche.

[![spécifier la source de données de DropDownList](master-detail-filtering-with-a-dropdownlist-vb/_static/image2.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image1.png)

**Figure 1**: Spécifier la source de données de DropDownList ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-vb/_static/image3.png))

Choisissez d’ajouter un nouvel ObjectDataSource nommé `CategoriesDataSource` qui appelle la méthode `GetCategories()` de la classe `CategoriesBLL`.

[![ajouter un nouvel ObjectDataSource nommé CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-vb/_static/image5.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image4.png)

**Figure 2**: Ajouter un nouvel ObjectDataSource nommé `CategoriesDataSource` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-vb/_static/image6.png))

[![choisir d’utiliser la classe CategoriesBLL](master-detail-filtering-with-a-dropdownlist-vb/_static/image8.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image7.png)

**Figure 3**: Choisissez d’utiliser la classe `CategoriesBLL` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-vb/_static/image9.png))

[![configurer ObjectDataSource pour utiliser la méthode GetCategories ()](master-detail-filtering-with-a-dropdownlist-vb/_static/image11.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image10.png)

**Figure 4**: Configurer ObjectDataSource pour utiliser la méthode `GetCategories()` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-vb/_static/image12.png))

Après avoir configuré l’ObjectDataSource, nous devons toujours spécifier le champ de source de données à afficher dans DropDownList et celui qui doit être associé comme valeur pour l’élément de liste. Utilisez le champ `CategoryName` comme affichage et `CategoryID` comme valeur pour chaque élément de liste.

[![que le DropDownList affiche le champ CategoryName et utilise CategoryID comme valeur](master-detail-filtering-with-a-dropdownlist-vb/_static/image14.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image13.png)

**Figure 5**: Si le DropDownList affiche le champ `CategoryName` et utilise `CategoryID` comme valeur ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-vb/_static/image15.png))

À ce stade, nous disposons d’un contrôle DropDownList qui est rempli avec les enregistrements de la table `Categories` (tous sont exécutés en environ six secondes). La figure 6 montre notre progression jusqu’à présent dans un navigateur.

[![une liste déroulante répertorie les catégories actuelles](master-detail-filtering-with-a-dropdownlist-vb/_static/image17.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image16.png)

**Figure 6**: Une liste déroulante répertorie les catégories actuelles ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-vb/_static/image18.png))

## <a name="step-2-adding-the-products-gridview"></a>Étape 2 : Ajout du GridView Products

Cette dernière étape de notre rapport maître/détail consiste à répertorier les produits associés à la catégorie sélectionnée. Pour ce faire, ajoutez un GridView à la page et créez un nouvel ObjectDataSource nommé `productsDataSource`. Faire en sorte que le contrôle `productsDataSource` élimine ses données de la méthode `GetProductsByCategoryID(categoryID)` de la classe `ProductsBLL`.

[![sélectionnez la méthode GetProductsByCategoryID (categoryID)](master-detail-filtering-with-a-dropdownlist-vb/_static/image20.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image19.png)

**Figure 7**: Sélectionnez la méthode `GetProductsByCategoryID(categoryID)` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-vb/_static/image21.png))

Après avoir choisi cette méthode, l’Assistant ObjectDataSource nous invite à entrer la valeur du paramètre *`categoryID`* de la méthode. Pour utiliser la valeur de l’élément `categories` DropDownList sélectionné, définissez la source de paramètre sur Control et ControlID sur `Categories`.

[![affectez à la valeur du paramètre categoryID la valeur des catégories DropDownList](master-detail-filtering-with-a-dropdownlist-vb/_static/image23.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image22.png)

**Figure 8**: Définissez le paramètre *`categoryID`* sur la valeur de la `Categories` DropDownList ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-vb/_static/image24.png))

Prenez un moment pour consulter notre progression dans un navigateur. Lors de la première visite de la page, ces produits appartiennent à la catégorie sélectionnée (boissons), comme illustré à la figure 9, mais la modification de la classe DropDownList ne met pas à jour les données. Cela est dû au fait qu’une publication doit se produire pour que le GridView soit mis à jour. Pour ce faire, nous avons deux options (ne nécessitant aucune écriture de code) :

- **Affectez à la propriété AutoPostBack**[de la catégorie DropDownList](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listcontrol.autopostback%28VS.80%29.aspx)**la valeur true.** (Pour ce faire, vous pouvez activer l’option Activer AutoPostBack dans la balise active de DropDownList.) Cette opération déclenche une publication (postback) chaque fois que l’élément sélectionné de la DropDownList est modifié par l’utilisateur. Par conséquent, lorsque l’utilisateur sélectionne une nouvelle catégorie à partir du contrôle DropDownList, une publication s’affichera et le GridView sera mis à jour avec les produits de la catégorie nouvellement sélectionnée. (Il s’agit de l’approche que j’ai utilisée dans ce didacticiel.)
- **Ajoutez un contrôle Web Button à côté de DropDownList.** Affectez à sa propriété `Text` la valeur Refresh ou un nom similaire. Avec cette approche, l’utilisateur doit sélectionner une nouvelle catégorie, puis cliquer sur le bouton. Le fait de cliquer sur le bouton entraîne une publication (postback) et met à jour le contrôle GridView pour répertorier les produits de la catégorie sélectionnée.

Les figures 9 et 10 illustrent le rapport maître/détail en action.

[![lors de la première visite de la page, les produits de boisson s’affichent](master-detail-filtering-with-a-dropdownlist-vb/_static/image26.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image25.png)

**Figure 9**: Lors de la première visite de la page, les produits de boisson s’affichent ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-vb/_static/image27.png))

[![la sélection d’un nouveau produit (produit) entraîne automatiquement une publication (PostBack), en mettant à jour le contrôle GridView](master-detail-filtering-with-a-dropdownlist-vb/_static/image29.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image28.png)

**Figure 10**: La sélection d’un nouveau produit (produit) entraîne automatiquement une publication (PostBack), ce qui a pour effet de mettre à jour le contrôle GridView ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-vb/_static/image30.png))

## <a name="adding-a----choose-a-category----list-item"></a>Ajout d’une « --choisir une catégorie-- » élément de liste

Lorsque vous accédez pour la première fois à la page de `FilterByDropDownList.aspx`, le premier élément de liste (boissons) des catégories DropDownList est sélectionné par défaut, ce qui indique les produits de boisson dans le GridView. Au lieu d’utiliser les produits de la première catégorie, nous pouvons souhaiter qu’un élément DropDownList soit sélectionné, à l’instar de « --choisir une catégorie-- ».

Pour ajouter un nouvel élément de liste au DropDownList, accédez à la Fenêtre Propriétés et cliquez sur les ellipses dans la propriété `Items`. Ajoutez un nouvel élément de liste avec le `Text` « --choisissez une catégorie-- » et le `-1``Value`.

[![ajouter un--choisir une catégorie--élément de liste](master-detail-filtering-with-a-dropdownlist-vb/_static/image32.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image31.png)

**Figure 11**: Ajouter un--choisir une catégorie--élément de liste ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-vb/_static/image33.png))

Vous pouvez également ajouter l’élément de liste en ajoutant le balisage suivant au DropDownList :

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-vb/samples/sample1.aspx)]

En outre, nous devons définir le `AppendDataBoundItems` du contrôle DropDownList sur true car, lorsque les catégories sont liées à DropDownList de l’ObjectDataSource, elles remplacent tous les éléments de liste ajoutés manuellement si `AppendDataBoundItems` n’est pas true.

![Affectez la valeur true à la propriété AppendDataBoundItems](master-detail-filtering-with-a-dropdownlist-vb/_static/image34.png)

**Figure 12**: Affectez la valeur true à la propriété `AppendDataBoundItems`

Après ces modifications, lors de la première visite de la page, l’option « --choisir une catégorie-- » est sélectionnée et aucun produit n’est affiché.

[![sur le chargement de la page initiale, aucun produit n’est affiché](master-detail-filtering-with-a-dropdownlist-vb/_static/image36.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image35.png)

**Figure 13**: Dans la page chargement initial, aucun produit n’est affiché ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-vb/_static/image37.png))

La raison pour laquelle aucun produit n’est affiché lorsque l’élément de liste « --choisir une catégorie-- » est sélectionné est car sa valeur est `-1` et qu’il n’existe aucun produit dans la base de données avec un `CategoryID` de `-1`. S’il s’agit du comportement que vous souhaitez, vous avez terminé à ce stade. Toutefois, si vous souhaitez afficher *toutes* les catégories quand l’élément de liste « --choisir une catégorie-- » est sélectionné, revenez à la classe `ProductsBLL` et personnalisez la méthode `GetProductsByCategoryID(categoryID)` afin qu’elle appelle la méthode `GetProducts()` si le paramètre passé *`categoryID`* est inférieur à zéro :

[!code-vb[Main](master-detail-filtering-with-a-dropdownlist-vb/samples/sample2.vb)]

La technique utilisée ici est similaire à l’approche utilisée pour afficher tous les fournisseurs dans le didacticiel sur les [paramètres déclaratifs](../basic-reporting/declarative-parameters-cs.md) , bien que pour cet exemple, nous utilisons la valeur `-1` pour indiquer que tous les enregistrements doivent être récupérés au lieu de `Nothing`. Cela est dû au fait que le paramètre *`categoryID`* de la méthode `GetProductsByCategoryID(categoryID)` s’attend à ce que la valeur entière soit passée, tandis que dans le didacticiel sur les paramètres déclaratifs nous passons un paramètre d’entrée de chaîne.

La figure 14 illustre une capture d’écran de `FilterByDropDownList.aspx` lorsque l’option « --choisir une catégorie-- » est sélectionnée. Ici, tous les produits s’affichent par défaut et l’utilisateur peut affiner l’affichage en choisissant une catégorie spécifique.

[![tous les produits sont désormais répertoriés par défaut](master-detail-filtering-with-a-dropdownlist-vb/_static/image39.png)](master-detail-filtering-with-a-dropdownlist-vb/_static/image38.png)

**Figure 14**: Tous les produits sont désormais répertoriés par défaut ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-a-dropdownlist-vb/_static/image40.png))

## <a name="summary"></a>Récapitulatif

Lors de l’affichage de données hiérarchiques, il est souvent utile de présenter les données à l’aide de rapports maître/détail, à partir desquels l’utilisateur peut commencer à utiliser les données du haut de la hiérarchie et descendre en détail dans les détails. Dans ce didacticiel, nous avons examiné la création d’un rapport maître/détail simple indiquant les produits d’une catégorie sélectionnée. Pour ce faire, utilisez un contrôle DropDownList pour la liste des catégories et un GridView pour les produits appartenant à la catégorie sélectionnée.

Dans le [didacticiel suivant](master-detail-filtering-with-two-dropdownlists-vb.md) , nous allons utiliser l’interface DropDownList une étape supplémentaire, en utilisant deux DropDownList.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Précédent](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
> [Suivant](master-detail-filtering-with-two-dropdownlists-vb.md)
