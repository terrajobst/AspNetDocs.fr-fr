---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb
title: Personnalisation de l’interface de modification de données (VB) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons examiner comment personnaliser l’interface d’un GridView modifiable, en remplaçant les contrôles TextBox et CheckBox standard par des alternances...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 4830d984-bd2c-4a08-bfe5-2385599f1f7d
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 85ec7bdde6b2bffbbda066b0441bbd36b7072197
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78592616"
---
# <a name="customizing-the-data-modification-interface-vb"></a>Personnalisation de l’interface de modification des données (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_20_VB.exe) ou [Télécharger le PDF](customizing-the-data-modification-interface-vb/_static/datatutorial20vb1.pdf)

> Dans ce didacticiel, nous allons examiner comment personnaliser l’interface d’un GridView modifiable en remplaçant les contrôles TextBox et CheckBox standard par d’autres contrôles Web d’entrée.

## <a name="introduction"></a>Introduction

Les BoundFields et CheckBoxFields utilisés par les contrôles GridView et DetailsView simplifient le processus de modification des données en raison de leur capacité à afficher des interfaces en lecture seule, modifiables et pouvant être insérées. Ces interfaces peuvent être rendues sans qu’il soit nécessaire d’ajouter des balises ou du code déclaratifs supplémentaires. Toutefois, les interfaces BoundField et CheckBoxField n’ont pas la possibilité de personnalisation souvent requise dans les scénarios réels. Pour personnaliser l’interface modifiable ou pouvant être insérée dans un contrôle GridView ou DetailsView, nous devons utiliser un TemplateField.

Dans le [didacticiel précédent](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) , nous avons vu comment personnaliser les interfaces de modification des données en ajoutant des contrôles Web de validation. Dans ce didacticiel, nous allons examiner comment personnaliser les contrôles Web de la collecte de données, en remplaçant les contrôles TextBox et CheckBox standard de BoundField et de CheckBoxField par d’autres contrôles Web d’entrée. En particulier, nous allons créer un GridView modifiable qui autorise la mise à jour du nom, de la catégorie, du fournisseur et de l’état supprimé. Lors de la modification d’une ligne particulière, les champs Category et Supplier sont rendus sous la forme DropDownList, contenant l’ensemble des catégories et fournisseurs disponibles. En outre, nous allons remplacer la case à cocher par défaut de CheckBoxField par un contrôle RadioButtonList qui offre deux options : « active » et « Discontinued ».

[![l’interface de modification du contrôle GridView comprend DropDownList et des cases d’option](customizing-the-data-modification-interface-vb/_static/image2.png)](customizing-the-data-modification-interface-vb/_static/image1.png)

**Figure 1**: l’interface de modification du contrôle GridView comprend des DropDownList et des cases d’option ([cliquez pour afficher l’image en taille réelle](customizing-the-data-modification-interface-vb/_static/image3.png))

## <a name="step-1-creating-the-appropriateupdateproductoverload"></a>Étape 1 : création de la surcharge de`UpdateProduct`appropriée

Dans ce didacticiel, nous allons créer un GridView modifiable qui permet la modification du nom, de la catégorie, du fournisseur et de l’État abandonné d’un produit. Par conséquent, nous avons besoin d’une surcharge `UpdateProduct` qui accepte cinq paramètres d’entrée ces quatre valeurs de produit, ainsi que les `ProductID`. Comme dans les surcharges précédentes, celle-ci :

1. Récupérer les informations sur le produit de la base de données pour le `ProductID`spécifié,
2. Mettez à jour les champs `ProductName`, `CategoryID`, `SupplierID`et `Discontinued`, et
3. Envoyez la demande de mise à jour à la couche DAL par le biais de la méthode `Update()` du TableAdapter.

Par souci de concision, pour cette surcharge particulière, j’ai omis la vérification de la règle d’entreprise qui garantit qu’un produit marqué comme abandonné n’est pas le seul produit proposé par son fournisseur. N’hésitez pas à l’ajouter dans si vous préférez, ou, dans l’idéal, refactorisez la logique dans une méthode distincte.

