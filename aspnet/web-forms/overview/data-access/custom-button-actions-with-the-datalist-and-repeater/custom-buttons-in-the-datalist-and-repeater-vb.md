---
uid: web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-vb
title: Boutons personnalisés dans les contrôles DataList et Repeater (VB) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons créer une interface qui utilise un répéteur pour répertorier les catégories dans le système, chaque catégorie fournissant un bouton permettant d’afficher son associ...
ms.author: riande
ms.date: 11/13/2006
ms.assetid: 1afdb14d-6e49-4e1f-aead-2934730d472e
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-vb
msc.type: authoredcontent
ms.openlocfilehash: bc7e94e59226b739c2948434c1bfecb46b3d7856
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78576397"
---
# <a name="custom-buttons-in-the-datalist-and-repeater-vb"></a>Boutons personnalisés dans les contrôles DataList et Repeater (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_46_VB.exe) ou [Télécharger le PDF](custom-buttons-in-the-datalist-and-repeater-vb/_static/datatutorial46vb1.pdf)

> Dans ce didacticiel, nous allons créer une interface qui utilise un Repeater pour répertorier les catégories dans le système, chaque catégorie fournissant un bouton permettant d’afficher ses produits associés à l’aide d’un contrôle BulletedList.

## <a name="introduction"></a>Introduction

Dans les dix derniers sept didacticiels de DataList et de Repeater, nous avons créé des exemples en lecture seule et des exemples de modification et de suppression. Pour faciliter la modification et la suppression des fonctionnalités au sein d’un contrôle DataList, nous avons ajouté des boutons aux contrôles DataList `ItemTemplate` qui, lorsque vous cliquez dessus, ont provoqué une publication et déclenché un événement DataList correspondant à la propriété Button s `CommandName`. Par exemple, si vous ajoutez un bouton au `ItemTemplate` avec une valeur de propriété `CommandName` de Edit, les contrôles DataList `EditCommand` se déclenchent lors de la publication (postback); l’un avec l' `CommandName` DELETE déclenche l' `DeleteCommand`.

Outre les boutons modifier et supprimer, les contrôles DataList et Repeater peuvent également inclure des boutons, LinkButtons ou ImageButtons qui, lorsque vous cliquez dessus, exécutent une logique personnalisée côté serveur. Dans ce didacticiel, nous allons créer une interface qui utilise un répéteur pour répertorier les catégories dans le système. Pour chaque catégorie, le Repeater inclut un bouton pour afficher les produits associés de catégorie à l’aide d’un contrôle BulletedList (voir figure 1).

