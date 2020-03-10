---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-cs
title: Personnalisation de l’interface de modification du contrôleC#DataList () | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons créer une interface de modification plus riche pour la DataList, une qui comprend DropDownList et une case à cocher.
ms.author: riande
ms.date: 10/30/2006
ms.assetid: a5d13067-ddfb-4c36-8209-0f69fd40e45c
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 81f5a7f6737f544f577447f263dbd37dbc8279d9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78594121"
---
# <a name="customizing-the-datalists-editing-interface-c"></a>Personnalisation de l’interface de modification du contrôle DataList (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_40_CS.exe) ou [Télécharger le PDF](customizing-the-datalist-s-editing-interface-cs/_static/datatutorial40cs1.pdf)

> Dans ce didacticiel, nous allons créer une interface de modification plus riche pour la DataList, une qui comprend DropDownList et une case à cocher.

## <a name="introduction"></a>Introduction

Le balisage et les contrôles Web dans le contrôle DataList `EditItemTemplate` définissent son interface modifiable. Dans tous les exemples DataList modifiables que nous avons examinés jusqu’à présent, l’interface modifiable est composée de contrôles Web TextBox. Dans le [didacticiel précédent](adding-validation-controls-to-the-datalist-s-editing-interface-cs.md) , nous avons amélioré l’expérience utilisateur de modification en ajoutant des contrôles de validation.

Le `EditItemTemplate` peut être développé pour inclure des contrôles Web autres que la zone de texte, tels que DropDownList, RadioButtonLists, calendars, etc. Comme pour les zones de texte, lorsque vous personnalisez l’interface de modification pour inclure d’autres contrôles Web, utilisez les étapes suivantes :

1. Ajoutez le contrôle Web au `EditItemTemplate`.
2. Utilisez la syntaxe DataBinding pour assigner la valeur du champ de données correspondant à la propriété appropriée.
3. Dans le gestionnaire d’événements `UpdateCommand`, accédez par programmation à la valeur de contrôle Web et transmettez-la dans la méthode BLL appropriée.

Dans ce didacticiel, nous allons créer une interface de modification plus riche pour la DataList, une qui comprend DropDownList et une case à cocher. En particulier, nous allons créer un contrôle DataList qui répertorie les informations produit et permet de mettre à jour le nom du produit, le fournisseur, la catégorie et l’État abandonné (voir figure 1).

[![l’interface de modification contient une zone de texte, deux DropDownList et une case à cocher](customizing-the-datalist-s-editing-interface-cs/_static/image2.png)](customizing-the-datalist-s-editing-interface-cs/_static/image1.png)

**Figure 1**: l’interface de modification contient une zone de texte, deux DropDownList et une case à cocher ([cliquez pour afficher l’image en taille réelle](customizing-the-datalist-s-editing-interface-cs/_static/image3.png))

## <a name="step-1-displaying-product-information"></a>Étape 1 : affichage des informations sur le produit

Avant de pouvoir créer l’interface modifiable DataList, nous devons tout d’abord créer l’interface en lecture seule. Commencez par ouvrir la page `CustomizedUI.aspx` dans le dossier `EditDeleteDataList` et, à partir du concepteur, ajoutez un contrôle DataList à la page, en affectant à sa propriété `ID` la valeur `Products`. À partir de la balise active de DataList s, créez un nouvel ObjectDataSource. Nommez ce nouvel ObjectDataSource `ProductsDataSource` et configurez-le pour extraire les données de la méthode `GetProducts` de la classe `ProductsBLL`. Comme avec les didacticiels DataList modifiables précédents, nous allons mettre à jour les informations du produit édité en accédant directement à la couche de logique métier. Par conséquent, définissez les listes déroulantes dans les onglets mettre à jour, insérer et supprimer sur (aucun).

[![définir les listes déroulantes des onglets mettre à jour, insérer et supprimer sur (aucun)](customizing-the-datalist-s-editing-interface-cs/_static/image5.png)](customizing-the-datalist-s-editing-interface-cs/_static/image4.png)

**Figure 2**: définir les listes déroulantes des onglets mettre à jour, insérer et supprimer sur (aucun) ([cliquez pour afficher l’image en taille réelle](customizing-the-datalist-s-editing-interface-cs/_static/image6.png))