Le code suivant illustre la nouvelle surcharge `UpdateProduct` dans la classe `ProductsBLL` :

[!code-vb[Main](customizing-the-data-modification-interface-vb/samples/sample1.vb)]

## <a name="step-2-crafting-the-editable-gridview"></a>Étape 2 : création du contrôle GridView modifiable

Une fois la surcharge de `UpdateProduct` ajoutée, nous sommes prêts à créer notre GridView modifiable. Ouvrez la page `CustomizedUI.aspx` dans le dossier `EditInsertDelete` et ajoutez un contrôle GridView au concepteur. Ensuite, créez un ObjectDataSource à partir de la balise active de GridView. Configurez l’ObjectDataSource pour récupérer les informations sur le produit via la méthode `GetProducts()` de la classe `ProductBLL` et pour mettre à jour les données du produit à l’aide de la surcharge `UpdateProduct` que nous venons de créer. Dans les onglets insérer et supprimer, sélectionnez (aucun) dans les listes déroulantes.

[![configurer ObjectDataSource pour utiliser la surcharge UpdateProduct que vous venez de créer](customizing-the-data-modification-interface-vb/_static/image5.png)](customizing-the-data-modification-interface-vb/_static/image4.png)

**Figure 2**: configurer ObjectDataSource pour utiliser la surcharge de `UpdateProduct` que vous venez de créer ([cliquez pour afficher l’image en taille réelle](customizing-the-data-modification-interface-vb/_static/image6.png))

Comme nous l’avons vu dans les didacticiels de modification des données, la syntaxe déclarative de l’ObjectDataSource créé par Visual Studio affecte la propriété `OldValuesParameterFormatString` à `original_{0}`. Cela ne fonctionne évidemment pas avec notre couche de logique métier, car nos méthodes ne s’attendent pas à ce que la valeur `ProductID` d’origine soit transmise. Par conséquent, comme nous l’avons fait dans les didacticiels précédents, prenez un moment pour supprimer cette assignation de propriété de la syntaxe déclarative ou, à la place, définissez la valeur de cette propriété sur `{0}`.

Après cette modification, le balisage déclaratif de ObjectDataSource doit ressembler à ce qui suit :

[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample2.aspx)]

Notez que la propriété `OldValuesParameterFormatString` a été supprimée et qu’il existe un `Parameter` dans la collection `UpdateParameters` pour chacun des paramètres d’entrée attendus par notre surcharge de `UpdateProduct`.

Alors que ObjectDataSource est configuré pour mettre à jour uniquement un sous-ensemble de valeurs de produit, le contrôle GridView affiche actuellement *tous* les champs de produit. Prenez un moment pour modifier le contrôle GridView afin que :

- Il comprend uniquement les `ProductName`, `SupplierName`, `CategoryName` BoundFields et `Discontinued` CheckBoxField
- Les champs `CategoryName` et `SupplierName` qui doivent apparaître avant (à gauche de) la `Discontinued` CheckBoxField
- La propriété `CategoryName` et `SupplierName` BoundFields' `HeaderText` est définie respectivement sur « Category » et sur « Supplier ».
- La modification de la prise en charge est activée (cochez la case Activer la modification dans la balise active de GridView)

Une fois ces modifications effectuées, le concepteur ressemble à la figure 3, avec la syntaxe déclarative du contrôle GridView indiquée ci-dessous.

[![supprimer les champs inutiles du contrôle GridView](customizing-the-data-modification-interface-vb/_static/image8.png)](customizing-the-data-modification-interface-vb/_static/image7.png)

**Figure 3**: supprimer les champs inutiles du contrôle GridView ([cliquez pour afficher l’image en taille réelle](customizing-the-data-modification-interface-vb/_static/image9.png))

