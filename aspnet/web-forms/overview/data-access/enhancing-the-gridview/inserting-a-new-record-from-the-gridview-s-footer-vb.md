---
uid: web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-vb
title: Insertion d’un nouvel enregistrement à partir du pied de page du GridView (VB) | Microsoft Docs
author: rick-anderson
description: Bien que le contrôle GridView ne fournisse pas de prise en charge intégrée pour l’insertion d’un nouvel enregistrement de données, ce didacticiel montre comment augmenter le GridView pour inclure un...
ms.author: riande
ms.date: 03/06/2007
ms.assetid: 528acc48-f20c-4b4e-aa16-4cc02f068ebb
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-vb
msc.type: authoredcontent
ms.openlocfilehash: 67ef370a90bc843f5c2da80bb43c8ef8de216b51
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74631777"
---
# <a name="inserting-a-new-record-from-the-gridviews-footer-vb"></a>Insertion d’un nouvel enregistrement à partir du pied de page d’un contrôle GridView (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_53_VB.exe) ou [Télécharger le PDF](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/datatutorial53vb1.pdf)

> Bien que le contrôle GridView ne fournisse pas de prise en charge intégrée pour l’insertion d’un nouvel enregistrement de données, ce didacticiel montre comment augmenter le GridView pour inclure une interface d’insertion.

## <a name="introduction"></a>Introduction

Comme indiqué dans le didacticiel [vue d’ensemble de l’insertion, de la mise à jour et de la suppression des données](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) , les contrôles Web GridView, DetailsView et FormView incluent chacun des fonctionnalités de modification de données intégrées. Lorsqu’ils sont utilisés avec des contrôles de source de données déclaratifs, ces trois contrôles Web peuvent être rapidement et facilement configurés pour modifier les données et dans les scénarios sans avoir à écrire une seule ligne de code. Malheureusement, seuls les contrôles DetailsView et FormView offrent des fonctionnalités intégrées d’insertion, de modification et de suppression. Le GridView offre uniquement la prise en charge de la modification et de la suppression. Toutefois, avec un petit coude de graisse, nous pouvons augmenter le GridView pour inclure une interface d’insertion.

Lors de l’ajout de fonctionnalités d’insertion au GridView, nous sommes chargés de déterminer comment les nouveaux enregistrements seront ajoutés, de créer l’interface d’insertion et d’écrire le code permettant d’insérer le nouvel enregistrement. Dans ce didacticiel, nous allons ajouter l’interface d’insertion à la ligne de pied de page de GridView s (voir figure 1). La cellule de pied de page de chaque colonne comprend l’élément d’interface utilisateur de la collection de données approprié (une zone de texte pour le nom du produit s, un contrôle DropDownList pour le fournisseur, etc.). Nous avons également besoin d’une colonne pour un bouton Ajouter qui, lorsque vous cliquez dessus, entraîne une publication (postback) et insère un nouvel enregistrement dans la table `Products` à l’aide des valeurs fournies dans la ligne de pied de page.

