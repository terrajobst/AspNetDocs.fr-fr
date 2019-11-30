---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-vb
title: Filtrage maître/détail avec deux DropDownList (VB) | Microsoft Docs
author: rick-anderson
description: Ce didacticiel développe la relation maître/détail pour ajouter une troisième couche, à l’aide de deux contrôles DropDownList pour sélectionner les dit parent et grand-parent souhaités...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 11ae4f64-01ba-4823-95f4-a2fe1f84f7d7
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-vb
msc.type: authoredcontent
ms.openlocfilehash: 166d6a7664a326361dc2a3f115eddb988cd39d20
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74577953"
---
# <a name="masterdetail-filtering-with-two-dropdownlists-vb"></a>Filtrage maître/détail avec deux DropDownList (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_8_VB.exe) ou [Télécharger le PDF](master-detail-filtering-with-two-dropdownlists-vb/_static/datatutorial08vb1.pdf)

> Ce didacticiel développe la relation maître/détail pour ajouter une troisième couche, à l’aide de deux contrôles DropDownList pour sélectionner les enregistrements parents et grand-parent souhaités.

## <a name="introduction"></a>Introduction

Dans le [didacticiel précédent](master-detail-filtering-with-a-dropdownlist-vb.md) , nous avons examiné comment afficher un simple rapport maître/détails à l’aide d’un seul contrôle DropDownList rempli avec les catégories et un GridView affichant les produits appartenant à la catégorie sélectionnée. Ce modèle de rapport fonctionne bien lorsque vous affichez des enregistrements qui ont une relation un-à-plusieurs et qui peuvent être facilement étendus pour fonctionner dans des scénarios qui incluent plusieurs relations un-à-plusieurs. Par exemple, un système de saisie des commandes aurait des tables qui correspondent à des articles clients, commandes et lignes de commande. Un client donné peut avoir plusieurs commandes avec chaque commande composée de plusieurs éléments. Ces données peuvent être présentées à l’utilisateur avec deux DropDownList et un GridView. Le premier DropDownList a un élément de liste pour chaque client de la base de données, avec le contenu du deuxième qui correspond aux commandes passées par le client sélectionné. Un GridView répertorie les éléments de ligne de la commande sélectionnée.

Tandis que la base de données Northwind inclut les informations sur les détails des commandes et des clients canoniques dans ses tables `Customers`, `Orders`et `Order Details`, ces tables ne sont pas capturées dans notre architecture. Néanmoins, nous pouvons toujours illustrer l’utilisation de deux DropDownList dépendants. Le premier DropDownList répertorie les catégories et les produits appartenant à la catégorie sélectionnée. Un DetailsView affiche ensuite les détails du produit sélectionné.

## <a name="step-1-creating-and-populating-the-categories-dropdownlist"></a>Étape 1 : création et remplissage des catégories DropDownList

Notre premier objectif consiste à ajouter le DropDownList qui répertorie les catégories. Ces étapes ont été examinées en détail dans le didacticiel précédent, mais sont résumées ici à des fins d’exhaustivité.

Ouvrez la page `MasterDetailsDetails.aspx` dans le dossier `Filtering`, ajoutez un contrôle DropDownList à la page, définissez sa propriété `ID` sur `Categories`, puis cliquez sur le lien configurer la source de données dans sa balise active. À partir de l’Assistant Configuration de source de données, choisissez d’ajouter une nouvelle source de données.

[![ajouter une nouvelle source de données pour le DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image2.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image1.png)

**Figure 1**: ajouter une nouvelle source de données pour le DropDownList ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-vb/_static/image3.png))

La nouvelle source de données doit naturellement être un ObjectDataSource. Nommez ce nouvel ObjectDataSource `CategoriesDataSource` et appelez la méthode `GetCategories()` de l’objet `CategoriesBLL`.

[![choisir d’utiliser la classe CategoriesBLL](master-detail-filtering-with-two-dropdownlists-vb/_static/image5.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image4.png)

**Figure 2**: choisir d’utiliser la classe `CategoriesBLL` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-vb/_static/image6.png))