[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample3.aspx)]

À ce stade, le comportement en lecture seule de GridView est terminé. Lorsque vous affichez les données, chaque produit est rendu sous la forme d’une ligne dans le GridView, affichant le nom, la catégorie, le fournisseur et l’État abandonné du produit.

[![l’interface en lecture seule du contrôle GridView est terminée](customizing-the-data-modification-interface-vb/_static/image11.png)](customizing-the-data-modification-interface-vb/_static/image10.png)

**Figure 4**: l’interface en lecture seule du contrôle GridView est terminée ([cliquez pour afficher l’image en taille réelle](customizing-the-data-modification-interface-vb/_static/image12.png))

> [!NOTE]
> Comme nous l’avons vu dans [une vue d’ensemble du didacticiel sur l’insertion, la mise à jour et la suppression des données](an-overview-of-inserting-updating-and-deleting-data-cs.md), il est primordial que l’état d’affichage de GridView s soit activé (comportement par défaut). Si vous affectez à la propriété GridView s `EnableViewState` la valeur `false`, vous risquez d’avoir des utilisateurs simultanés qui suppriment ou modifient des enregistrements de manière involontaire. Consultez [Avertissement : problème d’accès concurrentiel avec ASP.NET 2,0 GridViews/DetailsView/FormViews qui prennent en charge l’édition et/ou la suppression et dont l’état d’affichage est désactivé](http://scottonwriting.net/sowblog/posts/10054.aspx) pour plus d’informations.

## <a name="step-3-using-a-dropdownlist-for-the-category-and-supplier-editing-interfaces"></a>Étape 3 : utilisation d’un DropDownList pour les interfaces de modification des catégories et des fournisseurs

N’oubliez pas que l’objet `ProductsRow` contient des propriétés `CategoryID`, `CategoryName`, `SupplierID`et `SupplierName`, qui fournissent les valeurs d’ID de clé étrangère réelles dans la table de base de données `Products` et les valeurs `Name` correspondantes dans les tables `Categories` et `Suppliers`. Les `CategoryID` et `SupplierID` du `ProductRow`peuvent être lus et écrits, tandis que les propriétés `CategoryName` et `SupplierName` sont marquées en lecture seule.

En raison de l’État en lecture seule des propriétés `CategoryName` et `SupplierName`, la propriété `ReadOnly` de la BoundFields correspondante a la valeur `True`, ce qui empêche la modification de ces valeurs lors de la modification d’une ligne. Bien que nous puissions définir la propriété `ReadOnly` sur `False`, le rendu de la `CategoryName` et `SupplierName` BoundFields comme des zones de texte pendant la modification, une telle approche entraîne une exception lorsque l’utilisateur tente de mettre à jour le produit, car il n’y a aucune surcharge `UpdateProduct` qui prend en `CategoryName` et `SupplierName` entrées. En fait, nous ne voulons pas créer une telle surcharge pour deux raisons :

- La table `Products` n’a pas de champs `SupplierName` ou `CategoryName`, mais `SupplierID` et `CategoryID`. Par conséquent, nous souhaitons que notre méthode passe ces valeurs d’ID particulières, et non leurs valeurs de tables de recherche.
- Demander à l’utilisateur de taper le nom du fournisseur ou de la catégorie n’est pas idéal, car il demande à l’utilisateur de connaître les catégories et fournisseurs disponibles et leurs corrections orthographiques.

Les champs fournisseur et catégorie doivent afficher les noms des catégories et des fournisseurs en mode lecture seule (comme c’est le cas maintenant) et une liste déroulante des options applicables lors de leur modification. À l’aide d’une liste déroulante, l’utilisateur final peut rapidement voir les catégories et les fournisseurs qui peuvent être sélectionnés et peut plus facilement effectuer leur sélection.

Pour fournir ce comportement, nous devons convertir le `SupplierName` et `CategoryName` BoundFields en TemplateFields dont `ItemTemplate` émet les valeurs `SupplierName` et `CategoryName` et dont `EditItemTemplate` utilise un contrôle DropDownList pour répertorier les catégories et les fournisseurs disponibles.

## <a name="adding-thecategoriesandsuppliersdropdownlists"></a>Ajout de la`Categories`et`Suppliers`DropDownList

Commencez par convertir le `SupplierName` et `CategoryName` BoundFields en TemplateFields en cliquant sur le lien modifier les colonnes de la balise active de GridView. sélection de BoundField dans la liste en bas à gauche ; et en cliquant sur le lien « convertir ce champ en TemplateField ». Le processus de conversion créera un TemplateField avec un `ItemTemplate` et un `EditItemTemplate`, comme indiqué dans la syntaxe déclarative ci-dessous :

[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample4.aspx)]