[![cliquant sur le lien Afficher les produits affiche les produits de la catégorie dans une liste à puces](custom-buttons-in-the-datalist-and-repeater-vb/_static/image2.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image1.png)

**Figure 1**: lorsque vous cliquez sur le lien Afficher les produits, les produits de la catégorie s’affichent dans une liste à puces ([cliquez pour afficher l’image en plein écran](custom-buttons-in-the-datalist-and-repeater-vb/_static/image3.png))

## <a name="step-1-adding-the-custom-button-tutorial-web-pages"></a>Étape 1 : ajout des pages Web du didacticiel du bouton personnalisé

Avant d’examiner comment ajouter un bouton personnalisé, commençons par prendre un moment pour créer les pages ASP.NET dans notre projet de site Web dont nous aurons besoin pour ce didacticiel. Commencez par ajouter un nouveau dossier nommé `CustomButtonsDataListRepeater`. Ensuite, ajoutez les deux pages ASP.NET suivantes à ce dossier, en veillant à associer chaque page à la page maître `Site.master` :

- `Default.aspx`
- `CustomButtons.aspx`

![Ajouter les pages ASP.NET pour les didacticiels associés aux boutons personnalisés](custom-buttons-in-the-datalist-and-repeater-vb/_static/image4.png)

**Figure 2**: ajouter les pages ASP.net pour les didacticiels associés aux boutons personnalisés

Comme dans les autres dossiers, `Default.aspx` dans le dossier `CustomButtonsDataListRepeater` répertorie les didacticiels de la section. N’oubliez pas que le contrôle utilisateur `SectionLevelTutorialListing.ascx` fournit cette fonctionnalité. Ajoutez ce contrôle utilisateur à `Default.aspx` en le faisant glisser de l’Explorateur de solutions sur la page s Mode Création.

[![ajouter le contrôle utilisateur SectionLevelTutorialListing. ascx à default. aspx](custom-buttons-in-the-datalist-and-repeater-vb/_static/image6.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image5.png)

**Figure 3**: ajouter le contrôle utilisateur `SectionLevelTutorialListing.ascx` à `Default.aspx` ([cliquez pour afficher l’image en taille réelle](custom-buttons-in-the-datalist-and-repeater-vb/_static/image7.png))

Enfin, ajoutez les pages en tant qu’entrées au fichier `Web.sitemap`. Plus précisément, ajoutez le balisage suivant après la pagination et le tri avec les `<siteMapNode>`DataList et Repeater :

[!code-xml[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample1.xml)]

Après avoir mis à jour `Web.sitemap`, prenez un moment pour afficher le site Web des didacticiels via un navigateur. Le menu à gauche contient maintenant des éléments pour les didacticiels de modification, d’insertion et de suppression.

![Le plan de site comprend désormais l’entrée du didacticiel sur les boutons personnalisés](custom-buttons-in-the-datalist-and-repeater-vb/_static/image8.png)

**Figure 4**: le plan de site comprend désormais l’entrée du didacticiel sur les boutons personnalisés

## <a name="step-2-adding-the-list-of-categories"></a>Étape 2 : ajout de la liste des catégories

Pour ce didacticiel, nous devons créer un répéteur qui répertorie toutes les catégories avec un LinkButton afficher les produits qui, lorsque vous cliquez dessus, affiche les produits des catégories associées dans une liste à puces. Commençons par créer un répéteur simple qui répertorie les catégories dans le système. Commencez par ouvrir la page `CustomButtons.aspx` dans le dossier `CustomButtonsDataListRepeater`. Faites glisser un Repeater de la boîte à outils vers le concepteur et affectez à sa propriété `ID` la valeur `Categories`. Ensuite, créez un nouveau contrôle de source de données à partir de la balise active de Repeater s. En particulier, créez un nouveau contrôle ObjectDataSource nommé `CategoriesDataSource` qui sélectionne ses données à partir de la méthode de `CategoriesBLL` classe s `GetCategories()`.

[![configurer ObjectDataSource pour utiliser la méthode CategoriesBLL de la classe s GetCategories ()](custom-buttons-in-the-datalist-and-repeater-vb/_static/image10.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image9.png)

**Figure 5**: configurer ObjectDataSource pour utiliser la méthode `CategoriesBLL` classe s `GetCategories()` ([cliquez pour afficher l’image en taille réelle](custom-buttons-in-the-datalist-and-repeater-vb/_static/image11.png))

Contrairement au contrôle DataList, pour lequel Visual Studio crée une `ItemTemplate` par défaut basée sur la source de données, les modèles Repeater s doivent être définis manuellement. En outre, les modèles Repeater s doivent être créés et modifiés de façon déclarative (autrement dit, il n’y a pas d’option modifier les modèles dans la balise active de Repeater s).

Cliquez sur l’onglet source dans le coin inférieur gauche et ajoutez une `ItemTemplate` qui affiche le nom de la catégorie dans un élément `<h3>` et sa description dans une balise de paragraphe. incluez une `SeparatorTemplate` qui affiche une règle horizontale (`<hr />`) entre chaque catégorie. Ajoutez également un LinkButton dont la propriété `Text` est définie sur Afficher les produits. Une fois ces étapes terminées, le balisage déclaratif de votre page doit ressembler à ce qui suit :

[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample2.aspx)]

La figure 6 illustre la page affichée dans un navigateur. Chaque nom de catégorie et description est listé. Quand l’utilisateur clique sur le bouton afficher les produits, provoque une publication mais n’effectue aucune action.

[![le nom et la description de chaque catégorie s’affichent, ainsi qu’un LinkButton afficher les produits](custom-buttons-in-the-datalist-and-repeater-vb/_static/image13.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image12.png)

**Figure 6**: le nom et la description de chaque catégorie s’affichent, ainsi qu’un LinkButton afficher les produits ([cliquez pour afficher l’image en plein écran](custom-buttons-in-the-datalist-and-repeater-vb/_static/image14.png))

## <a name="step-3-executing-server-side-logic-when-the-show-products-linkbutton-is-clicked"></a>Étape 3 : exécution de la logique côté serveur lorsque l’utilisateur clique sur le LinkButton afficher les produits

Chaque fois qu’un utilisateur clique sur un bouton, LinkButton ou ImageButton dans un modèle dans un contrôle DataList ou Repeater, une publication (postback) se produit et l’événement DataList ou Repeater s `ItemCommand` se déclenche. En plus de l’événement `ItemCommand`, le contrôle DataList peut également déclencher un autre événement plus spécifique si la propriété Button s `CommandName` est définie sur l’une des chaînes réservées (Delete, Edit, Cancel, Update ou Select), mais que l’événement `ItemCommand` est *toujours* déclenché.

Lorsque vous cliquez sur un bouton dans un contrôle DataList ou Repeater, il est souvent nécessaire de passer le long du bouton sur lequel l’utilisateur a cliqué (dans le cas où il peut y avoir plusieurs boutons dans le contrôle, comme un bouton modifier et supprimer) et peut-être des informations supplémentaires (par exemple, valeur de clé primaire de l’élément sur lequel l’utilisateur a cliqué sur le bouton). Le bouton, LinkButton et ImageButton fournissent deux propriétés dont les valeurs sont passées au gestionnaire d’événements `ItemCommand` :

- `CommandName` une chaîne généralement utilisée pour identifier chaque bouton dans le modèle
- `CommandArgument` couramment utilisé pour contenir la valeur d’un champ de données, tel que la valeur de clé primaire.

Pour cet exemple, définissez la propriété LinkButton s `CommandName` sur ShowProducts et liez la valeur de clé primaire de l’enregistrement actuel `CategoryID` à la propriété `CommandArgument` à l’aide de la syntaxe de liaison de liaison `CategoryArgument='<%# Eval("CategoryID") %>'`. Une fois ces deux propriétés spécifiées, la syntaxe déclarative des LinkButtons doit ressembler à ce qui suit :

[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample3.aspx)]

Lorsque l’utilisateur clique sur le bouton, une publication (postback) se produit et l’événement DataList ou Repeater s `ItemCommand` se déclenche. Le gestionnaire d’événements reçoit le bouton s `CommandName` et `CommandArgument` valeurs.

Créez un gestionnaire d’événements pour l’événement Repeater s `ItemCommand` et notez le second paramètre transmis au gestionnaire d’événements (nommé `e`). Ce deuxième paramètre est de type [`RepeaterCommandEventArgs`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeatercommandeventargs.aspx) et possède les quatre propriétés suivantes :

- `CommandArgument` la valeur du bouton cliqué s `CommandArgument` propriété
- `CommandName` la valeur de la propriété Button s `CommandName`
- `CommandSource` une référence au contrôle bouton sur lequel l’utilisateur a cliqué
- `Item` une référence au [`RepeaterItem`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem.aspx) qui contient le bouton sur lequel l’utilisateur a cliqué ; chaque enregistrement lié au répéteur est représenté sous la forme d’un `RepeaterItem`

Étant donné que les `CategoryID` des catégories sélectionnées sont transmises via la propriété `CommandArgument`, nous pouvons récupérer l’ensemble des produits associés à la catégorie sélectionnée dans le gestionnaire d’événements `ItemCommand`. Ces produits peuvent ensuite être liés à un contrôle BulletedList dans le `ItemTemplate` (que nous avons encore ajoutés). Tout ce qui reste, c’est d’ajouter la fonction BulletedList, de la référencer dans le gestionnaire d’événements `ItemCommand` et de la lier à l’ensemble des produits pour la catégorie sélectionnée, que nous allons aborder à l’étape 4.

> [!NOTE]
> Le gestionnaire d’événements `ItemCommand` DataList est passé à un objet de type [`DataListCommandEventArgs`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistcommandeventargs.aspx), qui offre les quatre mêmes propriétés que la classe `RepeaterCommandEventArgs`.

## <a name="step-4-displaying-the-selected-category-s-products-in-a-bulleted-list"></a>Étape 4 : affichage des produits de la catégorie sélectionnée dans une liste à puces

Les produits de la catégorie sélectionnée peuvent être affichés dans le `ItemTemplate` du répétiteur à l’aide d’un nombre quelconque de contrôles. Nous pourrions ajouter un autre Repeater imbriqué, un contrôle DataList, un contrôle DropDownList, un GridView, etc. Étant donné que nous souhaitons afficher les produits sous la forme d’une liste à puces, nous allons utiliser le contrôle BulletedList. En revenant au balisage déclaratif de la page `CustomButtons.aspx`, ajoutez un contrôle BulletedList au `ItemTemplate` après le LinkButton Show Products. Définissez le `ID` s BulletedLists sur `ProductsInCategory`. L’BulletedList affiche la valeur du champ de données spécifié via la propriété `DataTextField` ; étant donné que ce contrôle sera lié à des informations sur le produit, définissez la propriété `DataTextField` sur `ProductName`.

[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample4.aspx)]

Dans le gestionnaire d’événements `ItemCommand`, référencez ce contrôle à l’aide de `e.Item.FindControl("ProductsInCategory")` et liez-le à l’ensemble des produits associés à la catégorie sélectionnée.

[!code-vb[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample5.vb)]

Avant d’effectuer une action dans le gestionnaire d’événements `ItemCommand`, il est prudent de vérifier d’abord la valeur de la `CommandName`entrante. Étant donné que le gestionnaire d’événements `ItemCommand` se déclenche lorsque l’utilisateur clique sur *un* bouton, s’il y a plusieurs boutons dans le modèle, utilisez la valeur `CommandName` pour identifier l’action à entreprendre. La vérification de la `CommandName` ici est discutable, car nous n’avons qu’un seul bouton, mais il s’agit d’une bonne habitude de forme. Ensuite, les `CategoryID` de la catégorie sélectionnée sont récupérés à partir de la propriété `CommandArgument`. Le contrôle BulletedList dans le modèle est ensuite référencé et lié aux résultats de la méthode `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)`.

Dans les didacticiels précédents qui utilisaient les boutons d’un contrôle DataList, comme [une vue d’ensemble de la modification et de la suppression de données dans le contrôle DataList](../editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md), nous avons déterminé la valeur de clé primaire d’un élément donné via la collection `DataKeys`. Bien que cette approche fonctionne bien avec la DataList, le Repeater n’a pas de propriété `DataKeys`. Au lieu de cela, nous devons utiliser une autre approche pour fournir la valeur de clé primaire, par exemple par le biais de la propriété Button s `CommandArgument` ou en affectant la valeur de clé primaire à un contrôle Web d’étiquette masqué dans le modèle et en lisant sa valeur dans le gestionnaire d’événements `ItemCommand` à l’aide de `e.Item.FindControl("LabelID")`.

Après avoir terminé le gestionnaire d’événements `ItemCommand`, prenez un moment pour tester cette page dans un navigateur. Comme le montre la figure 7, le fait de cliquer sur le lien Afficher les produits entraîne une publication et affiche les produits pour la catégorie sélectionnée dans un BulletedList. En outre, Notez que ces informations sur les produits sont conservées, même si vous cliquez sur autres catégories, vous pouvez cliquer sur les liens des produits.

> [!NOTE]
> Si vous souhaitez modifier le comportement de ce rapport, de sorte que les seuls produits de la catégorie sont répertoriés à la fois, il suffit de définir la propriété du contrôle BulletedList s `EnableViewState` sur `False`.

[![un BulletedList est utilisé pour afficher les produits de la catégorie sélectionnée](custom-buttons-in-the-datalist-and-repeater-vb/_static/image16.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image15.png)

**Figure 7**: un BulletedList est utilisé pour afficher les produits de la catégorie sélectionnée ([cliquez pour afficher l’image en taille réelle](custom-buttons-in-the-datalist-and-repeater-vb/_static/image17.png))

## <a name="summary"></a>Récapitulatif

Les contrôles DataList et Repeater peuvent inclure un nombre quelconque de boutons, LinkButtons ou ImageButtons dans leurs modèles. De tels boutons, lorsque vous cliquez dessus, provoquent une publication (postback) et déclenchent l’événement `ItemCommand`. Pour associer une action personnalisée côté serveur à un clic sur un bouton, créez un gestionnaire d’événements pour l’événement `ItemCommand`. Dans ce gestionnaire d’événements, vérifiez d’abord la valeur de `CommandName` entrant pour déterminer le bouton sur lequel l’utilisateur a cliqué. Vous pouvez éventuellement fournir des informations supplémentaires à l’aide de la propriété Button s `CommandArgument`.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel était de Denis Patterson. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](custom-buttons-in-the-datalist-and-repeater-cs.md)
