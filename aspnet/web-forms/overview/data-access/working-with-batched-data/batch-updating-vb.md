---
uid: web-forms/overview/data-access/working-with-batched-data/batch-updating-vb
title: Mise à jour par lots (VB) | Microsoft Docs
author: rick-anderson
description: Découvrez comment mettre à jour plusieurs enregistrements de base de données en une seule opération. Dans la couche d’interface utilisateur, nous créons un GridView dans lequel chaque ligne est modifiable. Dans les données...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: d191a204-d7ea-458d-b81c-0b9049ecb55f
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-updating-vb
msc.type: authoredcontent
ms.openlocfilehash: f0bb83b17585876dd6d28a5893a223cce15da31d
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74591237"
---
# <a name="batch-updating-vb"></a>Mise à jour par lots (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_64_VB.zip) ou [Télécharger le PDF](batch-updating-vb/_static/datatutorial64vb1.pdf)

> Découvrez comment mettre à jour plusieurs enregistrements de base de données en une seule opération. Dans la couche d’interface utilisateur, nous créons un GridView dans lequel chaque ligne est modifiable. Dans la couche d’accès aux données, nous encapsulons les multiples opérations de mise à jour dans une transaction pour vous assurer que toutes les mises à jour ont été effectuées correctement ou que toutes les mises à jour sont annulées.

## <a name="introduction"></a>Introduction

Dans le [didacticiel précédent](wrapping-database-modifications-within-a-transaction-vb.md) , nous avons vu comment étendre la couche d’accès aux données pour ajouter la prise en charge des transactions de base de données. Les transactions de base de données garantissent qu’une série d’instructions de modification de données sera traitée comme une seule opération atomique, ce qui garantit que toutes les modifications échoueront ou que toutes seront réussies. Grâce à cette fonctionnalité de couche DAL de bas niveau, nous sommes prêts à attirer notre attention sur la création d’interfaces de modification de données batch.

Dans ce didacticiel, nous allons créer un GridView dans lequel chaque ligne est modifiable (voir la figure 1). Étant donné que chaque ligne est rendue dans son interface de modification, il n’est pas nécessaire de disposer d’une colonne de boutons modifier, mettre à jour et annuler. Au lieu de cela, il existe deux boutons mettre à jour les produits dans la page qui, lorsque vous cliquez dessus, énumère les lignes GridView et met à jour la base de données.

[![chaque ligne dans le GridView est modifiable](batch-updating-vb/_static/image1.gif)](batch-updating-vb/_static/image1.png)

**Figure 1**: chaque ligne du contrôle GridView est modifiable ([cliquez pour afficher l’image en taille réelle](batch-updating-vb/_static/image2.png))

Commençons !

> [!NOTE]
> Dans le didacticiel sur l' [exécution des mises à jour par lot](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) , nous avons créé une interface de modification par lot à l’aide du contrôle DataList. Ce didacticiel diffère de l’ancien dans qui utilise un GridView et la mise à jour par lot est effectuée dans l’étendue d’une transaction. À la fin de ce didacticiel, je vous encourage à revenir au didacticiel précédent et à le mettre à jour pour utiliser les fonctionnalités liées aux transactions de base de données ajoutées dans le didacticiel précédent.

## <a name="examining-the-steps-for-making-all-gridview-rows-editable"></a>Examen des étapes pour rendre toutes les lignes GridView modifiables