Après la configuration de l’ObjectDataSource, Visual Studio crée un `ItemTemplate` par défaut pour le contrôle DataList qui répertorie le nom et la valeur de chacun des champs de données retournés. Modifiez la `ItemTemplate` afin que le modèle répertorie le nom du produit dans un élément `<h4>` avec le nom de la catégorie, le nom du fournisseur, le prix et l’État abandonné. En outre, ajoutez un bouton modifier, en vous assurant que sa propriété `CommandName` est définie sur Edit. Le balisage déclaratif pour mon `ItemTemplate` suit :

[!code-aspx[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample1.aspx)]

Le balisage ci-dessus présente les informations sur le produit à l’aide d’un en-tête &lt;H4&gt; pour le nom du produit et un `<table>` de quatre colonnes pour les champs restants. Les classes CSS `ProductPropertyLabel` et `ProductPropertyValue`, définies dans `Styles.css`, ont été abordées dans les didacticiels précédents. La figure 3 illustre notre progression dans un navigateur.

[![le nom, le fournisseur, la catégorie, l’État abandonné et le prix de chaque produit s’affichent.](customizing-the-datalist-s-editing-interface-cs/_static/image8.png)](customizing-the-datalist-s-editing-interface-cs/_static/image7.png)

**Figure 3**: affichage du nom, du fournisseur, de la catégorie, de l’État abandonné et du prix de chaque produit ([cliquez pour afficher l’image en plein écran](customizing-the-datalist-s-editing-interface-cs/_static/image9.png))

## <a name="step-2-adding-the-web-controls-to-the-editing-interface"></a>Étape 2 : ajout des contrôles Web à l’interface de modification

La première étape de la création de l’interface de modification DataList personnalisée consiste à ajouter les contrôles Web nécessaires au `EditItemTemplate`. En particulier, nous avons besoin d’un contrôle DropDownList pour la catégorie, un autre pour le fournisseur et une case à cocher pour l’État abandonné. Étant donné que le prix du produit n’est pas modifiable dans cet exemple, nous pouvons continuer à l’afficher à l’aide d’un contrôle Web Label.

Pour personnaliser l’interface de modification, cliquez sur le lien modifier les modèles dans la balise active de DataList s et choisissez l’option `EditItemTemplate` dans la liste déroulante. Ajoutez un DropDownList au `EditItemTemplate` et affectez à son `ID` la valeur `Categories`.

[![ajouter un DropDownList pour les catégories](customizing-the-datalist-s-editing-interface-cs/_static/image11.png)](customizing-the-datalist-s-editing-interface-cs/_static/image10.png)

**Figure 4**: ajouter un DropDownList pour les catégories ([cliquez pour afficher l’image en taille réelle](customizing-the-datalist-s-editing-interface-cs/_static/image12.png))

Ensuite, dans la balise active DropDownList, sélectionnez l’option choisir la source de données et créez une nouvelle ObjectDataSource nommée `CategoriesDataSource`. Configurez cet ObjectDataSource pour utiliser la méthode `CategoriesBLL` classe s `GetCategories()` (voir figure 5). Ensuite, l’Assistant Configuration de source de données DropDownList s vous invite à entrer les champs de données à utiliser pour chaque `ListItem` s `Text` et `Value` propriétés. Si le DropDownList affiche le champ de données `CategoryName` et utilise le `CategoryID` comme valeur, comme illustré à la figure 6.

[![créer un ObjectDataSource nommé CategoriesDataSource](customizing-the-datalist-s-editing-interface-cs/_static/image14.png)](customizing-the-datalist-s-editing-interface-cs/_static/image13.png)

**Figure 5**: créer un ObjectDataSource nommé `CategoriesDataSource` ([cliquez pour afficher l’image en taille réelle](customizing-the-datalist-s-editing-interface-cs/_static/image15.png))

[![configurer les champs d’affichage et de valeur DropDownList](customizing-the-datalist-s-editing-interface-cs/_static/image17.png)](customizing-the-datalist-s-editing-interface-cs/_static/image16.png)

**Figure 6**: configurer les champs d’affichage et de valeur DropDownList ([cliquez pour afficher l’image en taille réelle](customizing-the-datalist-s-editing-interface-cs/_static/image18.png))

Répétez cette série d’étapes pour créer un DropDownList pour les fournisseurs. Définissez le `ID` de ce DropDownList sur `Suppliers` et nommez son `SuppliersDataSource`ObjectDataSource.