Dans la mesure où BoundField était marqué en lecture seule, les `ItemTemplate` et `EditItemTemplate` contiennent un contrôle Web label dont la propriété `Text` est liée au champ de données applicable (`CategoryName`, dans la syntaxe ci-dessus). Nous devons modifier le `EditItemTemplate`, en remplaçant le contrôle Label Web par un contrôle DropDownList.

Comme nous l’avons vu dans les didacticiels précédents, le modèle peut être modifié par le biais du concepteur ou directement à partir de la syntaxe déclarative. Pour le modifier par le biais du concepteur, cliquez sur le lien modifier les modèles à partir de la balise active de GridView et choisissez de travailler avec le `EditItemTemplate`du champ de catégorie. Supprimez le contrôle Label Web et remplacez-le par un contrôle DropDownList, en affectant à la propriété ID de DropDownList la valeur `Categories`.

[![supprimer zone et ajouter un DropDownList à EditItemTemplate](customizing-the-data-modification-interface-vb/_static/image14.png)](customizing-the-data-modification-interface-vb/_static/image13.png)

**Figure 5**: supprimer le zone et ajouter un DropDownList au `EditItemTemplate` ([cliquez pour afficher l’image en taille réelle](customizing-the-data-modification-interface-vb/_static/image15.png))

Nous devons ensuite remplir le DropDownList avec les catégories disponibles. Cliquez sur le lien choisir la source de données à partir de la balise active de DropDownList et choisissez de créer un ObjectDataSource nommé `CategoriesDataSource`.

[![créer un nouveau contrôle ObjectDataSource nommé CategoriesDataSource](customizing-the-data-modification-interface-vb/_static/image17.png)](customizing-the-data-modification-interface-vb/_static/image16.png)

**Figure 6**: créer un contrôle ObjectDataSource nommé `CategoriesDataSource` ([cliquez pour afficher l’image en taille réelle](customizing-the-data-modification-interface-vb/_static/image18.png))

Pour que cet ObjectDataSource retourne toutes les catégories, liez-le à la méthode `GetCategories()` de la classe `CategoriesBLL`.

[![lier l’ObjectDataSource à la méthode GetCategories () de CategoriesBLL](customizing-the-data-modification-interface-vb/_static/image20.png)](customizing-the-data-modification-interface-vb/_static/image19.png)

**Figure 7**: lier l’ObjectDataSource à la méthode de `GetCategories()` de l' `CategoriesBLL`([cliquez pour afficher l’image en taille réelle](customizing-the-data-modification-interface-vb/_static/image21.png))

Enfin, configurez les paramètres DropDownList de telle sorte que le champ `CategoryName` s’affiche dans chaque `ListItem` DropDownList avec le champ `CategoryID` utilisé comme valeur.

[![avez affiché le champ CategoryName et le RéfCatégorie utilisé comme valeur](customizing-the-data-modification-interface-vb/_static/image23.png)](customizing-the-data-modification-interface-vb/_static/image22.png)

**Figure 8**: afficher le champ `CategoryName` et le `CategoryID` utilisé comme valeur ([cliquez pour afficher l’image en taille réelle](customizing-the-data-modification-interface-vb/_static/image24.png))