Comme indiqué dans le didacticiel [vue d’ensemble de l’insertion, de la mise à jour et de la suppression des données](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) , le contrôle GridView offre une prise en charge intégrée de la modification de ses données sous-jacentes en fonction de chaque ligne. En interne, le GridView note la ligne modifiable par le biais de sa [propriété`EditIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.editindex(VS.80).aspx). Étant donné que le GridView est lié à sa source de données, il vérifie chaque ligne pour voir si l’index de la ligne est égal à la valeur de `EditIndex`. Si c’est le cas, les champs de ligne s sont rendus à l’aide de leurs interfaces de modification. Pour BoundFields, l’interface de modification est une zone de texte dont la propriété `Text` est assignée à la valeur du champ de données spécifié par la propriété BoundField s `DataField`. Pour TemplateFields, le `EditItemTemplate` est utilisé à la place du `ItemTemplate`.

Rappelez-vous que le flux de travail de modification démarre lorsqu’un utilisateur clique sur un bouton de modification de ligne. Cela provoque une publication (postback), définit la propriété GridView s `EditIndex` sur l’index de la ligne sur laquelle vous avez cliqué, puis relit les données à la grille. Quand l’utilisateur clique sur un bouton d’annulation de ligne, lors de la publication (postback), le `EditIndex` est défini sur une valeur de `-1` avant de relier les données à la grille. Étant donné que les lignes du contrôle GridView commencent l’indexation à zéro, la définition de `EditIndex` sur `-1` a pour effet d’afficher le contrôle GridView en mode lecture seule.

La propriété `EditIndex` fonctionne bien pour la modification par ligne, mais elle n’est pas conçue pour la modification par lot. Pour rendre le GridView entièrement modifiable, nous devons afficher chaque ligne à l’aide de son interface de modification. Le moyen le plus simple d’y parvenir consiste à créer où chaque champ modifiable est implémenté en tant que TemplateField avec son interface de modification définie dans la `ItemTemplate`.

Au cours des différentes étapes suivantes, nous allons créer un GridView entièrement modifiable. À l’étape 1, nous allons commencer par créer le GridView et son ObjectDataSource, puis convertir ses BoundFields et CheckBoxField en TemplateFields. Dans les étapes 2 et 3, nous allons déplacer les interfaces de modification de TemplateFields `EditItemTemplate` s vers leurs `ItemTemplate` s.

## <a name="step-1-displaying-product-information"></a>Étape 1 : affichage des informations sur le produit

Avant de vous soucier de la création d’un GridView dans lequel les lignes sont modifiables, commençons par afficher simplement les informations du produit. Ouvrez la page `BatchUpdate.aspx` dans le dossier `BatchData` et faites glisser un contrôle GridView de la boîte à outils vers le concepteur. Définissez le `ID` GridView s sur `ProductsGrid` et, à partir de sa balise active, choisissez de le lier à un nouvel ObjectDataSource nommé `ProductsDataSource`. Configurez l’ObjectDataSource pour récupérer ses données à partir de la méthode de `GetProducts` de la classe `ProductsBLL`.

[![configurer ObjectDataSource pour utiliser la classe ProductsBLL](batch-updating-vb/_static/image2.gif)](batch-updating-vb/_static/image3.png)

**Figure 2**: configurer ObjectDataSource pour utiliser la classe `ProductsBLL` ([cliquez pour afficher l’image en taille réelle](batch-updating-vb/_static/image4.png))

[![récupérer les données du produit à l’aide de la méthode GetProducts](batch-updating-vb/_static/image3.gif)](batch-updating-vb/_static/image5.png)

**Figure 3**: récupérer les données du produit à l’aide de la méthode `GetProducts` ([cliquez pour afficher l’image en taille réelle](batch-updating-vb/_static/image6.png))

À l’instar de GridView, les fonctionnalités de modification de ObjectDataSource sont conçues pour travailler par ligne. Pour mettre à jour un jeu d’enregistrements, nous devons écrire un peu de code dans la classe code-behind de la page ASP.NET pour traiter les données par lots et les transmettre à la couche BLL. Par conséquent, définissez les listes déroulantes dans les onglets mise à jour, insertion et suppression de ObjectDataSource s sur (aucune). Cliquez sur Terminer pour terminer l'Assistant.

[![définir les listes déroulantes dans les onglets mettre à jour, insérer et supprimer sur (aucun)](batch-updating-vb/_static/image4.gif)](batch-updating-vb/_static/image7.png)

**Figure 4**: définir les listes déroulantes des onglets mettre à jour, insérer et supprimer sur (aucun) ([cliquez pour afficher l’image en taille réelle](batch-updating-vb/_static/image8.png))

À l’issue de l’exécution de l’Assistant Configuration de la source de données, le balisage déclaratif s doit se présenter comme suit :

[!code-aspx[Main](batch-updating-vb/samples/sample1.aspx)]

L’exécution de l’Assistant Configuration de la source de données entraîne également la création par Visual Studio de BoundFields et d’un CheckBoxField pour les champs de données de produit dans le GridView. Pour ce didacticiel, autorisez uniquement l’utilisateur à afficher et à modifier le nom du produit, la catégorie, le prix et l’État abandonné. Supprimez tous les champs sauf les `ProductName`, `CategoryName`, `UnitPrice`et `Discontinued` et renommez les propriétés `HeaderText` des trois premiers champs respectivement en Product, Category et Price. Enfin, activez les cases à cocher Activer la pagination et activer le tri dans la balise active GridView s.

À ce stade, le contrôle GridView a trois BoundFields (`ProductName`, `CategoryName`et `UnitPrice`) et un CheckBoxField (`Discontinued`). Nous devons convertir ces quatre champs en TemplateFields, puis déplacer l’interface de modification du TemplateField s `EditItemTemplate` vers son `ItemTemplate`.

> [!NOTE]
> Nous avons exploré la création et la personnalisation de TemplateFields dans le didacticiel [Personnalisation de l’interface de modification des données](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) . Nous allons examiner les étapes de la conversion de BoundFields et CheckBoxField en TemplateFields et la définition de leurs interfaces de modification dans leurs `ItemTemplate` s, mais si vous êtes bloqué ou si vous avez besoin d’un actualisateur, Don t hésite à revenir à ce didacticiel précédent.

À partir de la balise active GridView s, cliquez sur le lien modifier les colonnes pour ouvrir la boîte de dialogue champs. Ensuite, sélectionnez chaque champ, puis cliquez sur le lien convertir ce champ en TemplateField.

![Convertir le BoundFields et le CheckBoxField existants en TemplateFields](batch-updating-vb/_static/image5.gif)

**Figure 5**: convertir le BoundFields et le CheckBoxField existants en TemplateFields

Maintenant que chaque champ est un TemplateField, nous sommes prêts à déplacer l’interface de modification du `EditItemTemplate` s vers le `ItemTemplate` s.

## <a name="step-2-creating-theproductnameunitprice-anddiscontinuedediting-interfaces"></a>Étape 2 : création des interfaces de modification`ProductName`,`UnitPrice`et`Discontinued`

La création des interfaces de modification `ProductName`, `UnitPrice`et `Discontinued` est la rubrique de cette étape et est assez simple, car chaque interface est déjà définie dans le `EditItemTemplate`TemplateField s. La création de l’interface de modification `CategoryName` est un peu plus complexe, car nous devons créer un DropDownList des catégories applicables. Cette `CategoryName` interface de modification est abordée à l’étape 3.

Commençons par le `ProductName` TemplateField. Cliquez sur le lien modifier les modèles dans la balise active de GridView s et explorez le `ProductName` TemplateField s `EditItemTemplate`. Sélectionnez la zone de texte, copiez-la dans le presse-papiers, puis collez-la dans le `ProductName` TemplateField s `ItemTemplate`. Modifiez la propriété TextBox s `ID` en `ProductName`.

Ensuite, ajoutez un RequiredFieldValidator au `ItemTemplate` pour vous assurer que l’utilisateur fournit une valeur pour chaque nom de produit. Affectez à la propriété `ControlToValidate` la valeur ProductName, la propriété `ErrorMessage` à vous devez fournir le nom du produit. et la propriété `Text` à \*. Après avoir apporté ces ajouts au `ItemTemplate`, votre écran doit ressembler à la figure 6.

[![le TemplateField ProductName comprend désormais une zone de texte et un contrôle RequiredFieldValidator](batch-updating-vb/_static/image6.gif)](batch-updating-vb/_static/image9.png)

**Figure 6**: le `ProductName` TemplateField comprend maintenant une zone de texte et un contrôle RequiredFieldValidator ([cliquez pour afficher l’image en taille réelle](batch-updating-vb/_static/image10.png))

Pour l’interface de modification `UnitPrice`, commencez par copier la zone de texte du `EditItemTemplate` vers le `ItemTemplate`. Ensuite, placez un $ devant la zone de texte et affectez à sa propriété `ID` la valeur UnitPrice et à sa propriété `Columns` la valeur 8.

Ajoutez également un CompareValidator au `UnitPrice` s `ItemTemplate` pour vous assurer que la valeur entrée par l’utilisateur est une valeur monétaire valide supérieure ou égale à $0,00. Affectez à la propriété validateur s `ControlToValidate` la valeur UnitPrice, sa propriété `ErrorMessage` à vous devez entrer une valeur de devise valide. Omettez les symboles monétaires., sa propriété `Text` à \*, sa propriété `Type` à `Currency`, sa propriété `Operator` à `GreaterThanEqual`, et sa propriété `ValueToCompare` à 0.

[![ajouter un CompareValidator pour vous assurer que le prix entré est une valeur monétaire non négative](batch-updating-vb/_static/image7.gif)](batch-updating-vb/_static/image11.png)

**Figure 7**: ajouter un CompareValidator pour vous assurer que le prix entré est une valeur monétaire non négative ([cliquez pour afficher l’image en plein écran](batch-updating-vb/_static/image12.png))

Pour le `Discontinued` TemplateField, vous pouvez utiliser la case à cocher déjà définie dans le `ItemTemplate`. Il vous suffit de définir son `ID` sur Discontinued et sa propriété `Enabled` sur `True`.

## <a name="step-3-creating-thecategorynameediting-interface"></a>Étape 3 : création de l’interface de modification`CategoryName`

L’interface de modification dans le `CategoryName` TemplateField s `EditItemTemplate` contient une zone de texte qui affiche la valeur du champ de données `CategoryName`. Nous devons remplacer ceci par un DropDownList qui répertorie les catégories possibles.

> [!NOTE]
> Le didacticiel [Personnalisation de l’interface de modification des données](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) contient une discussion plus complète et détaillée sur la personnalisation d’un modèle pour inclure un contrôle DropDownList par opposition à une zone de texte. Tandis que les étapes sont terminées, elles sont présentées tersely. Pour une analyse plus approfondie de la création et de la configuration des catégories DropDownList, reportez-vous au didacticiel [Personnalisation de l’interface de modification des données](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) .

Faites glisser un DropDownList de la boîte à outils vers le `CategoryName` TemplateField s `ItemTemplate`, en affectant à sa `ID` la valeur `Categories`. À ce stade, nous allons généralement définir la source de données DropDownList s par le biais de sa balise active, créant ainsi un ObjectDataSource. Toutefois, cette opération ajoute l’ObjectDataSource dans le `ItemTemplate`, ce qui entraîne la création d’une instance ObjectDataSource pour chaque ligne GridView. Au lieu de cela, nous allons créer l’ObjectDataSource en dehors de la TemplateFields de GridView s. Mettez fin à la modification du modèle et faites glisser un ObjectDataSource de la boîte à outils vers le concepteur sous le `ProductsDataSource` ObjectDataSource. Nommez le nouvel ObjectDataSource `CategoriesDataSource` et configurez-le pour qu’il utilise la méthode de `CategoriesBLL` classe s `GetCategories`.

[![configurer ObjectDataSource pour utiliser la classe CategoriesBLL](batch-updating-vb/_static/image8.gif)](batch-updating-vb/_static/image13.png)

**Figure 8**: configurer ObjectDataSource pour utiliser la classe `CategoriesBLL` ([cliquez pour afficher l’image en taille réelle](batch-updating-vb/_static/image14.png))

[![récupérer les données de catégorie à l’aide de la méthode GetCategories](batch-updating-vb/_static/image9.gif)](batch-updating-vb/_static/image15.png)

**Figure 9**: récupérer les données de catégorie à l’aide de la méthode `GetCategories` ([cliquez pour afficher l’image en taille réelle](batch-updating-vb/_static/image16.png))

Comme cet ObjectDataSource est utilisé uniquement pour récupérer des données, définissez les listes déroulantes dans les onglets mettre à jour et supprimer sur (aucun). Cliquez sur Terminer pour terminer l'Assistant.

[![définir les listes déroulantes dans les onglets mettre à jour et supprimer sur (aucun)](batch-updating-vb/_static/image10.gif)](batch-updating-vb/_static/image17.png)

**Figure 10**: définir les listes déroulantes des onglets mettre à jour et supprimer sur (aucun) ([cliquez pour afficher l’image en taille réelle](batch-updating-vb/_static/image18.png))

Une fois l’exécution de l’Assistant terminée, le balisage déclaratif `CategoriesDataSource` s doit se présenter comme suit :

[!code-aspx[Main](batch-updating-vb/samples/sample2.aspx)]

Une fois le `CategoriesDataSource` créé et configuré, revenez au `CategoryName` TemplateField s `ItemTemplate` et, à partir de la balise active DropDownList, cliquez sur le lien choisir la source de données. Dans l’Assistant Configuration de source de données, sélectionnez l’option `CategoriesDataSource` dans la première liste déroulante et choisissez de faire en sorte que `CategoryName` utilisé pour l’affichage et `CategoryID` comme valeur.

[![lier le DropDownList à CategoriesDataSource](batch-updating-vb/_static/image11.gif)](batch-updating-vb/_static/image19.png)

**Figure 11**: lier le DropDownList à l' `CategoriesDataSource` ([cliquez pour afficher l’image en taille réelle](batch-updating-vb/_static/image20.png))

À ce stade, le `Categories` DropDownList répertorie toutes les catégories, mais ne sélectionne pas encore automatiquement la catégorie appropriée pour le produit lié à la ligne GridView. Pour ce faire, nous devons définir le `SelectedValue` `Categories` DropDownList sur la valeur `CategoryID` du produit. Cliquez sur le lien modifier les DataBindings de la balise active DropDownList et associez la propriété `SelectedValue` avec le champ de données `CategoryID`, comme indiqué dans la figure 12.

![Lier la valeur RéfCatégorie du produit à la propriété SelectedValue s de DropDownList](batch-updating-vb/_static/image12.gif)

**Figure 12**: lier la valeur du `CategoryID` du produit à la propriété DropDownList s `SelectedValue`

Un dernier problème subsiste : si le produit n’a pas de valeur `CategoryID` spécifiée, l’instruction DataBinding sur `SelectedValue` entraîne une exception. Cela est dû au fait que le DropDownList contient uniquement des éléments pour les catégories et n’offre pas d’option pour les produits qui ont une valeur de base de données `NULL` pour `CategoryID`. Pour remédier à cela, définissez la propriété DropDownList s `AppendDataBoundItems` sur `True` et ajoutez un nouvel élément à la DropDownList, en omettant la propriété `Value` de la syntaxe déclarative. Autrement dit, assurez-vous que la syntaxe déclarative de `Categories` DropDownList ressemble à ce qui suit :

[!code-aspx[Main](batch-updating-vb/samples/sample3.aspx)]

Notez comment la `<asp:ListItem Value="">`--Select One--a son attribut `Value` explicitement défini sur une chaîne vide. Reportez-vous au didacticiel [Personnalisation de l’interface de modification des données](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) pour une discussion plus approfondie sur la raison pour laquelle cet élément DropDownList supplémentaire est nécessaire pour gérer le cas `NULL` et pourquoi l’affectation de la propriété `Value` à une chaîne vide est essentielle.

> [!NOTE]
> Il y a un problème de performances et d’évolutivité potentiel qui mérite d’être mentionné. Étant donné que chaque ligne a un DropDownList qui utilise le `CategoriesDataSource` comme source de données, la méthode `CategoriesBLL` classe s `GetCategories` sera appelée *n* fois par visite de page, où *n* est le nombre de lignes dans le GridView. Ces *n* appels à `GetCategories` aboutissent à *n* requêtes dans la base de données. Cet impact sur la base de données peut être atténué en mettant en cache les catégories retournées dans un cache par requête ou via la couche de mise en cache à l’aide d’une dépendance de mise en cache SQL ou d’une date d’expiration très brève. Pour plus d’informations sur l’option de mise en cache par demande, consultez [`HttpContext.Items` un magasin de cache par demande](http://aspnet.4guysfromrolla.com/articles/060904-1.aspx).

## <a name="step-4-completing-the-editing-interface"></a>Étape 4 : fin de l’interface de modification

Nous avons apporté un certain nombre de modifications aux modèles GridView s sans pause pour voir notre progression. Prenez un moment pour voir notre progression dans un navigateur. Comme le montre la figure 13, chaque ligne est rendue à l’aide de son `ItemTemplate`, qui contient l’interface de modification des cellules.

[![chaque ligne GridView est modifiable](batch-updating-vb/_static/image13.gif)](batch-updating-vb/_static/image21.png)

**Figure 13**: chaque ligne GridView est modifiable ([cliquez pour afficher l’image en taille réelle](batch-updating-vb/_static/image22.png))

Il y a quelques problèmes de mise en forme mineurs que nous devrions prendre en charge à ce stade. Tout d’abord, Notez que la valeur `UnitPrice` contient quatre points décimaux. Pour résoudre ce problème, revenez au `UnitPrice` TemplateField s `ItemTemplate` et, à partir de la balise active TextBox s, cliquez sur le lien modifier les DataBindings. Ensuite, spécifiez que la propriété `Text` doit être mise en forme en tant que nombre.

![Mettre en forme la propriété Text en tant que nombre](batch-updating-vb/_static/image14.gif)

**Figure 14**: mettre en forme la propriété `Text` en tant que nombre

Ensuite, laissez le centrer la case à cocher dans la colonne `Discontinued` (au lieu de l’aligner à gauche). Cliquez sur modifier les colonnes dans la balise active de GridView s et sélectionnez le `Discontinued` TemplateField dans la liste des champs dans le coin inférieur gauche. Explorez `ItemStyle` et affectez à la propriété `HorizontalAlign` la valeur Center, comme illustré dans la figure 15.

![Centrer la case à cocher abandonnée](batch-updating-vb/_static/image15.gif)

**Figure 15**: centrage de la case à cocher `Discontinued`

Ensuite, ajoutez un contrôle ValidationSummary à la page et affectez à sa propriété `ShowMessageBox` la valeur `True` et à sa propriété `ShowSummary` la valeur `False`. Ajoutez également les contrôles Web Button qui, lorsque vous cliquez dessus, mettent à jour les modifications des utilisateurs. En particulier, ajoutez deux contrôles Web Button, l’un au-dessus de GridView et l’autre au-dessous, en définissant les deux contrôles `Text` propriétés pour mettre à jour les produits.

Étant donné que l’interface de modification de GridView est définie dans son TemplateFields `ItemTemplate` s, les `EditItemTemplate` s sont superflues et peuvent être supprimées.

Après avoir effectué les modifications de mise en forme mentionnées ci-dessus, l’ajout des contrôles Button et la suppression des `EditItemTemplate` s inutiles, la syntaxe déclarative de votre page doit ressembler à ce qui suit :

[!code-aspx[Main](batch-updating-vb/samples/sample4.aspx)]

La figure 16 illustre cette page quand elle est affichée dans un navigateur après l’ajout du bouton contrôles Web et la mise en forme des modifications apportées.

[![la page comprend désormais deux boutons mettre à jour les produits](batch-updating-vb/_static/image16.gif)](batch-updating-vb/_static/image23.png)

**Figure 16**: la page comprend désormais deux boutons mettre à jour les produits ([cliquez pour afficher l’image en taille réelle](batch-updating-vb/_static/image24.png))

## <a name="step-5-updating-the-products"></a>Étape 5 : mise à jour des produits

Lorsqu’un utilisateur visite cette page, il effectue ses modifications, puis clique sur l’un des deux boutons mettre à jour les produits. À ce stade, nous devons enregistrer les valeurs entrées par l’utilisateur pour chaque ligne dans une instance de `ProductsDataTable`, puis les transmettre à une méthode BLL qui transmet ensuite cette `ProductsDataTable` instance à la méthode de `UpdateWithTransaction` DAL. La méthode `UpdateWithTransaction`, que nous avons créée dans le [didacticiel précédent](wrapping-database-modifications-within-a-transaction-vb.md), garantit que le lot de modifications sera mis à jour en tant qu’opération atomique.

Créez une méthode nommée `BatchUpdate` dans `BatchUpdate.aspx.vb` et ajoutez le code suivant :

[!code-vb[Main](batch-updating-vb/samples/sample5.vb)]

Cette méthode démarre en réobtenant tous les produits dans un `ProductsDataTable` via un appel à la méthode de `GetProducts` BLL s. Il énumère ensuite la collection `ProductGrid` GridView s [`Rows`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows(VS.80).aspx). La collection `Rows` contient une [instance`GridViewRow`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewrow.aspx) pour chaque ligne affichée dans le GridView. Étant donné que nous affichons au maximum dix lignes par page, la collection de `Rows` GridView ne comportera pas plus de dix éléments.

Pour chaque ligne, le `ProductID` est retiré de la collection `DataKeys` et le `ProductsRow` approprié est sélectionné dans le `ProductsDataTable`. Les quatre contrôles d’entrée TemplateField sont référencés par programmation et leurs valeurs affectées aux propriétés de l’instance `ProductsRow` s. Une fois que chaque valeur des lignes de GridView a été utilisée pour mettre à jour le `ProductsDataTable`, elle est transmise à la méthode de `UpdateWithTransaction` BLL s qui, comme nous l’avons vu dans le didacticiel précédent, appelle simplement la méthode DAL s `UpdateWithTransaction`.

L’algorithme de mise à jour par lot utilisé pour ce didacticiel met à jour chaque ligne de la `ProductsDataTable` qui correspond à une ligne dans le GridView, que les informations sur le produit aient été modifiées ou non. Bien que ces mises à jour aveugles ne soient généralement pas un problème de performance, elles peuvent entraîner des enregistrements superflus si vous renouvelez l’audit des modifications apportées à la table de base de données. Dans le didacticiel sur l' [exécution des mises à jour par lots](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) , nous avons exploré une interface de mise à jour par lot avec la DataList et ajouté du code qui permet de mettre à jour uniquement les enregistrements qui ont été modifiés par l’utilisateur. N’hésitez pas à utiliser les techniques d' [exécution des mises à jour par lots](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) pour mettre à jour le code de ce didacticiel, si vous le souhaitez.

> [!NOTE]
> Lors de la liaison de la source de données au contrôle GridView par le biais de sa balise active, Visual Studio attribue automatiquement la ou les valeurs de clé primaire de la source de données à la propriété GridView s `DataKeyNames`. Si vous n’avez pas lié ObjectDataSource au GridView via la balise active GridView s comme indiqué à l’étape 1, vous devez définir manuellement la propriété GridView s `DataKeyNames` sur ProductID pour accéder à la valeur `ProductID` pour chaque ligne via la collection `DataKeys`.

Le code utilisé dans `BatchUpdate` est semblable à celui utilisé dans les méthodes de `UpdateProduct` BLL s. la principale différence réside dans le fait que dans les méthodes de `UpdateProduct`, une seule instance de `ProductRow` est Récupérée de l’architecture. Le code qui affecte les propriétés de l' `ProductRow` est le même entre les méthodes `UpdateProducts` et le code de la boucle `For Each` dans `BatchUpdate`, comme le modèle global.

Pour suivre ce didacticiel, la méthode `BatchUpdate` doit être appelée lors d’un clic sur l’un des boutons des produits de mise à jour. Créez des gestionnaires d’événements pour les événements `Click` de ces deux contrôles Button et ajoutez le code suivant dans les gestionnaires d’événements :

[!code-vb[Main](batch-updating-vb/samples/sample6.vb)]

Tout d’abord, un appel est effectué pour `BatchUpdate`. Ensuite, la [propriété`ClientScript`](https://msdn.microsoft.com/library/system.web.ui.page.clientscript(VS.80).aspx) est utilisée pour injecter du code JavaScript qui affichera un MessageBox qui lit les produits qui ont été mis à jour.

Prenez quelques minutes pour tester ce code. Visitez `BatchUpdate.aspx` dans un navigateur, modifiez plusieurs lignes et cliquez sur l’un des boutons mettre à jour les produits. En supposant qu’il n’y ait pas d’erreurs de validation d’entrée, vous devez voir une MessageBox qui lit les produits qui ont été mis à jour. Pour vérifier l’atomicité de la mise à jour, envisagez d’ajouter une contrainte de `CHECK` aléatoire, comme celle qui interdit `UnitPrice` valeurs de 1234,56. Ensuite, à partir de `BatchUpdate.aspx`, modifiez un nombre d’enregistrements, en veillant à définir l’une des valeurs de `UnitPrice` du produit sur la valeur interdite (1234,56). Cela devrait entraîner une erreur lorsque vous cliquez sur mettre à jour les produits avec les autres modifications au cours de l’opération de traitement par lot restaurées à leurs valeurs d’origine.

## <a name="an-alternativebatchupdatemethod"></a>Une autre méthode de`BatchUpdate`

La méthode `BatchUpdate` que nous venons d’examiner récupère *tous* les produits de la méthode de `GetProducts` BLL s et met à jour uniquement les enregistrements qui s’affichent dans le GridView. Cette approche est idéale si le contrôle GridView n’utilise pas la pagination, mais dans le cas contraire, il peut y avoir des centaines, des milliers ou des dizaines de milliers de produits, mais seulement dix lignes dans le GridView. Dans ce cas, l’obtention de tous les produits de la base de données uniquement pour modifier 10 d’entre eux est moins que idéale.

Pour ces types de situations, envisagez d’utiliser la méthode `BatchUpdateAlternate` suivante à la place :

[!code-vb[Main](batch-updating-vb/samples/sample7.vb)]

`BatchMethodAlternate` commence par créer un `ProductsDataTable` vide nommé `products`. Il parcourt ensuite la collection de `Rows` GridView et, pour chaque ligne, obtient les informations de produit spécifiques à l’aide de la méthode de `GetProductByProductID(productID)` BLL s. Les propriétés de l’instance de `ProductsRow` Récupérée sont mises à jour de la même façon que `BatchUpdate`, mais après la mise à jour de la ligne, elle est importée dans le `ProductsDataTable` `products` via la méthode DataTable s [`ImportRow(DataRow)`](https://msdn.microsoft.com/library/system.data.datatable.importrow(VS.80).aspx).

Une fois la `For Each` boucle terminée, `products` contient une instance `ProductsRow` pour chaque ligne du contrôle GridView. Étant donné que chaque instance de `ProductsRow` a été ajoutée à la `products` (au lieu d’être mise à jour), si nous la passons en aveugle à la méthode `UpdateWithTransaction`, le `ProductsTableAdapter` essaiera d’insérer chacun des enregistrements dans la base de données. Au lieu de cela, nous devons spécifier que chacune de ces lignes a été modifiée (pas ajoutée).

Pour ce faire, vous pouvez ajouter une nouvelle méthode à la couche BLL nommée `UpdateProductsWithTransaction`. `UpdateProductsWithTransaction`, comme indiqué ci-dessous, définit la `RowState` de chacune des instances `ProductsRow` de l' `ProductsDataTable` sur `Modified`, puis passe la `ProductsDataTable` à la méthode de `UpdateWithTransaction` DAL s.

[!code-vb[Main](batch-updating-vb/samples/sample8.vb)]

## <a name="summary"></a>Récapitulatif

Le GridView fournit des fonctionnalités d’édition par ligne intégrées, mais ne prend pas en charge la création d’interfaces entièrement modifiables. Comme nous l’avons vu dans ce didacticiel, de telles interfaces sont possibles, mais nécessitent un peu de travail. Pour créer un contrôle GridView où chaque ligne est modifiable, nous devons convertir les champs GridView s en TemplateFields et définir l’interface de modification dans le `ItemTemplate` s. En outre, la mise à jour des contrôles Web des boutons tous les types doit être ajoutée à la page, séparément du contrôle GridView. Ces boutons `Click` gestionnaires d’événements doivent énumérer la collection de `Rows` GridView s, stocker les modifications dans un `ProductsDataTable`et transmettre les informations mises à jour dans la méthode BLL appropriée.

Dans le didacticiel suivant, nous allons apprendre à créer une interface pour la suppression par lots. En particulier, chaque ligne GridView inclut une case à cocher et, au lieu de mettre à jour tous les boutons de type, les boutons supprimer les lignes sélectionnées s’affichent.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel étaient Teresa Murphy et David SURU. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](wrapping-database-modifications-within-a-transaction-vb.md)
> [Suivant](batch-deleting-vb.md)