[![configurer ObjectDataSource pour utiliser la méthode GetCategories ()](master-detail-filtering-with-two-dropdownlists-vb/_static/image8.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image7.png)

**Figure 3**: configurer ObjectDataSource pour utiliser la méthode `GetCategories()` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-vb/_static/image9.png))

Après avoir configuré l’ObjectDataSource, nous devons toujours spécifier le champ de source de données à afficher dans le `Categories` DropDownList et celui qui doit être configuré comme valeur pour l’élément de liste. Définissez le champ `CategoryName` comme affichage et `CategoryID` comme valeur pour chaque élément de liste.

[![que le DropDownList affiche le champ CategoryName et utilise CategoryID comme valeur](master-detail-filtering-with-two-dropdownlists-vb/_static/image11.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image10.png)

**Figure 4**: afficher le champ `CategoryName` dans le champ et utiliser `CategoryID` comme valeur ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-vb/_static/image12.png))

À ce stade, nous disposons d’un contrôle DropDownList (`Categories`) rempli avec les enregistrements de la table `Categories`. Quand l’utilisateur choisit une nouvelle catégorie dans le DropDownList, nous voulons qu’une publication (postback) se produise pour actualiser le DropDownList du produit que nous allons créer à l’étape 2. Par conséquent, activez l’option Activer AutoPostBack à partir de la balise active de `categories` DropDownList.

[![activer AutoPostBack pour les catégories DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image14.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image13.png)

**Figure 5**: activer AutoPostBack pour le `Categories` DropDownList ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-vb/_static/image15.png))

## <a name="step-2-displaying-the-selected-categorys-products-in-a-second-dropdownlist"></a>Étape 2 : affichage des produits de la catégorie sélectionnée dans un deuxième DropDownList

Une fois la `Categories` DropDownList terminée, l’étape suivante consiste à afficher un DropDownList des produits appartenant à la catégorie sélectionnée. Pour ce faire, ajoutez un autre contrôle DropDownList à la page nommée `ProductsByCategory`. Comme avec la `Categories` DropDownList, créez un ObjectDataSource pour le `ProductsByCategory` DropDownList nommé `ProductsByCategoryDataSource`.

[![ajouter une nouvelle source de données pour le DropDownList ProductsByCategory](master-detail-filtering-with-two-dropdownlists-vb/_static/image17.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image16.png)

**Figure 6**: ajouter une nouvelle source de données pour le `ProductsByCategory` DropDownList ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-vb/_static/image18.png))

[![créer un ObjectDataSource nommé ProductsByCategoryDataSource](master-detail-filtering-with-two-dropdownlists-vb/_static/image20.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image19.png)

**Figure 7**: créer un ObjectDataSource nommé `ProductsByCategoryDataSource` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-vb/_static/image21.png))

Étant donné que le DropDownList `ProductsByCategory` doit afficher uniquement les produits appartenant à la catégorie sélectionnée, faites en sorte que ObjectDataSource appelle la méthode `GetProductsByCategoryID(categoryID)` à partir de l’objet `ProductsBLL`.

[![choisir d’utiliser la classe ProductsBLL](master-detail-filtering-with-two-dropdownlists-vb/_static/image23.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image22.png)

**Figure 8**: choisir d’utiliser la classe `ProductsBLL` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-vb/_static/image24.png))

[![configurer ObjectDataSource pour utiliser la méthode GetProductsByCategoryID (categoryID)](master-detail-filtering-with-two-dropdownlists-vb/_static/image26.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image25.png)

**Figure 9**: configurer ObjectDataSource pour utiliser la méthode `GetProductsByCategoryID(categoryID)` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-vb/_static/image27.png))

Dans la dernière étape de l’Assistant, vous devez spécifier la valeur du paramètre *`categoryID`* . Affectez ce paramètre à l’élément sélectionné à partir de la `Categories` DropDownList.