[![la ligne de pied de page fournit une interface pour l’ajout de nouveaux produits](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image1.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image1.png)

**Figure 1**: la ligne de pied de page fournit une interface pour l’ajout de nouveaux produits ([cliquez pour afficher l’image en taille réelle](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image2.png))

## <a name="step-1-displaying-product-information-in-a-gridview"></a>Étape 1 : affichage des informations sur le produit dans un GridView

Avant de nous soucier de la création de l’interface d’insertion dans le pied de page de GridView s, commençons par se concentrer sur l’ajout d’un GridView à la page qui répertorie les produits de la base de données. Commencez par ouvrir la page `InsertThroughFooter.aspx` dans le dossier `EnhancedGridView` et faites glisser un contrôle GridView de la boîte à outils vers le concepteur, en affectant à la propriété GridView s `ID` la valeur `Products`. Ensuite, utilisez la balise active GridView s pour la lier à un nouvel ObjectDataSource nommé `ProductsDataSource`.

[![créer un ObjectDataSource nommé ProductsDataSource](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image2.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image3.png)

**Figure 2**: créer un ObjectDataSource nommé `ProductsDataSource` ([cliquez pour afficher l’image en taille réelle](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image4.png))

Configurez le ObjectDataSource pour utiliser la méthode de `GetProducts()` de la classe `ProductsBLL` pour récupérer les informations sur le produit. Pour ce didacticiel, nous allons vous concentrer strictement sur l’ajout de fonctionnalités d’insertion et ne vous inquiétez pas de la modification et de la suppression. Par conséquent, assurez-vous que la liste déroulante de l’onglet Insertion est définie sur `AddProduct()` et que les listes déroulantes des onglets mettre à jour et supprimer ont la valeur (aucune).

[![mapper la méthode AddProduct à la méthode d’insertion () de ObjectDataSource](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image3.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image5.png)

**Figure 3**: mapper la méthode `AddProduct` à la méthode d' `Insert()` ObjectDataSource s ([cliquez pour afficher l’image en taille réelle](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image6.png))

[![définir les listes déroulantes mettre à jour et supprimer les onglets sur (aucune)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image4.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image7.png)

**Figure 4**: définir les listes déroulantes mettre à jour et supprimer les onglets sur (aucun) ([cliquez pour afficher l’image en taille réelle](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image8.png))

À la fin de l’Assistant Configuration de la source de données ObjectDataSource s, Visual Studio ajoute automatiquement des champs au GridView pour chacun des champs de données correspondants. Pour le moment, laissez tous les champs ajoutés par Visual Studio. Plus loin dans ce didacticiel, nous allons revenir et supprimer certains champs dont les valeurs ne doivent pas être spécifiées lors de l’ajout d’un nouvel enregistrement.

Étant donné qu’il y a près de 80 produits dans la base de données, un utilisateur devra faire défiler jusqu’au bas de la page Web pour ajouter un nouvel enregistrement. Par conséquent, activez la pagination pour rendre l’interface d’insertion plus visible et accessible. Pour activer la pagination, activez simplement la case à cocher Activer la pagination dans la balise active GridView s.

À ce stade, les balises déclaratives GridView et ObjectDataSource doivent ressembler à ce qui suit :

[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample1.aspx)]

[![tous les champs de données de produit sont affichés dans un contrôle GridView paginé](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image5.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image9.png)

**Figure 5**: tous les champs de données de produit s’affichent dans un contrôle GridView paginé ([cliquez pour afficher l’image en taille réelle](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image10.png))

## <a name="step-2-adding-a-footer-row"></a>Étape 2 : ajout d’une ligne de pied de page

Avec ses lignes d’en-tête et de données, le contrôle GridView comprend une ligne de pied de page. Les lignes d’en-tête et de pied de page s’affichent en fonction des valeurs des propriétés GridView s [`ShowHeader`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.showheader.aspx) et [`ShowFooter`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.showfooter.aspx) . Pour afficher la ligne de pied de page, il vous suffit de définir la propriété `ShowFooter` sur `True`. Comme illustré à la figure 6, la définition de la propriété `ShowFooter` sur `True` ajoute une ligne de pied de page à la grille.

[![pour afficher la ligne de pied de page, affectez la valeur true à ShowFooter](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image6.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image11.png)

**Figure 6**: pour afficher la ligne de pied de page, définissez `ShowFooter` sur `True` ([cliquez pour afficher l’image en taille réelle](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image12.png))

Notez que la ligne de pied de page a une couleur d’arrière-plan rouge foncé. Cela est dû au thème DataWebControls que nous avons créé et appliqué à toutes les pages dans le didacticiel [affichage des données avec ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) . Plus précisément, le fichier `GridView.skin` configure la propriété `FooterStyle` de manière à ce que utilise la classe CSS `FooterStyle`. La classe `FooterStyle` est définie dans `Styles.css` comme suit :

[!code-css[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample2.css)]

> [!NOTE]
> Nous avons exploré à l’aide de la ligne de pied de page de GridView s dans les didacticiels précédents. Si nécessaire, reportez-vous à la page [affichage des informations récapitulatives dans le didacticiel de pied de page du GridView](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md) pour un actualisateur.

Après avoir défini la propriété `ShowFooter` sur `True`, prenez un moment pour afficher la sortie dans un navigateur. Actuellement, la ligne de pied de page ne contient pas de texte ou de contrôles Web. À l’étape 3, nous allons modifier le pied de page pour chaque champ GridView afin qu’il comprenne l’interface d’insertion appropriée.

[![la ligne de pied de page vide est affichée au-dessus des contrôles d’interface de pagination](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image7.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image13.png)

**Figure 7**: la ligne de pied de page vide est affichée au-dessus des contrôles de l’interface de pagination ([cliquez pour afficher l’image en taille réelle](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image14.png))

## <a name="step-3-customizing-the-footer-row"></a>Étape 3 : personnalisation de la ligne de pied de page

De retour dans le didacticiel [à l’aide de TemplateFields dans le contrôle GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) , nous avons vu comment personnaliser de manière radicale l’affichage d’une colonne GridView particulière à l’aide de TemplateFields (par opposition à BoundFields ou CheckBoxFields). dans [Personnalisation de l’interface de modification des données](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) , nous avons examiné l’utilisation de TemplateFields pour personnaliser l’interface de modification dans un GridView. Rappelez-vous qu’un TemplateField est composé d’un certain nombre de modèles qui définissent la combinaison des balises, des contrôles Web et de la syntaxe de liaison de liaison utilisés pour certains types de lignes. Le `ItemTemplate`, par exemple, spécifie le modèle utilisé pour les lignes en lecture seule, tandis que l' `EditItemTemplate` définit le modèle pour la ligne modifiable.

Avec les `ItemTemplate` et `EditItemTemplate`, TemplateField comprend également un `FooterTemplate` qui spécifie le contenu de la ligne de pied de page. Par conséquent, nous pouvons ajouter les contrôles Web requis pour chaque champ, en insérant l’interface dans le `FooterTemplate`. Pour démarrer, convertissez tous les champs du contrôle GridView en TemplateFields. Pour ce faire, cliquez sur le lien modifier les colonnes dans la balise active GridView s, sélectionnez chaque champ dans le coin inférieur gauche, puis cliquez sur le lien convertir ce champ en TemplateField.

![Convertir chaque champ en TemplateField](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image8.gif)

**Figure 8**: convertir chaque champ en TemplateField

Si vous cliquez sur le bouton convertir ce champ en TemplateField, le type de champ actuel devient un TemplateField équivalent. Par exemple, chaque BoundField est remplacé par un TemplateField avec un `ItemTemplate` qui contient une étiquette qui affiche le champ de données correspondant et un `EditItemTemplate` qui affiche le champ de données dans une zone de texte. La `ProductName` BoundField a été convertie dans le balisage TemplateField suivant :

[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample3.aspx)]

De même, le `Discontinued` CheckBoxField a été converti en TemplateField dont `ItemTemplate` et `EditItemTemplate` contiennent un contrôle Web CheckBox (avec la case à cocher `ItemTemplate` s désactivée). Le `ProductID` BoundField en lecture seule a été converti en TemplateField avec un contrôle Label dans les `ItemTemplate` et `EditItemTemplate`. En résumé, la conversion d’un champ GridView existant en TemplateField est un moyen simple et rapide de basculer vers le TemplateField plus personnalisable, sans perdre aucune des fonctionnalités existantes des champs.

Étant donné que le GridView avec lequel nous travaillons avec ne prend pas en charge la modification, n’hésitez pas à supprimer le `EditItemTemplate` de chaque TemplateField, en laissant juste le `ItemTemplate`. Après cela, votre balisage déclaratif s doit ressembler à ce qui suit :

[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample4.aspx)]

Maintenant que chaque champ GridView a été converti en TemplateField, nous pouvons entrer l’interface d’insertion appropriée dans chaque champ s `FooterTemplate`. Certains champs ne disposent pas d’une interface d’insertion (`ProductID`, par exemple); d’autres peuvent varier dans les contrôles Web utilisés pour collecter les informations relatives aux nouveaux produits.

Pour créer l’interface d’édition, choisissez le lien modifier les modèles dans la balise active de GridView s. Ensuite, dans la liste déroulante, sélectionnez les `FooterTemplate` champs appropriés, puis faites glisser le contrôle approprié de la boîte à outils vers le concepteur.

[![ajouter l’interface d’insertion appropriée à chaque FooterTemplate Field](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image9.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image15.png)

**Figure 9**: ajouter l’interface d’insertion appropriée à chaque champ s `FooterTemplate` ([cliquez pour afficher l’image en taille réelle](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image16.png))

La liste à puces suivante énumère les champs GridView, en spécifiant l’interface d’insertion à ajouter :

- `ProductID` aucun.
- `ProductName` ajouter une zone de texte et définir ses `ID` sur `NewProductName`. Ajoutez également un contrôle RequiredFieldValidator pour vous assurer que l’utilisateur entre une valeur pour le nouveau nom du produit.
- `SupplierID` aucun.
- `CategoryID` aucun.
- `QuantityPerUnit` ajouter une zone de texte, en affectant à son `ID` la valeur `NewQuantityPerUnit`.
- `UnitPrice` ajouter une zone de texte nommée `NewUnitPrice` et un CompareValidator qui garantit que la valeur entrée est une valeur monétaire supérieure ou égale à zéro.
- `UnitsInStock` utiliser une zone de texte dont `ID` a la valeur `NewUnitsInStock`. Incluez un CompareValidator qui garantit que la valeur entrée est une valeur entière supérieure ou égale à zéro.
- `UnitsOnOrder` utiliser une zone de texte dont `ID` a la valeur `NewUnitsOnOrder`. Incluez un CompareValidator qui garantit que la valeur entrée est une valeur entière supérieure ou égale à zéro.
- `ReorderLevel` utiliser une zone de texte dont `ID` a la valeur `NewReorderLevel`. Incluez un CompareValidator qui garantit que la valeur entrée est une valeur entière supérieure ou égale à zéro.
- `Discontinued` ajoutez une case à cocher, en affectant à sa `ID` la valeur `NewDiscontinued`.
- `CategoryName` ajouter un DropDownList et définir ses `ID` sur `NewCategoryID`. Liez-le à un nouvel ObjectDataSource nommé `CategoriesDataSource` et configurez-le pour qu’il utilise la méthode de `CategoriesBLL` classe s `GetCategories()`. Que les `ListItem` DropDownList affichent le champ de données `CategoryName`, en utilisant le champ de données `CategoryID` comme valeurs.
- `SupplierName` ajouter un DropDownList et définir ses `ID` sur `NewSupplierID`. Liez-le à un nouvel ObjectDataSource nommé `SuppliersDataSource` et configurez-le pour qu’il utilise la méthode de `SuppliersBLL` classe s `GetSuppliers()`. Que les `ListItem` DropDownList affichent le champ de données `CompanyName`, en utilisant le champ de données `SupplierID` comme valeurs.

Pour chacun des contrôles de validation, supprimez la propriété `ForeColor` afin que la couleur d’arrière-plan de la `FooterStyle` classe CSS White s soit utilisée à la place du rouge par défaut. Utilisez également la propriété `ErrorMessage` pour obtenir une description détaillée, mais affectez à la propriété `Text` la valeur un astérisque. Pour éviter que le texte du contrôle de validation ne provoque l’encapsulation de l’interface d’insertion sur deux lignes, affectez la valeur false à la propriété `FooterStyle` s `Wrap` pour chaque `FooterTemplate` s qui utilise un contrôle de validation. Enfin, ajoutez un contrôle ValidationSummary sous le GridView et définissez sa propriété `ShowMessageBox` sur `True` et sa propriété `ShowSummary` sur `False`.

Lorsque vous ajoutez un nouveau produit, nous devons fournir les `CategoryID` et les `SupplierID`. Ces informations sont capturées via le DropDownList dans les cellules de pied de page pour les champs `CategoryName` et `SupplierName`. J’ai choisi d’utiliser ces champs plutôt que les `CategoryID` et `SupplierID` TemplateFields, car dans les lignes de données de la grille, l’utilisateur est susceptible de voir les noms des catégories et des fournisseurs plutôt que leurs valeurs d’ID. Étant donné que les valeurs `CategoryID` et `SupplierID` sont maintenant capturées dans les interfaces d’insertion de `CategoryName` et de `SupplierName` Fields, nous pouvons supprimer les `CategoryID` et `SupplierID` TemplateFields du contrôle GridView.

De même, le `ProductID` n’est pas utilisé lors de l’ajout d’un nouveau produit, donc le `ProductID` TemplateField peut également être supprimé. Toutefois, laissez le champ `ProductID` dans la grille. Outre les contrôles TextBox, les DropDownList, les cases à cocher et les contrôles de validation qui composent l’interface d’insertion, nous aurons également besoin d’un bouton Ajouter qui, lorsque vous cliquez dessus, exécute la logique pour ajouter le nouveau produit à la base de données. À l’étape 4, nous allons inclure un bouton Ajouter dans l’interface d’insertion de la `FooterTemplate``ProductID` TemplateField s.

N’hésitez pas à améliorer l’apparence des différents champs GridView. Par exemple, vous pouvez mettre en forme les valeurs de `UnitPrice` comme une devise, aligner à droite les champs `UnitsInStock`, `UnitsOnOrder`et `ReorderLevel`, puis mettre à jour les valeurs de `HeaderText` pour TemplateFields.

Après avoir créé l’interface d’insertion des interfaces dans le `FooterTemplate` s, en supprimant les `SupplierID`et `CategoryID` TemplateFields et en améliorant l’esthétique de la grille par le biais de la mise en forme et de l’alignement du TemplateFields, votre balisage déclaratif s doit ressembler à ce qui suit :

[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample5.aspx)]

En cas d’affichage dans un navigateur, la ligne de pied de page de GridView s comprend désormais l’interface d’insertion terminée (voir figure 10). À ce stade, l’interface d’insertion n’inclut pas de moyen permettant à l’utilisateur d’indiquer qu’il a entré les données pour le nouveau produit et souhaite insérer un nouvel enregistrement dans la base de données. En outre, nous avons encore pu traiter la manière dont les données entrées dans le pied de page seront traduites en un nouvel enregistrement dans la base de données `Products`. À l’étape 4, nous verrons comment inclure un bouton Ajouter à l’interface d’insertion et comment exécuter du code sur la publication (postback) quand l’utilisateur clique dessus. L’étape 5 montre comment insérer un nouvel enregistrement à l’aide des données du pied de page.

[![le pied de page GridView fournit une interface pour l’ajout d’un nouvel enregistrement](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image10.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image17.png)

**Figure 10**: le pied de page GridView fournit une interface permettant d’ajouter un nouvel enregistrement ([cliquez pour afficher l’image en taille réelle](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image18.png))

## <a name="step-4-including-an-add-button-in-the-inserting-interface"></a>Étape 4 : inclure un bouton Ajouter dans l’interface d’insertion

Nous devons inclure un bouton ajouter quelque part dans l’interface d’insertion, car la ligne de pied de page qui insère l’interface ne dispose pas actuellement des moyens permettant à l’utilisateur d’indiquer qu’il a terminé d’entrer les nouvelles informations du produit. Cela peut être placé dans l’un des `FooterTemplate` existants ou nous pouvons ajouter une nouvelle colonne à cette grille à cet effet. Pour ce didacticiel, nous allons placer le bouton Ajouter dans le `ProductID` TemplateField s `FooterTemplate`.

Dans le concepteur, cliquez sur le lien modifier les modèles dans la balise active GridView s, puis sélectionnez les `ProductID` champ s `FooterTemplate` dans la liste déroulante. Ajoutez un contrôle Web Button (ou LinkButton ou ImageButton, si vous préférez) au modèle, en définissant son ID sur `AddProduct`, son `CommandName` à insérer et sa propriété `Text` à ajouter, comme illustré à la figure 11.

[![Placez le bouton Ajouter dans ProductID TemplateField s FooterTemplate](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image11.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image19.png)

**Figure 11**: placer le bouton Ajouter dans le `ProductID` TemplateField s `FooterTemplate` ([cliquez pour afficher l’image en taille réelle](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image20.png))

Une fois que vous avez inclus le bouton Ajouter, testez la page dans un navigateur. Notez que lorsque vous cliquez sur le bouton Ajouter avec des données non valides dans l’interface d’insertion, la publication est court-circuitée et le contrôle ValidationSummary indique les données non valides (voir figure 12). Lorsque les données appropriées sont entrées, le fait de cliquer sur le bouton Ajouter entraîne une publication (postback). Aucun enregistrement n’est toutefois ajouté à la base de données. Nous devons écrire un peu de code pour exécuter réellement l’insertion.

[![le bouton ajouter de la publication (postback) est court-circuit Si des données ne sont pas valides dans l’interface d’insertion](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image12.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image21.png)

**Figure 12**: le bouton Ajouter une publication (postback) est court-circuit Si des données ne sont pas valides dans l’interface d’insertion ([cliquez pour afficher l’image en plein écran](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image22.png))

> [!NOTE]
> Les contrôles de validation de l’interface d’insertion n’ont pas été assignés à un groupe de validation. Cela fonctionne très bien tant que l’interface d’insertion est le seul ensemble de contrôles de validation sur la page. Si, toutefois, il existe d’autres contrôles de validation sur la page (tels que les contrôles de validation dans l’interface de modification de la grille), les contrôles de validation dans les propriétés d’insertion d’interface et d’ajout de bouton `ValidationGroup` doivent être affectés de la même valeur afin d’associer ces contrôles à un groupe de validation particulier. Pour plus d’informations sur le partitionnement des contrôles de validation et des boutons d’une page dans des groupes de validation, consultez [discroiser les contrôles de validation dans ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx) .

## <a name="step-5-inserting-a-new-record-into-theproductstable"></a>Étape 5 : insertion d’un nouvel enregistrement dans la table`Products`

Lors de l’utilisation des fonctionnalités d’édition intégrées du GridView, le GridView gère automatiquement tout le travail nécessaire pour effectuer la mise à jour. En particulier, lorsque l’utilisateur clique sur le bouton mettre à jour, il copie les valeurs entrées à partir de l’interface de modification vers les paramètres de la collection ObjectDataSource s `UpdateParameters` et lance la mise à jour en appelant la méthode ObjectDataSource s `Update()`. Étant donné que le contrôle GridView ne fournit pas ces fonctionnalités intégrées pour l’insertion, nous devons implémenter le code qui appelle la méthode ObjectDataSource s `Insert()` et copie les valeurs de l’interface d’insertion dans la collection de `InsertParameters` ObjectDataSource s.

Cette logique d’insertion doit être exécutée une fois que vous avez cliqué sur le bouton Ajouter. Comme indiqué dans les [boutons ajouter et répondre à dans un didacticiel GridView](../custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb.md) , chaque fois qu’un utilisateur clique sur un bouton, LinkButton ou ImageButton dans un GridView, l’événement gridview s `RowCommand` se déclenche lors de la publication (postback). Cet événement se déclenche si le bouton, LinkButton ou ImageButton a été ajouté explicitement, tel que le bouton Ajouter dans la ligne de pied de page ou s’il a été ajouté automatiquement par le GridView (tel que le LinkButtons en haut de chaque colonne lorsque l’option Activer le tri est sélectionnée ou la valeur LinkButtons dans l’interface de pagination lorsque l’option Activer la pagination est sélectionnée).

Par conséquent, pour répondre à l’utilisateur qui clique sur le bouton Ajouter, nous devons créer un gestionnaire d’événements pour l’événement `RowCommand` GridView s. Étant donné que cet événement est déclenché chaque fois que l’utilisateur clique sur *un* bouton, LinkButton ou ImageButton dans le GridView, il est vital que nous procédions uniquement à la logique d’insertion si la propriété `CommandName` transmise dans le gestionnaire d’événements est mappée à la valeur `CommandName` du bouton Ajouter (insérer). En outre, nous devons également continuer uniquement si les contrôles de validation signalent des données valides. Pour ce faire, créez un gestionnaire d’événements pour l’événement `RowCommand` à l’aide du code suivant :

[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample6.vb)]

> [!NOTE]
> Vous vous demandez peut-être pourquoi le gestionnaire d’événements vérifie la propriété `Page.IsValid`. Après tout, la publication ne sera-t-elle pas supprimée si des données non valides sont fournies dans l’interface d’insertion ? Cette hypothèse est correcte tant que l’utilisateur n’a pas désactivé JavaScript ou a pris les mesures nécessaires pour contourner la logique de validation côté client. En bref, l’un ne doit jamais reposer strictement sur la validation côté client ; une vérification de validité côté serveur doit toujours être effectuée avant l’utilisation des données.

À l’étape 1, nous avons créé le `ProductsDataSource` ObjectDataSource de telle sorte que sa méthode `Insert()` soit mappée à la méthode de `AddProduct` de la classe `ProductsBLL`. Pour insérer le nouvel enregistrement dans la table `Products`, nous pouvons simplement invoquer la méthode d' `Insert()` ObjectDataSource s :

[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample7.vb)]

Maintenant que la méthode `Insert()` a été appelée, il ne reste plus qu’à copier les valeurs de l’interface d’insertion vers les paramètres passés à la méthode `AddProduct` de la classe `ProductsBLL`. Comme nous l’avons vu dans le didacticiel [examen des événements associés à l’insertion, à la mise à jour et](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) à la suppression, vous pouvez le faire via l’événement ObjectDataSource s `Inserting`. Dans le `Inserting` événement, nous devons référencer par programmation les contrôles à partir de la ligne de pied de page de `Products` GridView et affecter leurs valeurs à la collection de `e.InputParameters`. Si l’utilisateur omet une valeur telle que en laissant la `ReorderLevel` zone de texte vide, nous devons spécifier que la valeur insérée dans la base de données doit être `NULL`. Étant donné que la méthode `AddProducts` accepte les types Nullable pour les champs de base de données Nullable, utilisez simplement un type Nullable et affectez-lui la valeur `Nothing` dans le cas où l’entrée utilisateur est omise.

[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample8.vb)]

Une fois le gestionnaire d’événements `Inserting` terminé, de nouveaux enregistrements peuvent être ajoutés à la table de base de données `Products` par le biais de la ligne de pied de page GridView s. Essayez d’ajouter plusieurs nouveaux produits.

## <a name="enhancing-and-customizing-the-add-operation"></a>Amélioration et personnalisation de l’opération d’ajout

Actuellement, le fait de cliquer sur le bouton Ajouter ajoute un nouvel enregistrement à la table de base de données, mais ne fournit aucune sorte de retour visuel indiquant que l’enregistrement a été ajouté avec succès. Dans l’idéal, un contrôle Web label ou une zone d’alerte côté client informera l’utilisateur que son insertion s’est terminée avec succès. Je laisse cela en tant qu’exercice pour le lecteur.

Le contrôle GridView utilisé dans ce didacticiel n’applique aucun ordre de tri aux produits listés et ne permet pas non plus à l’utilisateur final de trier les données. Par conséquent, les enregistrements sont triés tels qu’ils figurent dans la base de données par leur champ de clé primaire. Étant donné que chaque nouvel enregistrement a une valeur de `ProductID` supérieure à la dernière, chaque fois qu’un nouveau produit est ajouté, il est ajouté à la fin de la grille. Par conséquent, vous souhaiterez peut-être envoyer automatiquement l’utilisateur à la dernière page du contrôle GridView après l’ajout d’un nouvel enregistrement. Pour ce faire, vous pouvez ajouter la ligne de code suivante après l’appel à `ProductsDataSource.Insert()` dans le gestionnaire d’événements `RowCommand` pour indiquer que l’utilisateur doit être envoyé à la dernière page après avoir lié les données au GridView :

[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample9.vb)]

`SendUserToLastPage` est une variable booléenne au niveau de la page à laquelle une valeur de `False`est assignée initialement. Dans le gestionnaire d’événements `DataBound` GridView s, si `SendUserToLastPage` a la valeur false, la propriété `PageIndex` est mise à jour pour envoyer l’utilisateur à la dernière page.

[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample10.vb)]

La raison pour laquelle la propriété `PageIndex` est définie dans le gestionnaire d’événements `DataBound` (par opposition au gestionnaire d’événements `RowCommand`) est que, lorsque le gestionnaire d’événements `RowCommand` se déclenche, nous avons encore ajouté le nouvel enregistrement à la table de base de données `Products`. Par conséquent, dans le gestionnaire d’événements `RowCommand` le dernier index de page (`PageCount - 1`) représente le dernier index de page *avant* l’ajout du nouveau produit. Pour la majorité des produits ajoutés, le dernier index de page est le même après l’ajout du nouveau produit. Toutefois, lorsque le produit ajouté entraîne la création d’un nouvel index de dernière page, si nous mettons à jour de manière incorrecte le `PageIndex` dans le gestionnaire d’événements `RowCommand`, nous allons passer à la dernière page (l’index de la dernière page avant l’ajout du nouveau produit), par opposition au nouvel index de la dernière page. Étant donné que le gestionnaire d’événements `DataBound` se déclenche une fois que le nouveau produit a été ajouté et que les données sont reliées à la grille, en définissant la propriété `PageIndex`, nous savons que nous obtenons à nouveau l’index de dernière page correct.

Enfin, le GridView utilisé dans ce didacticiel est assez vaste en raison du nombre de champs qui doivent être collectés pour l’ajout d’un nouveau produit. En raison de cette largeur, une disposition verticale de DetailsView s peut être préférée. La largeur globale de GridView s peut être réduite en collectant moins d’entrées. Peut-être n’avez-vous pas besoin de collecter les champs `UnitsOnOrder`, `UnitsInStock`et `ReorderLevel` lors de l’ajout d’un nouveau produit, auquel cas ces champs peuvent être supprimés du contrôle GridView.

Pour ajuster les données collectées, nous pouvons utiliser l’une des deux approches suivantes :

- Continuez à utiliser la méthode `AddProduct` qui attend des valeurs pour les champs `UnitsOnOrder`, `UnitsInStock`et `ReorderLevel`. Dans le gestionnaire d’événements `Inserting`, fournissez des valeurs par défaut codées en dur à utiliser pour ces entrées qui ont été supprimées de l’interface d’insertion.
- Créez une nouvelle surcharge de la méthode `AddProduct` dans la classe `ProductsBLL` qui n’accepte pas les entrées pour les champs `UnitsOnOrder`, `UnitsInStock`et `ReorderLevel`. Ensuite, dans la page ASP.NET, configurez le ObjectDataSource pour utiliser cette nouvelle surcharge.

Les deux options fonctionnent également aussi bien. Dans les didacticiels précédents, nous avons utilisé cette dernière option, en créant plusieurs surcharges pour la méthode `ProductsBLL` Class s `UpdateProduct`.

## <a name="summary"></a>Récapitulatif

Le GridView ne dispose pas des fonctionnalités d’insertion intégrées de DetailsView et FormView, mais avec un peu d’effort, une interface d’insertion peut être ajoutée à la ligne de pied de page. Pour afficher la ligne de pied de page dans un GridView, il suffit de définir sa propriété `ShowFooter` sur `True`. Le contenu de la ligne de pied de page peut être personnalisé pour chaque champ en convertissant le champ en TemplateField et en ajoutant l’interface d’insertion au `FooterTemplate`. Comme nous l’avons vu dans ce didacticiel, les `FooterTemplate` peuvent contenir des boutons, des zones de texte, des DropDownList, des cases à cocher, des contrôles de source de données pour remplir des contrôles Web pilotés par les données (tels que DropDownList) et des contrôles de validation. Outre les contrôles pour la collecte de l’entrée de l’utilisateur, un bouton Ajouter, LinkButton ou ImageButton est nécessaire.

Quand l’utilisateur clique sur le bouton Ajouter, la méthode ObjectDataSource s `Insert()` est appelée pour démarrer le flux de travail d’insertion. ObjectDataSource appellera ensuite la méthode d’insertion configurée (la méthode `ProductsBLL` classe s `AddProduct`, dans ce didacticiel). Nous devons copier les valeurs de l’interface de l’insertion de GridView s vers la collection d’objets ObjectDataSource `InsertParameters` avant d’appeler la méthode Insert. Pour ce faire, vous pouvez référencer par programmation les contrôles Web d’insertion d’interface dans le gestionnaire d’événements `Inserting` ObjectDataSource s.

Ce didacticiel termine notre examen des techniques pour améliorer l’apparence de GridView s. L’ensemble suivant de didacticiels vous apprendra à utiliser des données binaires telles que des images, des fichiers PDF, des documents Word, etc., ainsi que les contrôles Web de données.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel était Bernadette Leigh. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](adding-a-gridview-column-of-checkboxes-vb.md)