Après avoir ajouté les deux DropDownList, ajoutez une case à cocher pour l’État abandonné et une zone de texte pour le nom du produit s. Définissez la `ID` s pour la case à cocher et la zone de texte sur `Discontinued` et `ProductName`, respectivement. Ajoutez un RequiredFieldValidator pour vous assurer que l’utilisateur fournit une valeur pour le nom du produit.

Enfin, ajoutez les boutons mettre à jour et annuler. N’oubliez pas que pour ces deux boutons, il est impératif que leurs propriétés `CommandName` soient définies sur Update et Cancel, respectivement.

N’hésitez pas à disposer l’interface de modification comme vous le souhaitez. J’ai choisi d’utiliser la même disposition de `<table>` à quatre colonnes à partir de l’interface en lecture seule, comme l’illustre la syntaxe déclarative et la capture d’écran suivantes :

[!code-aspx[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample2.aspx)]

[![l’interface de modification est présentée comme l’interface en lecture seule](customizing-the-datalist-s-editing-interface-cs/_static/image20.png)](customizing-the-datalist-s-editing-interface-cs/_static/image19.png)

**Figure 7**: l’interface de modification est présentée comme l’interface en lecture seule ([cliquez pour afficher l’image en taille réelle](customizing-the-datalist-s-editing-interface-cs/_static/image21.png))

## <a name="step-3-creating-the-editcommand-and-cancelcommand-event-handlers"></a>Étape 3 : création des gestionnaires d’événements EditCommand et CancelCommand

Actuellement, il n’y a pas de syntaxe DataBinding dans le `EditItemTemplate` (à l’exception de la `UnitPriceLabel`, qui a été copiée sur Verbatim à partir du `ItemTemplate`). Nous ajouterons la syntaxe DataBinding momentanément, mais commençons par créer les gestionnaires d’événements pour les événements DataList `EditCommand` et `CancelCommand`. Rappelez-vous que la responsabilité du gestionnaire d’événements `EditCommand` consiste à afficher l’interface de modification pour l’élément DataList sur lequel l’utilisateur a cliqué sur le bouton modifier, tandis que le travail `CancelCommand` est de ramener le contrôle DataList à son état antérieur à la modification.

Créez ces deux gestionnaires d’événements et utilisez le code suivant :

[!code-csharp[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample3.cs)]

Une fois ces deux gestionnaires d’événements en place, cliquez sur le bouton modifier pour afficher l’interface de modification et cliquer sur le bouton Annuler pour retourner l’élément modifié en mode lecture seule. La figure 8 illustre le contrôle DataList une fois que vous avez cliqué sur le bouton modifier pour chef de s Gumbo Mix. Étant donné que nous avons encore ajouté une syntaxe DataBinding à l’interface de modification, la zone de texte `ProductName` est vide, la case `Discontinued` désactivée et les premiers éléments sélectionnés dans la `Categories` et `Suppliers` DropDownList.

[![cliquant sur le bouton modifier affiche l’interface de modification](customizing-the-datalist-s-editing-interface-cs/_static/image23.png)](customizing-the-datalist-s-editing-interface-cs/_static/image22.png)

**Figure 8**: lorsque vous cliquez sur le bouton modifier, l’interface de modification s’affiche ([cliquez pour afficher l’image en taille réelle](customizing-the-datalist-s-editing-interface-cs/_static/image24.png))

## <a name="step-4-adding-the-databinding-syntax-to-the-editing-interface"></a>Étape 4 : ajout de la syntaxe DataBinding à l’interface de modification

Pour que l’interface de modification affiche les valeurs s du produit actuel, nous devons utiliser la syntaxe DataBinding pour assigner les valeurs de champ de données aux valeurs de contrôle Web appropriées. La syntaxe DataBinding peut être appliquée par le biais du concepteur en accédant à l’écran modifier les modèles et en sélectionnant le lien modifier les DataBindings dans les balises actives des contrôles Web. La syntaxe DataBinding peut également être ajoutée directement à la balise déclarative.