[![de l’extraction de la valeur du paramètre categoryID des catégories DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image29.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image28.png)

**Figure 10**: extraire la valeur de paramètre *`categoryID`* du `Categories` DropDownList ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-vb/_static/image30.png))

Avec ObjectDataSource configuré, il ne reste plus qu’à spécifier les champs de source de données utilisés pour l’affichage et la valeur des éléments du DropDownList. Affichez le champ `ProductName` et utilisez le champ `ProductID` comme valeur.

[![spécifier les champs de source de données utilisés pour les propriétés du texte et de la valeur de l’ListItems de DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image32.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image31.png)

**Figure 11**: spécifier les champs de source de données utilisés pour les propriétés du `ListItem` s’du DropDownList `Text` et `Value` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-vb/_static/image33.png))

Avec ObjectDataSource et `ProductsByCategory` DropDownList configurés, notre page affiche deux DropDownList : la première liste toutes les catégories, tandis que la seconde répertorie les produits appartenant à la catégorie sélectionnée. Lorsque l’utilisateur sélectionne une nouvelle catégorie à partir du premier DropDownList, une publication s’affiche et le deuxième DropDownList est relié, indiquant les produits qui appartiennent à la catégorie nouvellement sélectionnée. Les figures 12 et 13 affichent `MasterDetailsDetails.aspx` en action lorsqu’ils sont affichés dans un navigateur.

[![lors de la première visite de la page, la catégorie boissons est sélectionnée](master-detail-filtering-with-two-dropdownlists-vb/_static/image35.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image34.png)

**Figure 12**: lors de la première visite de la page, la catégorie boissons est sélectionnée ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-vb/_static/image36.png))

[![le choix d’une autre catégorie affiche les produits de la nouvelle catégorie](master-detail-filtering-with-two-dropdownlists-vb/_static/image38.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image37.png)

**Figure 13**: choix d’une autre catégorie affiche les produits de la nouvelle catégorie ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-vb/_static/image39.png))

Actuellement, la `productsByCategory` DropDownList, en cas de modification, n’entraîne *pas* de publication (postback). Toutefois, nous souhaitons qu’une publication (postback) se produise une fois que nous ajoutons un DetailsView pour afficher les détails du produit sélectionné (étape 3). Par conséquent, activez la case à cocher Activer AutoPostBack à partir de la balise active de `productsByCategory` DropDownList.

[![activer la fonctionnalité AutoPostBack pour le DropDownList productsByCategory](master-detail-filtering-with-two-dropdownlists-vb/_static/image41.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image40.png)

**Figure 14**: activer la fonctionnalité AutoPostBack pour le `productsByCategory` DropDownList ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-vb/_static/image42.png))

## <a name="step-3-using-a-detailsview-to-display-details-for-the-selected-product"></a>Étape 3 : utilisation d’un DetailsView pour afficher les détails du produit sélectionné

La dernière étape consiste à afficher les détails du produit sélectionné dans un DetailsView. Pour ce faire, ajoutez un DetailsView à la page, affectez à sa propriété `ID` la valeur `ProductDetails`et créez un ObjectDataSource pour celui-ci. Configurez cet ObjectDataSource pour extraire ses données de la méthode `GetProductByProductID(productID)` de la classe `ProductsBLL` à l’aide de la valeur sélectionnée de la `ProductsByCategory` DropDownList pour la valeur du paramètre *`productID`* .

[![choisir d’utiliser la classe ProductsBLL](master-detail-filtering-with-two-dropdownlists-vb/_static/image44.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image43.png)

**Figure 15**: choisir d’utiliser la classe `ProductsBLL` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-vb/_static/image45.png))

[![configurer ObjectDataSource pour utiliser la méthode GetProductByProductID (productID)](master-detail-filtering-with-two-dropdownlists-vb/_static/image47.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image46.png)

**Figure 16**: configurer ObjectDataSource pour utiliser la méthode `GetProductByProductID(productID)` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-vb/_static/image48.png))