Après avoir apporté ces modifications, le balisage déclaratif pour le `EditItemTemplate` dans le `CategoryName` TemplateField inclura à la fois un DropDownList et un ObjectDataSource :

[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample5.aspx)]

> [!NOTE]
> L’état d’affichage du DropDownList du `EditItemTemplate` doit être activé. Nous ajouterons bientôt la syntaxe DataBinding à la syntaxe déclarative de DropDownList et aux commandes de liaison de liaison comme `Eval()` et `Bind()` ne peuvent apparaître que dans les contrôles dont l’état d’affichage est activé.

Répétez ces étapes pour ajouter un DropDownList nommé `Suppliers` au `EditItemTemplate``SupplierName` TemplateField. Cela implique l’ajout d’un DropDownList au `EditItemTemplate` et la création d’un autre ObjectDataSource. Toutefois, le ObjectDataSource de `Suppliers` DropDownList doit être configuré pour appeler la méthode `GetSuppliers()` de la classe `SuppliersBLL`. En outre, configurez le `Suppliers` DropDownList pour afficher le champ `CompanyName` et utilisez le champ `SupplierID` comme valeur pour ses `ListItem` s.

Après avoir ajouté les DropDownList aux deux `EditItemTemplate`, chargez la page dans un navigateur, puis cliquez sur le bouton modifier pour le produit Cajun assaisonnement du chef. Comme le montre la figure 9, les colonnes de catégorie et de fournisseur du produit sont rendues sous la forme de listes déroulantes contenant les catégories et fournisseurs disponibles. Toutefois, Notez que les *premiers* éléments dans les deux listes déroulantes sont sélectionnés par défaut (boissons pour la catégorie et les liquides exotiques en tant que fournisseur), même si le chef de l’assaisonnement de chef

[![le premier élément dans la liste déroulante est sélectionné par défaut](customizing-the-data-modification-interface-vb/_static/image26.png)](customizing-the-data-modification-interface-vb/_static/image25.png)

**Figure 9**: le premier élément dans les listes déroulantes est sélectionné par défaut ([cliquez pour afficher l’image en taille réelle](customizing-the-data-modification-interface-vb/_static/image27.png))

En outre, si vous cliquez sur mettre à jour, vous constaterez que les valeurs `CategoryID` et `SupplierID` du produit sont définies sur `NULL`. Ces deux comportements indésirables sont dus au fait que les DropDownList dans les `EditItemTemplate` s ne sont pas liés aux champs de données des données de produit sous-jacentes.

## <a name="binding-the-dropdownlists-to-thecategoryidandsupplieriddata-fields"></a>Liaison du DropDownList aux champs de données`CategoryID`et`SupplierID`

Pour que les listes déroulantes catégorie et fournisseur du produit modifié aient les valeurs appropriées et que ces valeurs soient renvoyées à la méthode `UpdateProduct` de la couche BLL en cliquant sur mettre à jour, nous devons lier les propriétés de `SelectedValue` DropDownList aux champs de données `CategoryID` et `SupplierID` à l’aide de la liaison de données bidirectionnelle. Pour ce faire, avec la `Categories` DropDownList, vous pouvez ajouter `SelectedValue='<%# Bind("CategoryID") %>'` directement à la syntaxe déclarative.

Vous pouvez également définir les DataBindings du DropDownList en modifiant le modèle par le biais du concepteur, puis en cliquant sur le lien modifier les DataBindings à partir de la balise active de DropDownList. Ensuite, indiquez que la propriété `SelectedValue` doit être liée au champ `CategoryID` à l’aide de la liaison de liaison bidirectionnelle (voir figure 10). Répétez le processus déclaratif ou concepteur pour lier le champ de données `SupplierID` au DropDownList `Suppliers`.