Affectez la valeur du champ de données `ProductName` à la propriété `ProductName` TextBox s `Text`, les valeurs de champ de données `CategoryID` et `SupplierID` aux propriétés `Categories` et `Suppliers` DropDownList `SelectedValue`, et la valeur de champ de données `Discontinued` à la propriété `Discontinued` s `Checked`. Après avoir apporté ces modifications, par le biais du concepteur ou directement via le balisage déclaratif, réexaminez la page via un navigateur, puis cliquez sur le bouton modifier pour chef Anton s Gumbo Mix. Comme le montre la figure 9, la syntaxe DataBinding a ajouté les valeurs actuelles dans les zones de texte, DropDownList et CheckBox.

[![cliquant sur le bouton modifier affiche l’interface de modification](customizing-the-datalist-s-editing-interface-cs/_static/image26.png)](customizing-the-datalist-s-editing-interface-cs/_static/image25.png)

**Figure 9**: Si vous cliquez sur le bouton modifier, l’interface de modification s’affiche ([cliquez pour afficher l’image en taille réelle](customizing-the-datalist-s-editing-interface-cs/_static/image27.png))

## <a name="step-5-saving-the-user-s-changes-in-the-updatecommand-event-handler"></a>Étape 5 : enregistrement des modifications apportées aux utilisateurs dans le gestionnaire d’événements UpdateCommand

Lorsque l’utilisateur modifie un produit et clique sur le bouton mettre à jour, une publication (postback) se produit et l’événement de `UpdateCommand` DataList est déclenché. Dans le gestionnaire d’événements, nous devons lire les valeurs des contrôles Web dans le `EditItemTemplate` et l’interface avec la couche BLL pour mettre à jour le produit dans la base de données. Comme nous l’avons vu dans les didacticiels précédents, le `ProductID` du produit mis à jour est accessible via le regroupement `DataKeys`. Les champs entrés par l’utilisateur sont accessibles en référençant par programmation les contrôles Web à l’aide de `FindControl("controlID")`, comme le montre le code suivant :

[!code-csharp[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample4.cs)]

Le code commence par consulter la propriété `Page.IsValid` pour s’assurer que tous les contrôles de validation de la page sont valides. Si `Page.IsValid` est `True`, la valeur `ProductID` du produit modifié est lue à partir de la collection `DataKeys` et les contrôles Web de saisie de données dans le `EditItemTemplate` sont référencés par programmation. Ensuite, les valeurs de ces contrôles Web sont lues dans des variables qui sont ensuite transmises à la surcharge de `UpdateProduct` appropriée. Après la mise à jour des données, la DataList est retournée à son état antérieur à la modification.

> [!NOTE]
> J’ai omis la logique de gestion des exceptions ajoutée dans le didacticiel de [gestion des exceptions de niveau BLL et dal](handling-bll-and-dal-level-exceptions-cs.md) afin de garder le code et cet exemple concentrés. En guise d’exercice, ajoutez cette fonctionnalité une fois ce didacticiel terminé.

## <a name="step-6-handling-null-categoryid-and-supplierid-values"></a>Étape 6 : gestion des valeurs CategoryID et RéfFournisseur NULL

La base de données Northwind permet de `NULL` des valeurs pour les colonnes `Products` table s `CategoryID` et `SupplierID`. Toutefois, notre interface de modification ne prend actuellement pas en charge les valeurs `NULL`. Si nous tentons de modifier un produit qui a une valeur de `NULL` pour ses colonnes `CategoryID` ou `SupplierID`, nous obtenons un `ArgumentOutOfRangeException` avec un message d’erreur semblable à : *« categories » a une valeur SelectedValue qui n’est pas valide, car elle n’existe pas dans la liste d’éléments.* En outre, il n’existe actuellement aucun moyen de modifier la valeur d’une catégorie ou d’un fournisseur de produit à partir d’une valeur non`NULL` en `NULL` un.

Pour prendre en charge les valeurs de `NULL` pour les DropDownList Category et supplier, nous devons ajouter un `ListItem`supplémentaire. J’ai choisi d’utiliser (aucun) comme valeur de `Text` pour cette `ListItem`, mais vous pouvez la remplacer par une autre valeur (telle qu’une chaîne vide) si vous aimez. Enfin, n’oubliez pas de définir la `AppendDataBoundItems` DropDownList sur `True`; Si vous oubliez de le faire, les catégories et les fournisseurs liés à la DropDownList remplacent la `ListItem`ajoutée statiquement.