[![de l’extraction de la valeur de paramètre productID à partir du DropDownList ProductsByCategory](master-detail-filtering-with-two-dropdownlists-vb/_static/image50.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image49.png)

**Figure 17**: extraire la valeur de paramètre *`productID`* du `ProductsByCategory` DropDownList ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-vb/_static/image51.png))

Vous pouvez choisir d’afficher les champs disponibles dans le `ProductDetails` DetailsView. J’ai choisi de supprimer les champs `ProductID`, `SupplierID`et `CategoryID` et de réorganiser et mettre en forme les champs restants. En outre, j’ai effacé les propriétés `Height` et `Width` du contrôle DetailsView, ce qui permet au DetailsView de se développer jusqu’à la largeur nécessaire pour afficher ses données au lieu de les limiter à une taille spécifiée. Le balisage complet apparaît ci-dessous :

[!code-aspx[Main](master-detail-filtering-with-two-dropdownlists-vb/samples/sample1.aspx)]

Prenez un moment pour tester la page `MasterDetailsDetails.aspx` dans un navigateur. À première vue, il peut sembler que tout fonctionne comme vous le souhaitez, mais qu’il y a un problème subtil. Lorsque vous choisissez une nouvelle catégorie, la `ProductsByCategory` DropDownList est mise à jour pour inclure ces produits pour la catégorie sélectionnée, mais le `ProductDetails` DetailsView continue d’afficher les informations sur le produit précédent. Le contrôle DetailsView est mis à jour lors du choix d’un autre produit pour la catégorie sélectionnée. En outre, si vous testez suffisamment de temps, vous constaterez que si vous choisissez continuellement de nouvelles catégories (par exemple, en choisissant des boissons dans le `Categories` DropDownList, puis des condiments, puis des friandises), l' `ProductDetails` DetailsView est actualisé.

Pour vous aider à concrétiser LAR ce problème, examinons un exemple spécifique. Lorsque vous accédez pour la première fois à la page, la catégorie boissons est sélectionnée et les produits associés sont chargés dans le `ProductsByCategory` DropDownList. Chai est le produit sélectionné et ses détails s’affichent dans le `ProductDetails` DetailsView, comme illustré à la figure 18.

[![les détails du produit sélectionné s’affichent dans un DetailsView](master-detail-filtering-with-two-dropdownlists-vb/_static/image53.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image52.png)

**Figure 18**: les détails du produit sélectionné s’affichent dans un DetailsView ([cliquez pour afficher l’image en plein écran](master-detail-filtering-with-two-dropdownlists-vb/_static/image54.png))

Si vous modifiez la sélection de catégorie de boissons en condiments, une publication (postback) se produit et le `ProductsByCategory` DropDownList est mis à jour en conséquence, mais le DetailsView affiche toujours les détails pour Chai.

[![les détails du produit précédemment sélectionné s’affichent toujours](master-detail-filtering-with-two-dropdownlists-vb/_static/image56.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image55.png)

**Figure 19**: les détails du produit précédemment sélectionné sont toujours affichés ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-vb/_static/image57.png))

Le choix d’un nouveau produit dans la liste actualise le DetailsView comme prévu. Si vous choisissez une nouvelle catégorie après avoir modifié le produit, le contrôle DetailsView ne s’actualise pas. Toutefois, si au lieu de choisir un nouveau produit, vous avez sélectionné une nouvelle catégorie, le contrôle DetailsView est actualisé. Que se passe-t-il dans le monde ?

Le problème est un problème de synchronisation dans le cycle de vie de la page. Chaque fois qu’une page est demandée, elle passe par un certain nombre d’étapes comme son rendu. Dans l’une de ces étapes, les contrôles ObjectDataSource vérifient si l’une de leurs valeurs `SelectParameters` a été modifiée. Si c’est le cas, le contrôle Web des données lié à l’ObjectDataSource sait qu’il doit actualiser son affichage. Par exemple, lorsqu’une nouvelle catégorie est sélectionnée, le `ProductsByCategoryDataSource` ObjectDataSource détecte que ses valeurs de paramètres ont changé et le `ProductsByCategory` DropDownList se lie de nouveau, en obtenant les produits pour la catégorie sélectionnée.