[![lier le CategoryID à la propriété SelectedValue de DropDownList à l’aide de la liaison de liaison bidirectionnelle](customizing-the-data-modification-interface-vb/_static/image29.png)](customizing-the-data-modification-interface-vb/_static/image28.png)

**Figure 10**: lier le `CategoryID` à la propriété de `SelectedValue` du DropDownList à l’aide de la liaison de liaison bidirectionnelle ([cliquez pour afficher l’image en taille réelle](customizing-the-data-modification-interface-vb/_static/image30.png))

Une fois que les liaisons ont été appliquées aux propriétés `SelectedValue` des deux DropDownList, les colonnes de la catégorie et du fournisseur du produit modifié auront par défaut les valeurs du produit actuel. Quand vous cliquez sur mettre à jour, les valeurs `CategoryID` et `SupplierID` de l’élément de liste déroulante sélectionné sont transmises à la méthode `UpdateProduct`. La figure 11 montre le didacticiel après l’ajout des instructions DataBinding. Notez la manière dont les éléments de la liste déroulante sélectionnée pour les dépassements de la taille de Cajun

[![la catégorie actuelle du produit modifié et les valeurs des fournisseurs sont sélectionnées par défaut](customizing-the-data-modification-interface-vb/_static/image32.png)](customizing-the-data-modification-interface-vb/_static/image31.png)

**Figure 11**: la catégorie actuelle du produit modifié et les valeurs du fournisseur sont sélectionnées par défaut ([cliquez pour afficher l’image en taille réelle](customizing-the-data-modification-interface-vb/_static/image33.png))

## <a name="handlingnullvalues"></a>Gestion des valeurs de`NULL`

Les colonnes `CategoryID` et `SupplierID` de la table `Products` peuvent être `NULL`, mais les DropDownList dans les `EditItemTemplate` s n’incluent pas d’élément de liste pour représenter une valeur `NULL`. Cela a deux conséquences :

- L’utilisateur ne peut pas utiliser notre interface pour changer la catégorie ou le fournisseur d’un produit d’une valeur non`NULL` en `NULL` un
- Si un produit possède un `NULL` `CategoryID` ou `SupplierID`, le fait de cliquer sur le bouton modifier entraîne une exception. Cela est dû au fait que la valeur `NULL` retournée par `CategoryID` (ou `SupplierID`) dans l’instruction `Bind()` n’est pas mappée à une valeur dans le DropDownList (le DropDownList lève une exception lorsque sa propriété `SelectedValue` est définie sur une valeur qui *n’est pas* dans sa collection d’éléments de liste).

Pour prendre en charge `NULL` valeurs de `CategoryID` et de `SupplierID`, nous devons ajouter un autre `ListItem` à chaque DropDownList pour représenter la valeur `NULL`. Dans le didacticiel [maître/détail avec un didacticiel DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) , nous avons vu comment ajouter un `ListItem` supplémentaire à un DropDownList lié aux données, qui impliquait la définition de la propriété `AppendDataBoundItems` de dropdownlist pour `True` et l’ajout manuel de `ListItem`supplémentaires. Dans ce didacticiel précédent, toutefois, nous avons ajouté un `ListItem` avec une `Value` de `-1`. La logique de liaison de liaison de ASP.NET, cependant, convertit automatiquement une chaîne vide en valeur `NULL` et vice-versa. Par conséquent, pour ce didacticiel, nous voulons que la `Value` du `ListItem`soit une chaîne vide.

Commencez par affecter à la propriété DropDownList' `AppendDataBoundItems` la valeur `True`. Ajoutez ensuite le `NULL` `ListItem` en ajoutant l’élément `<asp:ListItem>` suivant à chaque DropDownList afin que le balisage déclaratif ressemble à ceci :

[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample6.aspx)]

J’ai choisi d’utiliser « (aucun) » comme valeur de texte pour cette `ListItem`, mais vous pouvez également le remplacer par une chaîne vide si vous le souhaitez.

