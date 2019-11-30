---
uid: web-forms/overview/data-access/working-with-batched-data/batch-inserting-vb
title: Insertion par lots (VB) | Microsoft Docs
author: rick-anderson
description: Apprenez à insérer plusieurs enregistrements de base de données en une seule opération. Dans la couche d’interface utilisateur, nous étendons le contrôle GridView pour permettre à l’utilisateur d’entrer plusieurs n...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: 48e2a4ae-77ca-4208-a204-c38c690ffb59
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-inserting-vb
msc.type: authoredcontent
ms.openlocfilehash: 413a0e209b1899414eaab70aff07ee0d3223f28f
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74643123"
---
# <a name="batch-inserting-vb"></a>Insertion par lots (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_66_VB.zip) ou [Télécharger le PDF](batch-inserting-vb/_static/datatutorial66vb1.pdf)

> Apprenez à insérer plusieurs enregistrements de base de données en une seule opération. Dans la couche d’interface utilisateur, nous étendons le contrôle GridView pour permettre à l’utilisateur d’entrer plusieurs nouveaux enregistrements. Dans la couche d’accès aux données, nous encapsulons les multiples opérations d’insertion dans une transaction pour vous assurer que toutes les insertions ont été effectuées ou que toutes les insertions sont annulées.

## <a name="introduction"></a>Introduction

Dans le didacticiel de [mise à jour par lot](batch-updating-vb.md) , nous avons examiné la personnalisation du contrôle GridView pour présenter une interface dans laquelle plusieurs enregistrements étaient modifiables. L’utilisateur visitant la page peut apporter une série de modifications, puis, avec un seul clic de bouton, effectuer une mise à jour par lot. Dans les situations où les utilisateurs mettent généralement à jour un grand nombre d’enregistrements en une seule fois, une telle interface peut réduire les clics et les changements de contexte du clavier à la souris par rapport aux fonctionnalités d’édition par ligne par défaut qui ont été examinées pour la première fois dans le didacticiel sur l' [insertion, la mise à jour et la suppression des données](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) .

Ce concept peut également être appliqué lors de l’ajout d’enregistrements. Imaginez que chez Northwind Traders, nous recevons habituellement des expéditions de fournisseurs qui contiennent un certain nombre de produits pour une catégorie particulière. À titre d’exemple, nous pouvons recevoir une livraison de six produits de thé et de café différents de Tokyo Traders. Si un utilisateur entre les six produits un par un à l’aide d’un contrôle DetailsView, il devra choisir un grand nombre des mêmes valeurs plusieurs fois : il devra choisir la même catégorie (boissons), le même fournisseur (Tokyo Traders), la même valeur abandonnée ( False) et les mêmes unités sur la valeur de la commande (0). Cette entrée de données répétitive n’est pas seulement une opération fastidieuse, mais elle est sujette aux erreurs.

Avec un peu de travail, nous pouvons créer une interface d’insertion par lot qui permet à l’utilisateur de choisir le fournisseur et la catégorie une seule fois, d’entrer une série de noms de produits et de prix unitaires, puis de cliquer sur un bouton pour ajouter les nouveaux produits à la base de données (voir la figure 1). À mesure que chaque produit est ajouté, les valeurs entrées dans les zones de texte `ProductName` et `UnitPrice` sont affectées aux valeurs entrées dans les zones de texte, tandis que les valeurs `CategoryID` et `SupplierID` sont affectées aux valeurs du DropDownList en haut du formulaire. Les valeurs `Discontinued` et `UnitsOnOrder` sont définies sur les valeurs codées en dur de `False` et 0, respectivement.

[![l’interface d’insertion de lot](batch-inserting-vb/_static/image2.png)](batch-inserting-vb/_static/image1.png)

**Figure 1**: interface d’insertion de lot ([cliquez pour afficher l’image en taille réelle](batch-inserting-vb/_static/image3.png))

Dans ce didacticiel, nous allons créer une page qui implémente l’interface d’insertion de lot illustrée à la figure 1. Comme pour les deux didacticiels précédents, nous encapsulerons les insertions dans l’étendue d’une transaction pour garantir l’atomicité. Commençons !

## <a name="step-1-creating-the-display-interface"></a>Étape 1 : création de l’interface d’affichage