Le problème qui survient dans cette situation est que le point dans le cycle de vie de la page que le contrôle ObjectDataSources pour les paramètres modifiés se produit *avant* la reliaison des contrôles Web de données associés. Par conséquent, lors de la sélection d’une nouvelle catégorie, le `ProductsByCategoryDataSource` ObjectDataSource détecte une modification de sa valeur. Toutefois, l’ObjectDataSource utilisé par le `ProductDetails` DetailsView ne note pas de telles modifications, car le `ProductsByCategory` DropDownList n’a pas encore été relié. Plus tard dans le cycle de vie, le `ProductsByCategory` DropDownList est lié à son ObjectDataSource, en saisissant les produits de la catégorie nouvellement sélectionnée. Tandis que la valeur de la `ProductsByCategory` DropDownList a changé, le ObjectDataSource du `ProductDetails` DetailsView a déjà effectué la vérification de sa valeur de paramètre ; par conséquent, le contrôle DetailsView affiche ses résultats précédents. Cette interaction est illustrée à la figure 20.

[![la valeur DropDownList ProductsByCategory change après que l’ObjectDataSource de ProductDetails DetailsView a vérifié les modifications](master-detail-filtering-with-two-dropdownlists-vb/_static/image59.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image58.png)

**Figure 20**: la valeur de la `ProductsByCategory` DropDownList change après que l’ObjectDataSource du `ProductDetails` DetailsView a vérifié les modifications ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-vb/_static/image60.png))

Pour remédier à cela, nous devons lier explicitement la `ProductDetails` DetailsView une fois que le contrôle DropDownList `ProductsByCategory` a été lié. Pour ce faire, nous appelons la méthode `DataBind()` de `ProductDetails` DetailsView lorsque l’événement `DataBound` de `ProductsByCategory` DropDownList est déclenché. Ajoutez le code de gestionnaire d’événements suivant à la classe code-behind de la page `MasterDetailsDetails.aspx` (reportez-vous à la section «[définition par programmation des valeurs de paramètre ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)» pour une discussion sur l’ajout d’un gestionnaire d’événements) :

[!code-vb[Main](master-detail-filtering-with-two-dropdownlists-vb/samples/sample2.vb)]

Après l’ajout de cet appel explicite à la méthode `DataBind()` de `ProductDetails` DetailsView, le didacticiel fonctionne comme prévu. La figure 21 met en évidence la manière dont le problème précédent a été résolu.

[![le DetailsView ProductDetails est actualisé de manière explicite lorsque l’événement DataBound de ProductsByCategory DropDownList est déclenché](master-detail-filtering-with-two-dropdownlists-vb/_static/image62.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image61.png)

**Figure 21**: le `ProductDetails` DetailsView est actualisé de manière explicite lorsque l’événement `DataBound` de la `ProductsByCategory` DropDownList est déclenché ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-with-two-dropdownlists-vb/_static/image63.png))

## <a name="summary"></a>Récapitulatif

Le DropDownList constitue un élément d’interface utilisateur idéal pour les rapports maître/détail dans lesquels il existe une relation un-à-plusieurs entre les enregistrements maître et détail. Dans le didacticiel précédent, nous avons vu comment utiliser un seul DropDownList pour filtrer les produits affichés par la catégorie sélectionnée. Dans ce didacticiel, nous avons remplacé le contrôle GridView des produits par un contrôle DropDownList et utilisé un contrôle DetailsView pour afficher les détails du produit sélectionné. Les concepts abordés dans ce didacticiel peuvent être facilement étendus aux modèles de données impliquant plusieurs relations un-à-plusieurs, telles que les clients, les commandes et les Articles de commande. En général, vous pouvez toujours ajouter un DropDownList pour chacune des entités « une » dans les relations un-à-plusieurs.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel était Hilton Giesenow. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](master-detail-filtering-with-a-dropdownlist-vb.md)
> [Suivant](master-detail-filtering-across-two-pages-vb.md)