> [!NOTE]
> Comme nous l’avons vu dans le *filtrage maître/détail avec un didacticiel DropDownList* , `ListItem` s peuvent être ajoutés à un contrôle DropDownList via le concepteur en cliquant sur la propriété de `Items` du contrôle DropDownList dans le fenêtre Propriétés (qui affichera l’éditeur de collections `ListItem`). Toutefois, veillez à ajouter le `NULL` `ListItem` pour ce didacticiel par le biais de la syntaxe déclarative. Si vous utilisez l’éditeur de collections `ListItem`, la syntaxe déclarative générée omet le paramètre `Value` en même temps lorsqu’une chaîne vide est assignée, créant ainsi un balisage déclaratif comme : `<asp:ListItem>(None)</asp:ListItem>`. Bien que cela puisse paraître inoffensif, la valeur manquante fait que le DropDownList utilise la valeur de propriété `Text` à la place. Cela signifie que si ce `NULL` `ListItem` est sélectionné, la valeur « (aucune) » sera tentée pour être assignée au `CategoryID`, ce qui entraînera une exception. En définissant explicitement `Value=""`, une valeur de `NULL` est assignée à `CategoryID` lorsque la `NULL` `ListItem` est sélectionnée.

Répétez ces étapes pour la DropDownList des fournisseurs.

Avec cette `ListItem`supplémentaire, l’interface de modification peut désormais affecter des valeurs `NULL` aux champs `CategoryID` et `SupplierID` d’un produit, comme illustré à la figure 12.

[![choisir (aucun) pour affecter une valeur NULL pour la catégorie ou le fournisseur d’un produit](customizing-the-data-modification-interface-vb/_static/image35.png)](customizing-the-data-modification-interface-vb/_static/image34.png)

**Figure 12**: choisir (aucun) pour affecter une valeur de `NULL` pour la catégorie ou le fournisseur d’un produit ([cliquez pour afficher l’image en taille réelle](customizing-the-data-modification-interface-vb/_static/image36.png))

## <a name="step-4-using-radiobuttons-for-the-discontinued-status"></a>Étape 4 : utilisation des cases d’option pour l’État abandonné

Actuellement, le champ de données `Discontinued` des produits est exprimé à l’aide d’un CheckBoxField, qui affiche une case à cocher désactivée pour les lignes en lecture seule et une case à cocher activée pour la ligne en cours de modification. Bien que cette interface utilisateur soit souvent appropriée, nous pouvons la personnaliser si nécessaire à l’aide d’un TemplateField. Pour ce didacticiel, nous allons modifier le CheckBoxField en TemplateField qui utilise un contrôle RadioButtonList avec deux options « actif » et « supprimé » à partir desquelles l’utilisateur peut spécifier la valeur de `Discontinued` du produit.

Commencez par convertir le `Discontinued` CheckBoxField en TemplateField, ce qui créera un TemplateField avec un `ItemTemplate` et `EditItemTemplate`. Les deux modèles incluent une case à cocher avec sa propriété `Checked` liée au champ de données `Discontinued`, la seule différence entre les deux est que la propriété `Enabled` de la case à cocher de l' `ItemTemplate`est définie sur `False`.

Remplacez la case à cocher dans le `ItemTemplate` et `EditItemTemplate` par un contrôle RadioButtonList, en affectant à la fois les propriétés RadioButtonLists' `ID` la valeur `DiscontinuedChoice`. Ensuite, indiquez que le RadioButtonLists doit contenir deux cases d’option, l’une intitulée « active » avec la valeur « false » et l’autre intitulée « Discontinued » avec la valeur « true ». Pour ce faire, vous pouvez entrer les éléments `<asp:ListItem>` dans directement par le biais de la syntaxe déclarative ou utiliser l’éditeur de collections `ListItem` à partir du concepteur. La figure 13 montre l’éditeur de collections `ListItem` une fois que les deux options de case d’option ont été spécifiées.