Une fois ces modifications apportées, le balisage DropDownList dans la DataList s `EditItemTemplate` doit ressembler à ce qui suit :

[!code-aspx[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample5.aspx)]

> [!NOTE]
> Les `ListItem` statiques peuvent être ajoutées à un DropDownList par le biais du concepteur ou directement par le biais de la syntaxe déclarative. Quand vous ajoutez un élément DropDownList pour représenter une valeur de `NULL` de base de données, veillez à ajouter le `ListItem` par le biais de la syntaxe déclarative. Si vous utilisez l’éditeur de collections `ListItem` dans le concepteur, la syntaxe déclarative générée omet le paramètre `Value` en même temps lorsqu’une chaîne vide est assignée, créant ainsi un balisage déclaratif comme : `<asp:ListItem>(None)</asp:ListItem>`. Bien que cela puisse paraître inoffensif, le `Value` manquant entraîne l’utilisation par le DropDownList de la valeur de propriété `Text` à la place. Cela signifie que si ce `NULL` `ListItem` est sélectionné, la valeur (aucune) sera tentée pour être assignée au champ de données du produit (`CategoryID` ou `SupplierID`, dans ce didacticiel), ce qui entraînera une exception. En définissant explicitement `Value=""`, une valeur de `NULL` est assignée au champ de données du produit lorsque la `ListItem` `NULL` est sélectionnée.

Prenez un moment pour voir notre progression dans un navigateur. Lorsque vous modifiez un produit, Notez que les `Categories` et `Suppliers` DropDownList ont une option (aucune) au début du DropDownList.

[![les catégories et les fournisseurs DropDownList incluent une option (aucune)](customizing-the-datalist-s-editing-interface-cs/_static/image29.png)](customizing-the-datalist-s-editing-interface-cs/_static/image28.png)

**Figure 10**: l' `Categories` et `Suppliers` DropDownList incluent une option (aucune) ([cliquez pour afficher l’image en taille réelle](customizing-the-datalist-s-editing-interface-cs/_static/image30.png))

Pour enregistrer l’option (None) comme valeur de base de données `NULL`, nous devons revenir au gestionnaire d’événements `UpdateCommand`. Modifiez les variables `categoryIDValue` et `supplierIDValue` pour qu’elles soient des entiers Nullable et attribuez-leur une valeur autre que `Nothing` uniquement si le DropDownList s `SelectedValue` n’est pas une chaîne vide :

[!code-csharp[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample6.cs)]

Avec cette modification, une valeur de `Nothing` est transmise dans la méthode BLL `UpdateProduct` si l’utilisateur a sélectionné l’option (aucune) dans l’une des listes déroulantes, ce qui correspond à une valeur de base de données `NULL`.

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, nous avons vu comment créer une interface de modification DataList plus complexe qui comprenait trois contrôles Web d’entrée différents, une zone de texte, deux DropDownList et une case à cocher, ainsi que des contrôles de validation. Lors de la génération de l’interface de modification, les étapes sont les mêmes, quels que soient les contrôles Web utilisés : commencez par ajouter les contrôles Web aux contrôles DataList `EditItemTemplate`; Utilisez la syntaxe DataBinding pour assigner les valeurs de champ de données correspondantes avec les propriétés de contrôle Web appropriées ; et, dans le gestionnaire d’événements `UpdateCommand`, accéder par programmation aux contrôles Web et à leurs propriétés appropriées, en passant leurs valeurs dans la couche BLL.

Lors de la création d’une interface de modification, qu’elle soit composée uniquement de zones de texte ou d’une collection de contrôles Web différents, veillez à gérer correctement les valeurs de `NULL` de base de données. Lors de la gestion des `NULL`, il est impératif que vous affichiez non seulement une valeur de `NULL` existante dans l’interface de modification, mais également que vous offrez un moyen de marquer une valeur comme `NULL`. Pour DropDownList dans DataLists, cela signifie généralement que l’ajout d’un `ListItem` statique dont la propriété `Value` est explicitement définie sur une chaîne vide (`Value=""`) et l’ajout d’un peu de code au gestionnaire d’événements `UpdateCommand` pour déterminer si le `NULL``ListItem` a ou non été sélectionné.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel étaient Denis Patterson, David SURU et Randy Schmidt. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](adding-validation-controls-to-the-datalist-s-editing-interface-cs.md)
> [Suivant](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md)