Ce didacticiel se compose d’une seule page divisée en deux régions : une zone d’affichage et une région d’insertion. L’interface d’affichage, que nous allons créer dans cette étape, affiche les produits dans un GridView et comprend un bouton intitulé traiter le produit expédition. Lorsque vous cliquez sur ce bouton, l’interface d’affichage est remplacée par l’interface d’insertion, illustrée à la figure 1. L’interface d’affichage retourne une fois que vous avez cliqué sur les boutons ajouter des produits à partir de l’expédition ou annuler. Nous allons créer l’interface d’insertion à l’étape 2.

Lors de la création d’une page qui a deux interfaces, une seule qui est visible à la fois, chaque interface est généralement placée dans un [contrôle Web Panel](http://www.w3schools.com/aspnet/control_panel.asp), qui sert de conteneur pour d’autres contrôles. Par conséquent, notre page disposera de deux contrôles Panel, un pour chaque interface.

Commencez par ouvrir la page `BatchInsert.aspx` dans le dossier `BatchData` et faites glisser un panneau de la boîte à outils vers le concepteur (voir figure 2). Définissez la propriété Panel s `ID` sur `DisplayInterface`. Lorsque vous ajoutez le panneau au concepteur, ses propriétés `Height` et `Width` sont définies sur 50px et 125px, respectivement. Effacez ces valeurs de propriété de la Fenêtre Propriétés.

[![faire glisser un panneau de la boîte à outils vers le concepteur](batch-inserting-vb/_static/image5.png)](batch-inserting-vb/_static/image4.png)

**Figure 2**: faire glisser un panneau de la boîte à outils vers le concepteur ([cliquez pour afficher l’image en taille réelle](batch-inserting-vb/_static/image6.png))

Ensuite, faites glisser un contrôle Button et GridView dans le panneau. Affectez à la propriété Button s `ID` la valeur `ProcessShipment` et à sa propriété `Text` pour traiter l’expédition du produit. Définissez la propriété GridView s `ID` sur `ProductsGrid` et, à partir de sa balise active, liez-la à un nouvel ObjectDataSource nommé `ProductsDataSource`. Configurez le ObjectDataSource pour extraire ses données de la méthode de `GetProducts` de la classe `ProductsBLL`. Étant donné que ce GridView est utilisé uniquement pour afficher des données, définissez les listes déroulantes dans les onglets mettre à jour, insérer et supprimer sur (aucun). Cliquez sur Terminer pour terminer l’Assistant Configuration de la source de données.

[![afficher les données retournées à partir de la méthode ProductsBLL de la classe s GetProducts](batch-inserting-vb/_static/image8.png)](batch-inserting-vb/_static/image7.png)

**Figure 3**: afficher les données retournées par la méthode `ProductsBLL` classe s `GetProducts` ([cliquez pour afficher l’image en taille réelle](batch-inserting-vb/_static/image9.png))

[![définir les listes déroulantes dans les onglets mettre à jour, insérer et supprimer sur (aucun)](batch-inserting-vb/_static/image11.png)](batch-inserting-vb/_static/image10.png)

**Figure 4**: définir les listes déroulantes des onglets mettre à jour, insérer et supprimer sur (aucun) ([cliquez pour afficher l’image en taille réelle](batch-inserting-vb/_static/image12.png))

Une fois l’exécution de l’Assistant ObjectDataSource terminée, Visual Studio ajoute BoundFields et CheckBoxField pour les champs de données de produit. Supprimez tous les champs sauf les `ProductName`, `CategoryName`, `SupplierName`, `UnitPrice`et `Discontinued`. N’hésitez pas à effectuer des personnalisations esthétiques. J’ai décidé de mettre en forme le champ `UnitPrice` en tant que valeur monétaire, de réorganiser les champs et de renommer plusieurs champs `HeaderText` valeurs. Configurez également le contrôle GridView pour inclure la prise en charge de la pagination et du tri en activant les cases à cocher Activer la pagination et activer le tri dans la balise active GridView s.

Après avoir ajouté les contrôles Panel, Button, GridView et ObjectDataSource et personnalisé les champs GridView s, le balisage déclaratif de votre page doit ressembler à ce qui suit :

[!code-aspx[Main](batch-inserting-vb/samples/sample1.aspx)]

Notez que le balisage du bouton et de GridView apparaissent dans les balises d’ouverture et de fermeture `<asp:Panel>`. Étant donné que ces contrôles se trouvent dans le panneau `DisplayInterface`, nous pouvons les masquer en affectant simplement à la propriété Panel s `Visible` la valeur `False`. L’étape 3 examine par programmation la modification de la propriété Panel s `Visible` en réponse à un clic sur un bouton pour afficher une interface tout en masquant l’autre.

Prenez un moment pour voir notre progression dans un navigateur. Comme le montre la figure 5, vous devez voir un bouton traiter le produit dans un contrôle GridView qui répertorie les produits dix à la fois.

[![le contrôle GridView répertorie les produits et propose des fonctionnalités de tri et de pagination](batch-inserting-vb/_static/image14.png)](batch-inserting-vb/_static/image13.png)

**Figure 5**: le contrôle GridView répertorie les produits et propose des fonctionnalités de tri et de pagination ([cliquez pour afficher l’image en taille réelle](batch-inserting-vb/_static/image15.png))

## <a name="step-2-creating-the-inserting-interface"></a>Étape 2 : création de l’interface d’insertion

Une fois l’interface d’affichage terminée, nous sommes prêts à créer l’interface d’insertion. Pour ce didacticiel, vous pouvez créer une interface d’insertion qui invite à fournir une valeur unique pour le fournisseur et la catégorie, puis permet à l’utilisateur d’entrer jusqu’à cinq noms de produits et des valeurs de prix unitaire. Avec cette interface, l’utilisateur peut ajouter un à cinq nouveaux produits qui partagent tous la même catégorie et le même fournisseur, mais qui ont des noms de produits et des prix uniques.

Commencez par faire glisser un panneau de la boîte à outils vers le concepteur, en le plaçant sous le panneau `DisplayInterface` existant. Définissez la propriété `ID` de ce panneau récemment ajouté sur `InsertingInterface` et définissez sa propriété `Visible` sur `False`. Nous allons ajouter du code qui définit la propriété `InsertingInterface` du panneau `Visible` sur `True` à l’étape 3. Désactivez également les valeurs de propriété Panel `Height` et `Width`.

Ensuite, nous devons créer l’interface d’insertion illustrée à la figure 1. Cette interface peut être créée à l’aide de diverses techniques HTML, mais nous allons utiliser une table à quatre colonnes et sept lignes.

> [!NOTE]
> Quand vous entrez le balisage pour les éléments de `<table>` HTML, je préfère utiliser le mode Source. Alors que Visual Studio possède des outils permettant d’ajouter des éléments de `<table>` par le biais du concepteur, le concepteur semble être trop disposé à injecter des paramètres de `style` dans le balisage. Une fois que j’ai créé le balisage `<table>`, je retourne généralement dans le concepteur pour ajouter les contrôles Web et définir leurs propriétés. Lors de la création de tables avec des colonnes et des lignes prédéterminées, je préfère utiliser du code HTML statique plutôt que le [contrôle Web de table](https://msdn.microsoft.com/library/system.web.ui.webcontrols.table.aspx) , car tous les contrôles Web placés dans un contrôle Web table sont accessibles uniquement à l’aide du modèle `FindControl("controlID")`. Toutefois, j’utilise des contrôles Web table pour les tables de taille dynamique (celles dont les lignes ou les colonnes sont basées sur une base de données ou des critères spécifiés par l’utilisateur), étant donné que le contrôle Web table peut être construit par programmation.

Entrez la balise suivante dans les balises `<asp:Panel>` du panneau `InsertingInterface` :

[!code-html[Main](batch-inserting-vb/samples/sample2.html)]

Ce balisage `<table>` n’inclut pas encore de contrôles Web, nous les ajouterons momentanément. Notez que chaque élément `<tr>` contient un paramètre de classe CSS particulier : `BatchInsertHeaderRow` pour la ligne d’en-tête où le fournisseur et la catégorie DropDownList vont. `BatchInsertFooterRow` de la ligne de pied de page où se trouvent les boutons ajouter des produits à partir de l’expédition et annuler. et en alternant les valeurs `BatchInsertRow` et `BatchInsertAlternatingRow` pour les lignes qui contiendront les contrôles de zone de texte Product et Unit Price. J’ai créé des classes CSS correspondantes dans le fichier `Styles.css` pour attribuer à l’interface d’insertion une apparence similaire aux contrôles GridView et DetailsView que nous avons utilisés dans ces didacticiels. Ces classes CSS sont présentées ci-dessous.

[!code-css[Main](batch-inserting-vb/samples/sample3.css)]

Une fois cette balise entrée, revenez à la Mode Création. Cette `<table>` doit s’afficher sous la forme d’une table de quatre colonnes et sept lignes dans le concepteur, comme illustré dans la figure 6.

[![l’interface d’insertion est composée d’une table de quatre colonnes, sept lignes](batch-inserting-vb/_static/image17.png)](batch-inserting-vb/_static/image16.png)

**Figure 6**: l’interface d’insertion est composée d’une table de quatre colonnes, sept lignes ([cliquez pour afficher l’image en taille réelle](batch-inserting-vb/_static/image18.png))

Nous sommes maintenant prêts à ajouter les contrôles Web à l’interface d’insertion. Faites glisser deux DropDownList de la boîte à outils vers les cellules appropriées de la table 1 pour le fournisseur et une pour la catégorie.

Définissez la propriété DropDownList s du fournisseur `ID` sur `Suppliers` et liez-la à un nouvel ObjectDataSource nommé `SuppliersDataSource`. Configurez le nouvel ObjectDataSource pour récupérer ses données à partir de la méthode `SuppliersBLL` classe s `GetSuppliers` et définissez la liste déroulante des tabulations de mise à jour sur (aucune). Cliquez sur Terminer pour terminer l'Assistant.

[![configurer ObjectDataSource pour utiliser la méthode SuppliersBLL de la classe s GetSuppliers](batch-inserting-vb/_static/image20.png)](batch-inserting-vb/_static/image19.png)

**Figure 7**: configurer ObjectDataSource pour utiliser la méthode `SuppliersBLL` classe s `GetSuppliers` ([cliquez pour afficher l’image en taille réelle](batch-inserting-vb/_static/image21.png))

Faites en sorte que le `Suppliers` DropDownList affiche le champ de données `CompanyName` et utilisez le champ de données `SupplierID` comme valeurs de `ListItem` s.

[![afficher le champ de données CompanyName et utiliser RéfFournisseur comme valeur](batch-inserting-vb/_static/image23.png)](batch-inserting-vb/_static/image22.png)

**Figure 8**: afficher le champ de données `CompanyName` et utiliser `SupplierID` comme valeur ([cliquez pour afficher l’image en taille réelle](batch-inserting-vb/_static/image24.png))

Nommez la deuxième `Categories` DropDownList et liez-la à un nouvel ObjectDataSource nommé `CategoriesDataSource`. Configurez le `CategoriesDataSource` ObjectDataSource pour utiliser la méthode `GetCategories` de la classe `CategoriesBLL` ; Définissez les listes déroulantes dans les onglets mettre à jour et supprimer sur (aucun), puis cliquez sur Terminer pour terminer l’Assistant. Enfin, faites en sorte que le DropDownList affiche le champ de données `CategoryName` et utilisez le `CategoryID` comme valeur.

Une fois ces deux DropDownList ajoutés et liés à des ObjectDataSources configurés correctement, votre écran doit ressembler à la figure 9.

[![la ligne d’en-tête contient désormais les fournisseurs et les catégories DropDownList](batch-inserting-vb/_static/image26.png)](batch-inserting-vb/_static/image25.png)

**Figure 9**: la ligne d’en-tête contient maintenant les `Suppliers` et `Categories` DropDownList ([cliquez pour afficher l’image en taille réelle](batch-inserting-vb/_static/image27.png))

Nous devons maintenant créer les zones de texte pour collecter le nom et le prix de chaque nouveau produit. Faites glisser un contrôle TextBox de la boîte à outils vers le concepteur pour chacun des cinq lignes Product Name et Price. Définissez les propriétés de `ID` des zones de texte sur `ProductName1`, `UnitPrice1`, `ProductName2`, `UnitPrice2`, `ProductName3`, `UnitPrice3`, et ainsi de suite.

Ajoutez un CompareValidator après chaque zone de texte prix unitaire, en affectant à la propriété `ControlToValidate` la `ID`appropriée. Définissez également la propriété `Operator` sur `GreaterThanEqual`, `ValueToCompare` sur 0 et `Type` sur `Currency`. Ces paramètres indiquent à CompareValidator de s’assurer que le prix, s’il est entré, est une valeur monétaire valide supérieure ou égale à zéro. Affectez à la propriété `Text` la valeur \*et `ErrorMessage` au prix doit être supérieur ou égal à zéro. Par ailleurs, omettez les symboles monétaires.

> [!NOTE]
> L’interface d’insertion n’inclut pas de contrôles RequiredFieldValidator, même si le champ `ProductName` de la table de base de données `Products` n’autorise pas les valeurs `NULL`. Cela est dû au fait que nous souhaitons permettre à l’utilisateur d’entrer jusqu’à cinq produits. Par exemple, si l’utilisateur doit fournir le nom de produit et le prix unitaire pour les trois premières lignes, en laissant les deux dernières lignes vides, nous allons simplement ajouter trois nouveaux produits au système. Étant donné que `ProductName` est requis, toutefois, nous devons vérifier par programmation si un prix unitaire est entré pour indiquer qu’une valeur de nom de produit correspondante est fournie. Nous allons aborder ce contrôle à l’étape 4.

Lors de la validation de l’entrée de l’utilisateur, CompareValidator signale des données non valides si la valeur contient un symbole monétaire. Ajoutez un $ devant chaque zone de texte prix unitaire pour servir d’indicateur visuel qui indique à l’utilisateur d’omettre le symbole monétaire lors de la saisie du prix.

Enfin, ajoutez un contrôle ValidationSummary dans le panneau `InsertingInterface`, paramètre sa propriété `ShowMessageBox` à `True` et sa propriété `ShowSummary` à `False`. Avec ces paramètres, si l’utilisateur entre une valeur de prix unitaire non valide, un astérisque apparaît en regard des contrôles de zone de texte incriminés et le contrôle ValidationSummary affiche un MessageBox côté client qui affiche le message d’erreur que nous avons spécifié précédemment.

À ce stade, votre écran doit ressembler à la figure 10.

[![l’interface d’insertion comprend désormais des zones de texte pour les noms et les prix des produits](batch-inserting-vb/_static/image29.png)](batch-inserting-vb/_static/image28.png)

**Figure 10**: l’interface d’insertion comprend maintenant des zones de texte pour les noms de produits et les prix ([cliquez pour afficher l’image en taille réelle](batch-inserting-vb/_static/image30.png))

Ensuite, nous devons ajouter les boutons ajouter des produits à partir de l’expédition et annuler à la ligne de pied de page. Faites glisser deux contrôles Button de la boîte à outils vers le pied de page de l’interface d’insertion, en définissant les boutons `ID` propriétés sur `AddProducts` et `CancelButton` et `Text` propriétés pour ajouter des produits à partir de l’expédition et annuler, respectivement. En outre, définissez la propriété `CancelButton` contrôle s `CausesValidation` sur `false`.

Enfin, nous devons ajouter un contrôle Web Label qui affichera les messages d’État pour les deux interfaces. Par exemple, lorsqu’un utilisateur ajoute avec succès une nouvelle livraison de produits, nous voulons revenir à l’interface d’affichage et afficher un message de confirmation. Toutefois, si l’utilisateur fournit un prix pour un nouveau produit, mais ne quitte pas le nom du produit, nous devons afficher un message d’avertissement dans la mesure où le champ `ProductName` est obligatoire. Comme nous avons besoin de ce message pour afficher les deux interfaces, placez-le en haut de la page en dehors des panneaux.

Faites glisser un contrôle Web label de la boîte à outils vers le haut de la page dans le concepteur. Affectez à la propriété `ID` la valeur `StatusLabel`, décochez la propriété `Text` et définissez les propriétés `Visible` et `EnableViewState` sur `False`. Comme nous l’avons vu dans les didacticiels précédents, la définition de la propriété `EnableViewState` sur `False` nous permet de modifier par programmation les valeurs des propriétés des étiquettes et de les rétablir automatiquement à leurs valeurs par défaut lors de la publication suivante. Cela simplifie le code pour l’indication d’un message d’État en réponse à une action de l’utilisateur qui disparaît lors de la publication (postback) suivante. Enfin, définissez la propriété `StatusLabel` Control s `CssClass` sur Warning, qui est le nom d’une classe CSS définie dans `Styles.css` qui affiche du texte dans une police large, en italique, en gras et en rouge.

La figure 11 montre le concepteur Visual Studio après l’ajout et la configuration de l’étiquette.

[![placer le contrôle StatusLabel au-dessus des deux contrôles Panel](batch-inserting-vb/_static/image32.png)](batch-inserting-vb/_static/image31.png)

**Figure 11**: placer le contrôle de `StatusLabel` au-dessus des deux contrôles de panneau ([cliquez pour afficher l’image en taille réelle](batch-inserting-vb/_static/image33.png))

## <a name="step-3-switching-between-the-display-and-inserting-interfaces"></a>Étape 3 : basculer entre l’affichage et l’insertion d’interfaces

À ce stade, nous avons terminé le balisage de nos interfaces d’affichage et d’insertion, mais nous avons conservé les deux tâches suivantes :

- Basculement entre l’affichage et l’insertion d’interfaces
- Ajout des produits dans la base de données

Actuellement, l’interface d’affichage est visible, mais l’interface d’insertion est masquée. Cela est dû au fait que la propriété `DisplayInterface` du panneau des `Visible` est définie sur `True` (valeur par défaut), alors que la `Visible` propriété du panneau `InsertingInterface` s est définie sur `False`. Pour basculer entre les deux interfaces, il suffit de basculer chaque contrôle s `Visible` valeur de propriété.

Nous voulons passer de l’interface d’affichage à l’interface d’insertion lorsque l’utilisateur clique sur le bouton expédier le produit. Par conséquent, créez un gestionnaire d’événements pour ce bouton `Click` événement qui contient le code suivant :

[!code-vb[Main](batch-inserting-vb/samples/sample4.vb)]

Ce code masque simplement le panneau `DisplayInterface` et affiche le panneau `InsertingInterface`.

Ensuite, créez des gestionnaires d’événements pour les contrôles ajouter des produits à partir de l’expédition et du bouton Annuler dans l’interface d’insertion. Lorsque vous cliquez sur l’un de ces boutons, nous devons revenir à l’interface d’affichage. Créez `Click` gestionnaires d’événements pour les deux contrôles de bouton afin qu’ils appellent `ReturnToDisplayInterface`, une méthode que nous ajouterons momentanément. En plus de masquer le panneau de `InsertingInterface` et d’afficher le panneau de `DisplayInterface`, la méthode `ReturnToDisplayInterface` doit renvoyer les contrôles Web à leur état antérieur à la modification. Cela implique la définition des propriétés de `SelectedIndex` DropDownList sur 0 et l’effacement des propriétés `Text` des contrôles TextBox.

> [!NOTE]
> Réfléchissez à ce qui peut se produire si nous n’avons pas renvoyé les contrôles à leur état antérieur à la modification avant de revenir à l’interface d’affichage. Un utilisateur peut cliquer sur le bouton traiter l’expédition du produit, entrer les produits de l’expédition, puis cliquer sur Ajouter des produits à partir de l’expédition. Cela ajoute les produits et renvoie l’utilisateur à l’interface d’affichage. À ce stade, l’utilisateur peut souhaiter ajouter une autre expédition. Quand vous cliquez sur le bouton traiter l’expédition du produit, vous revenez à l’interface d’insertion, mais les sélections DropDownList et les valeurs de zone de texte sont toujours remplies avec leurs valeurs précédentes.

[!code-vb[Main](batch-inserting-vb/samples/sample5.vb)]

Les deux `Click` gestionnaires d’événements appellent simplement la méthode `ReturnToDisplayInterface`, bien que nous puissions revenir au gestionnaire d’événements ajouter des produits à partir de la livraison `Click` à l’étape 4 et ajouter du code pour enregistrer les produits. `ReturnToDisplayInterface` commence par retourner le `Suppliers` et `Categories` DropDownList à leurs premières options. Les deux constantes `firstControlID` et `lastControlID` marquer les valeurs d’index de contrôle de début et de fin utilisées pour nommer les zones de texte Nom du produit et prix unitaire dans l’interface d’insertion et sont utilisées dans les limites de la boucle `For` qui définit les propriétés `Text` des contrôles TextBox sur une chaîne vide. Enfin, les panneaux `Visible` propriétés sont réinitialisés afin que l’interface d’insertion soit masquée et l’interface d’affichage affichée.

Prenez un moment pour tester cette page dans un navigateur. Lors de la première visite de la page, vous devez voir l’interface d’affichage comme illustré à la figure 5. Cliquez sur le bouton traiter l’expédition du produit. La page est postback et vous devez maintenant voir l’interface d’insertion, comme illustré à la figure 12. Si vous cliquez sur le bouton Ajouter des produits à partir d’un envoi ou d’un bouton Annuler, vous pouvez revenir à l’interface d’affichage.

> [!NOTE]
> Lors de l’affichage de l’interface d’insertion, prenez un moment pour tester le CompareValidators dans les zones de texte Unit Price. Vous devez voir un avertissement MessageBox côté client quand vous cliquez sur le bouton Ajouter des produits à partir de l’expédition avec des valeurs monétaires ou des prix non valides avec une valeur inférieure à zéro.

[![l’interface d’insertion s’affiche après avoir cliqué sur le bouton traiter l’expédition du produit](batch-inserting-vb/_static/image35.png)](batch-inserting-vb/_static/image34.png)

**Figure 12**: l’interface d’insertion s’affiche après avoir cliqué sur le bouton traiter l’expédition du produit ([cliquez pour afficher l’image en taille réelle](batch-inserting-vb/_static/image36.png))

## <a name="step-4-adding-the-products"></a>Étape 4 : ajout des produits

Tout ce qui reste dans ce didacticiel consiste à enregistrer les produits dans la base de données dans le gestionnaire d’événements ajouter des produits à partir du bouton d’expédition s `Click`. Pour ce faire, vous pouvez créer un `ProductsDataTable` et ajouter une instance `ProductsRow` pour chacun des noms de produits fournis. Une fois ces `ProductsRow` s ajoutées, nous allons appeler la méthode `ProductsBLL` classe s `UpdateWithTransaction` passant dans le `ProductsDataTable`. Rappelez-vous que la méthode `UpdateWithTransaction`, qui a été créée dans les [modifications d’encapsulation de la base de données dans un](wrapping-database-modifications-within-a-transaction-vb.md) didacticiel sur les transactions, passe le `ProductsDataTable` à la méthode `UpdateWithTransaction` de `ProductsTableAdapter`. À partir de là, une transaction ADO.NET est démarrée et le TableAdapter émet une instruction `INSERT` à la base de données pour chaque `ProductsRow` ajoutée dans le DataTable. En supposant que tous les produits sont ajoutés sans erreur, la transaction est validée, sinon elle est restaurée.

Le code du bouton Ajouter des produits à partir du bouton de livraison `Click` un gestionnaire d’événements doit également effectuer une vérification des erreurs. Étant donné qu’il n’y a aucun RequiredFieldValidators utilisé dans l’interface d’insertion, un utilisateur peut entrer un prix pour un produit tout en omettant son nom. Étant donné que le nom du produit est obligatoire, si une telle condition est déroulée, nous devons signaler l’utilisateur et ne pas poursuivre les insertions. Le code complet du gestionnaire d’événements `Click` suit :

[!code-vb[Main](batch-inserting-vb/samples/sample6.vb)]

Le gestionnaire d’événements commence par s’assurer que la propriété `Page.IsValid` retourne une valeur de `True`. Si elle retourne `False`, cela signifie qu’un ou plusieurs CompareValidators sont en signalant des données non valides ; dans ce cas, nous ne souhaitons pas tenter d’insérer les produits entrés ou nous obtiendrons une exception lors de la tentative d’affectation de la valeur du prix unitaire entré par l’utilisateur à la propriété `ProductsRow` s `UnitPrice`.

Ensuite, une nouvelle instance de `ProductsDataTable` est créée (`products`). Une boucle `For` est utilisée pour effectuer une itération dans les zones de texte Product Name et Price Unit et les propriétés `Text` sont lues dans les variables locales `productName` et `unitPrice`. Si l’utilisateur a entré une valeur pour le prix unitaire mais pas pour le nom du produit correspondant, le `StatusLabel` affiche le message si vous fournissez un prix unitaire, vous devez également inclure le nom du produit et le gestionnaire d’événements est fermé.

Si un nom de produit a été fourni, une nouvelle instance de `ProductsRow` est créée à l’aide de la méthode `ProductsDataTable` s `NewProductsRow`. Cette nouvelle propriété d’instance de `ProductsRow` `ProductName` est définie sur la zone de texte nom de produit actuel, tandis que les propriétés `SupplierID` et `CategoryID` sont affectées aux propriétés de `SelectedValue` de DropDownList dans l’en-tête d’insertion d’interface s. Si l’utilisateur a entré une valeur pour le prix du produit, il est affecté à la propriété `ProductsRow` d’instances `UnitPrice` ; dans le cas contraire, la propriété n’est pas assignée, ce qui génère une valeur `NULL` pour `UnitPrice` dans la base de données. Enfin, les propriétés `Discontinued` et `UnitsOnOrder` sont affectées aux valeurs codées en dur `False` et à 0, respectivement.

Une fois les propriétés affectées à l’instance `ProductsRow`, elles sont ajoutées au `ProductsDataTable`.

À la fin de la boucle de `For`, nous vérifions si des produits ont été ajoutés. L’utilisateur peut, après tout, avoir cliqué sur Ajouter des produits à partir de la livraison avant d’entrer un nom de produit ou un prix. S’il existe au moins un produit dans le `ProductsDataTable`, la méthode `ProductsBLL` classe s `UpdateWithTransaction` est appelée. Ensuite, les données sont reliées au `ProductsGrid` GridView afin que les produits récemment ajoutés s’affichent dans l’interface d’affichage. Le `StatusLabel` est mis à jour pour afficher un message de confirmation et le `ReturnToDisplayInterface` est appelé, en masquant l’interface d’insertion et en affichant l’interface d’affichage.

Si aucun produit n’a été entré, l’interface d’insertion reste affichée mais le message aucun produit n’a été ajouté. Entrez les noms des produits et les prix unitaires dans les zones de texte s’affichent.

Les figures 13, 14 et 15 montrent les interfaces d’insertion et d’affichage en action. Dans la figure 13, l’utilisateur a entré une valeur de prix unitaire sans nom de produit correspondant. La figure 14 montre l’interface d’affichage après que trois nouveaux produits ont été ajoutés avec succès, tandis que la figure 15 illustre deux des produits récemment ajoutés dans le GridView (le troisième est sur la page précédente).

[![un nom de produit est requis lors de l’entrée d’un prix unitaire](batch-inserting-vb/_static/image38.png)](batch-inserting-vb/_static/image37.png)

**Figure 13**: un nom de produit est requis lors de l’entrée d’un prix unitaire ([cliquez pour afficher l’image en taille réelle](batch-inserting-vb/_static/image39.png))

[![trois nouveaux veggies ont été ajoutés pour le fournisseur Mayumi s](batch-inserting-vb/_static/image41.png)](batch-inserting-vb/_static/image40.png)

**Figure 14**: trois nouveaux veggies ont été ajoutés pour le fournisseur Mayumi s ([cliquez pour afficher l’image en taille réelle](batch-inserting-vb/_static/image42.png))

[![les nouveaux produits se trouvent dans la dernière page du contrôle GridView](batch-inserting-vb/_static/image44.png)](batch-inserting-vb/_static/image43.png)

**Figure 15**: les nouveaux produits se trouvent dans la dernière page du GridView ([cliquez pour afficher l’image en taille réelle](batch-inserting-vb/_static/image45.png))

> [!NOTE]
> La logique d’insertion de lot utilisée dans ce didacticiel encapsule les insertions dans l’étendue de la transaction. Pour vérifier cela, introduisez une erreur au niveau de la base de données de façon intentionnelle. Par exemple, au lieu d’assigner la propriété nouvelle `ProductsRow` instance `CategoryID` à la valeur sélectionnée dans le DropDownList `Categories`, affectez-la à une valeur telle que `i * 5`. Ici `i` est l’indexeur de boucle et contient des valeurs comprises entre 1 et 5. Par conséquent, lors de l’ajout de deux produits ou plus dans le cadre de l’insertion par lot, le premier produit aura une valeur de `CategoryID` valide (5), mais les produits suivants auront des valeurs `CategoryID` qui ne correspondent pas à `CategoryID` valeurs de la table `Categories`. L’effet net est que, pendant que la première `INSERT` réussit, les suivantes échouent avec une violation de contrainte de clé étrangère. Étant donné que l’insertion de lot est atomique, la première `INSERT` est restaurée, ce qui ramène la base de données à son état avant le début du processus d’insertion de lot.

## <a name="summary"></a>Récapitulatif

Au-delà de ces deux didacticiels, nous avons créé des interfaces qui permettent de mettre à jour, de supprimer et d’insérer des lots de données, qui utilisaient tous la prise en charge des transactions que nous avons ajoutée à la couche d’accès aux données dans le didacticiel sur les [modifications de la base de données d’encapsulation dans une transaction](wrapping-database-modifications-within-a-transaction-vb.md) . Pour certains scénarios, ces interfaces utilisateur de traitement par lots améliorent nettement l’efficacité des utilisateurs finaux en réduisant le nombre de clics, de publications et de commutateurs de contexte clavier à souris, tout en maintenant l’intégrité des données sous-jacentes.

Ce didacticiel termine notre examen de l’utilisation des données par lot. L’ensemble suivant de didacticiels explore un large éventail de scénarios avancés de couche d’accès aux données, notamment l’utilisation de procédures stockées dans les méthodes de TableAdapter s, la configuration des paramètres de connexion et de niveau de commande dans la couche DAL, le chiffrement des chaînes de connexion et bien plus encore.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel étaient Hilton Giesenow et S Ren Jacob Lauritsen. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](batch-deleting-vb.md)