[![ajouter des options actives et abandonnées à RadioButtonList](customizing-the-data-modification-interface-vb/_static/image38.png)](customizing-the-data-modification-interface-vb/_static/image37.png)

**Figure 13**: ajouter des options actives et supprimées au RadioButtonList ([cliquez pour afficher l’image en taille réelle](customizing-the-data-modification-interface-vb/_static/image39.png))

Étant donné que le RadioButtonList dans le `ItemTemplate` ne doit pas être modifiable, définissez sa propriété `Enabled` sur `False`, en laissant la propriété `Enabled` à `True` (valeur par défaut) pour RadioButtonList dans le `EditItemTemplate`. Les cases d’option de la ligne non modifiée sont ainsi en lecture seule, mais elles permettent à l’utilisateur de modifier les valeurs de RadioButton pour la ligne modifiée.

Nous devons toujours affecter les propriétés `SelectedValue` des contrôles RadioButtonList afin que la case d’option appropriée soit sélectionnée en fonction du champ de données `Discontinued` du produit. Comme avec les DropDownList examinés précédemment dans ce didacticiel, cette syntaxe DataBinding peut être ajoutée directement dans le balisage déclaratif ou via le lien modifier les DataBindings dans les balises actives RadioButtonLists.

Après avoir ajouté les deux RadioButtonLists et les avoir configurés, le balisage déclaratif de `Discontinued` TemplateField doit ressembler à ceci :

[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample7.aspx)]

Avec ces modifications, la colonne `Discontinued` a été transformée à partir d’une liste de cases à cocher en une liste de paires de cases d’option (voir figure 14). Lorsque vous modifiez un produit, la case d’option appropriée est sélectionnée et l’État abandonné du produit peut être mis à jour en sélectionnant la case d’option autre et en cliquant sur mettre à jour.

[![les cases à cocher abandonnées ont été remplacées par des paires de cases d’option](customizing-the-data-modification-interface-vb/_static/image41.png)](customizing-the-data-modification-interface-vb/_static/image40.png)

**Figure 14**: les cases à cocher abandonnées ont été remplacées par des paires de cases d’option ([cliquez pour afficher l’image en taille réelle](customizing-the-data-modification-interface-vb/_static/image42.png))

> [!NOTE]
> Étant donné que la colonne `Discontinued` de la base de données `Products` ne peut pas avoir de valeurs `NULL`, nous n’avons pas besoin de se soucier de capturer les informations `NULL` dans l’interface. Toutefois, si `Discontinued` colonne peut contenir `NULL` valeurs, nous souhaitons ajouter une troisième case d’option à la liste avec sa `Value` définie sur une chaîne vide (`Value=""`), comme avec la catégorie et le fournisseur DropDownList.

## <a name="summary"></a>Récapitulatif

Alors que BoundField et CheckBoxField restituent automatiquement des interfaces en lecture seule, d’édition et d’insertion, elles ne peuvent pas être personnalisées. Bien souvent, nous devrons personnaliser la modification ou l’insertion d’une interface, par exemple ajouter des contrôles de validation (comme nous l’avons vu dans le didacticiel précédent) ou en personnalisant l’interface utilisateur de la collecte de données (comme nous l’avons vu dans ce didacticiel). La personnalisation de l’interface avec un TemplateField peut être résumée dans les étapes suivantes :

1. Ajouter un TemplateField ou convertir un BoundField ou un CheckBoxField existant en TemplateField
2. Augmenter l’interface en fonction des besoins
3. Lier les champs de données appropriés aux contrôles Web récemment ajoutés à l’aide de la liaison de données bidirectionnelle

Outre l’utilisation des contrôles Web ASP.NET intégrés, vous pouvez également personnaliser les modèles d’un TemplateField avec des contrôles serveur compilés et des contrôles utilisateur personnalisés.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Précédent](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)
> [Suivant](implementing-optimistic-concurrency-vb.md)
