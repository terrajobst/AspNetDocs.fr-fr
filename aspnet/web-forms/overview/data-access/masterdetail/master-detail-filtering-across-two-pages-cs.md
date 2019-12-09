---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
title: Filtrage maître/détail sur deux pages (C#) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons implémenter ce modèle à l’aide d’un GridView pour répertorier les fournisseurs dans la base de données. Chaque ligne de fournisseur dans le contrôle GridView contient une vie...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 552d2d50-fe73-4153-9a7f-2b379bec4625
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: ccb3bfa5f215ba6e65b8a10b40041d5c2896c7e3
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74620002"
---
# <a name="masterdetail-filtering-across-two-pages-c"></a>Filtrage maître/détail sur deux pages (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_9_CS.exe) ou [Télécharger le PDF](master-detail-filtering-across-two-pages-cs/_static/datatutorial09cs1.pdf)

> Dans ce didacticiel, nous allons implémenter ce modèle à l’aide d’un GridView pour répertorier les fournisseurs dans la base de données. Chaque ligne de fournisseur dans le contrôle GridView contient un lien Afficher les produits qui, lorsque vous cliquez dessus, permet à l’utilisateur d’accéder à une page distincte qui répertorie les produits pour le fournisseur sélectionné.

## <a name="introduction"></a>Introduction

Dans les deux didacticiels précédents, nous avons vu comment [afficher des rapports maître/détail dans une page Web unique à l’aide de DropDownList](master-detail-filtering-with-a-dropdownlist-cs.md) pour [afficher les enregistrements « maîtres » et un contrôle GridView ou DetailsView](master-detail-filtering-with-two-dropdownlists-cs.md) pour afficher les détails. Un autre modèle courant utilisé pour les rapports maître/détails consiste à avoir les enregistrements maîtres sur une page Web et les détails affichés sur un autre. Un site Web de forum, comme [les Forums ASP.net](https://forums.asp.net/), est un excellent exemple de ce modèle dans la pratique. Les Forums ASP.NET sont composés de différents forums Prise en main, Web Forms, des contrôles de présentation des données, etc. Chaque forum est composé de nombreux threads et chaque thread se compose d’un certain nombre de publications. Sur la page d’accueil des forums ASP.NET, les forums sont répertoriés. Cliquer sur un forum vous donne une page `ShowForum.aspx`, qui répertorie les threads de ce forum. De même, le fait de cliquer sur un thread vous amène à `ShowPost.aspx`, qui affiche les publications du thread sur lequel l’utilisateur a cliqué.

Dans ce didacticiel, nous allons implémenter ce modèle à l’aide d’un GridView pour répertorier les fournisseurs dans la base de données. Chaque ligne de fournisseur dans le contrôle GridView contient un lien Afficher les produits qui, lorsque vous cliquez dessus, permet à l’utilisateur d’accéder à une page distincte qui répertorie les produits pour le fournisseur sélectionné.

## <a name="step-1-addingsupplierlistmasteraspxandproductsforsupplierdetailsaspxpages-to-thefilteringfolder"></a>Étape 1 : ajout des pages`SupplierListMaster.aspx`et`ProductsForSupplierDetails.aspx`au dossier`Filtering`

Lors de la définition de la mise en page dans le troisième didacticiel, nous avons ajouté plusieurs pages « Starter » dans les dossiers `BasicReporting`, `Filtering`et `CustomFormatting`. Toutefois, nous n’avons pas ajouté de page de démarrage pour ce didacticiel à ce moment-là, prenez un moment pour ajouter deux nouvelles pages au dossier `Filtering` : `SupplierListMaster.aspx` et `ProductsForSupplierDetails.aspx`. `SupplierListMaster.aspx` répertorie les enregistrements « principaux » (fournisseurs), tandis que `ProductsForSupplierDetails.aspx` affiche les produits pour le fournisseur sélectionné.

Lorsque vous créez ces deux nouvelles pages, veillez à les associer à la page maître `Site.master`.

![Ajouter les pages SupplierListMaster. aspx et ProductsForSupplierDetails. aspx au dossier de filtrage](master-detail-filtering-across-two-pages-cs/_static/image1.png)

**Figure 1**: ajouter les pages `SupplierListMaster.aspx` et `ProductsForSupplierDetails.aspx` au dossier `Filtering`

En outre, lors de l’ajout de nouvelles pages au projet, veillez à mettre à jour le fichier de plan de site, `Web.sitemap`, en conséquence. Pour ce didacticiel, ajoutez simplement la page `SupplierListMaster.aspx` à la carte de site en utilisant le contenu XML suivant comme enfant de l’élément de filtrage des rapports `<siteMapNode>` :

[!code-xml[Main](master-detail-filtering-across-two-pages-cs/samples/sample1.xml)]

> [!NOTE]
> Vous pouvez aider à automatiser le processus de mise à jour du fichier de plan de site lors de l’ajout de nouvelles pages ASP.NET à l’aide de la [macro plan de site](http://odetocode.com/Blogs/scott/archive/2005/11/29/2537.aspx)Visual Studio gratuite de [K. Scott Allen](http://odetocode.com/Blogs/scott/).

## <a name="step-2-displaying-the-supplier-list-insupplierlistmasteraspx"></a>Étape 2 : affichage de la liste des fournisseurs dans`SupplierListMaster.aspx`

Avec les pages `SupplierListMaster.aspx` et `ProductsForSupplierDetails.aspx` créées, l’étape suivante consiste à créer le contrôle GridView des fournisseurs dans `SupplierListMaster.aspx`. Ajoutez un GridView à la page et liez-le à un nouvel ObjectDataSource. Cet ObjectDataSource doit utiliser la méthode `GetSuppliers()` de la classe `SuppliersBLL` pour retourner tous les fournisseurs.

[![sélectionnez la classe SuppliersBLL](master-detail-filtering-across-two-pages-cs/_static/image3.png)](master-detail-filtering-across-two-pages-cs/_static/image2.png)

**Figure 2**: sélectionner la classe de `SuppliersBLL` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image4.png))

[![configurer ObjectDataSource pour utiliser la méthode GetSuppliers ()](master-detail-filtering-across-two-pages-cs/_static/image6.png)](master-detail-filtering-across-two-pages-cs/_static/image5.png)

**Figure 3**: configurer ObjectDataSource pour utiliser la méthode `GetSuppliers()` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image7.png))

Nous devons inclure un lien intitulé Products View dans chaque ligne GridView qui, lorsque vous cliquez dessus, amène l’utilisateur à `ProductsForSupplierDetails.aspx` passant la valeur `SupplierID` de la ligne sélectionnée par le biais de la chaîne de chaîne. Par exemple, si l’utilisateur clique sur le lien Afficher les produits pour le fournisseur Tokyo Traders (qui a une valeur `SupplierID` de 4), il doit être envoyé à `ProductsForSupplierDetails.aspx?SupplierID=4`.

Pour ce faire, ajoutez un [HyperLinkField](https://msdn.microsoft.com/library/system.web.ui.webcontrols.hyperlinkfield.aspx) au GridView, qui ajoute un lien hypertexte à chaque ligne GridView. Commencez par cliquer sur le lien modifier les colonnes à partir de la balise active de GridView. Ensuite, sélectionnez le HyperLinkField dans la liste en haut à gauche, puis cliquez sur Ajouter pour inclure le HyperLinkField dans la liste de champs du contrôle GridView.

[![ajouter un HyperLinkField au GridView](master-detail-filtering-across-two-pages-cs/_static/image9.png)](master-detail-filtering-across-two-pages-cs/_static/image8.png)

**Figure 4**: ajouter un HyperLinkField à GridView ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image10.png))

Le HyperLinkField peut être configuré pour utiliser les mêmes valeurs de texte ou d’URL que le lien dans chaque ligne GridView, ou peut baser ces valeurs sur les valeurs de données liées à chaque ligne particulière. Pour spécifier une valeur statique sur toutes les lignes, utilisez les propriétés `Text` ou `NavigateUrl` de HyperLinkField. Étant donné que nous voulons que le texte du lien soit le même pour toutes les lignes, définissez la propriété `Text` du HyperLinkField pour afficher les produits.

[![définir la propriété Text de HyperLinkField pour afficher les produits](master-detail-filtering-across-two-pages-cs/_static/image12.png)](master-detail-filtering-across-two-pages-cs/_static/image11.png)

**Figure 5**: définir la propriété de `Text` de HyperLinkField pour afficher les produits ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image13.png))

Pour définir les valeurs de texte ou d’URL en fonction des données sous-jacentes liées à la ligne GridView, spécifiez les champs de données à partir desquels les valeurs de texte ou d’URL doivent être extraites dans les propriétés `DataTextField` ou `DataNavigateUrlFields`. `DataTextField` peut uniquement être défini sur un champ de données unique ; Toutefois, `DataNavigateUrlFields`peut avoir la valeur d’une liste de champs de données délimitée par des virgules. Nous devons souvent baser le texte ou l’URL sur une combinaison de la valeur du champ de données de la ligne actuelle et d’un balisage statique. Dans ce didacticiel, par exemple, nous voulons que l’URL des liens de HyperLinkField soit `ProductsForSupplierDetails.aspx?SupplierID=supplierID`, où *`supplierID`* correspond à la valeur de `SupplierID` row’s de chaque GridView. Notez que nous avons besoin des valeurs statiques et pilotées par les données ici : la partie `ProductsForSupplierDetails.aspx?SupplierID=` de l’URL du lien est statique, tandis que la partie *`supplierID`* est pilotée par les données, car sa valeur est la sienne `SupplierID` valeur de chaque ligne.

Pour indiquer une combinaison de valeurs statiques et pilotées par les données, utilisez les propriétés `DataTextFormatString` et `DataNavigateUrlFormatString`. Dans ces propriétés, entrez le balisage statique si nécessaire, puis utilisez le marqueur `{0}` où vous souhaitez que la valeur du champ spécifié dans les propriétés `DataTextField` ou `DataNavigateUrlFields` apparaisse. Si plusieurs champs sont spécifiés pour la propriété `DataNavigateUrlFields`, utilisez `{0}` où vous souhaitez que la première valeur de champ soit insérée, `{1}` pour la deuxième valeur de champ, et ainsi de suite.

En appliquant cela à notre didacticiel, nous devons définir la propriété `DataNavigateUrlFields` sur `SupplierID`, car il s’agit du champ de données dont nous devons personnaliser la valeur en fonction de chaque ligne, et la propriété `DataNavigateUrlFormatString` pour `ProductsForSupplierDetails.aspx?SupplierID={0}`.

[![configurer HyperLinkField de façon à inclure l’URL de lien appropriée en fonction du RéfFournisseur](master-detail-filtering-across-two-pages-cs/_static/image15.png)](master-detail-filtering-across-two-pages-cs/_static/image14.png)

**Figure 6**: configurer HyperLinkField pour inclure l’URL de lien appropriée en fonction du `SupplierID` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image16.png))

Après avoir ajouté le HyperLinkField, n’hésitez pas à personnaliser et à réorganiser les champs du contrôle GridView. Le balisage suivant montre le GridView une fois que j’ai effectué des personnalisations mineures au niveau des champs.

[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample2.aspx)]

Prenez un moment pour afficher la page `SupplierListMaster.aspx` dans un navigateur. Comme le montre la figure 7, la page répertorie actuellement tous les fournisseurs, y compris un lien Afficher les produits. Le fait de cliquer sur le lien Afficher les produits vous permet d’accéder `ProductsForSupplierDetails.aspx`, en passant le `SupplierID` du fournisseur dans la chaîne de chaîne.

[![chaque ligne fournisseur contient un lien Afficher les produits](master-detail-filtering-across-two-pages-cs/_static/image18.png)](master-detail-filtering-across-two-pages-cs/_static/image17.png)

**Figure 7**: chaque ligne de fournisseur contient un lien Afficher les produits ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image19.png))

## <a name="step-3-listing-the-suppliers-products-inproductsforsupplierdetailsaspx"></a>Étape 3 : énumération des produits du fournisseur dans`ProductsForSupplierDetails.aspx`

À ce stade, la page `SupplierListMaster.aspx` envoie des utilisateurs à `ProductsForSupplierDetails.aspx`en passant le `SupplierID` du fournisseur sélectionné dans la chaîne de chaîne. La dernière étape du didacticiel consiste à afficher les produits dans un GridView dans `ProductsForSupplierDetails.aspx` dont les `SupplierID` sont égales à la `SupplierID` passée via la chaîne de chaîne. Pour ce faire, ajoutez un GridView à la page `ProductsForSupplierDetails.aspx`, à l’aide d’un nouveau contrôle ObjectDataSource nommé `ProductsBySupplierDataSource` qui appelle la méthode `GetProductsBySupplierID(supplierID)` à partir de la classe `ProductsBLL`.

[![ajouter un nouvel ObjectDataSource nommé ProductsBySupplierDataSource](master-detail-filtering-across-two-pages-cs/_static/image21.png)](master-detail-filtering-across-two-pages-cs/_static/image20.png)

**Figure 8**: ajouter un nouvel ObjectDataSource nommé `ProductsBySupplierDataSource` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image22.png))

[![sélectionnez la classe ProductsBLL](master-detail-filtering-across-two-pages-cs/_static/image24.png)](master-detail-filtering-across-two-pages-cs/_static/image23.png)

**Figure 9**: sélectionner la classe de `ProductsBLL` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image25.png))

[![que ObjectDataSource appelle la méthode GetProductsBySupplierID (RéfFournisseur)](master-detail-filtering-across-two-pages-cs/_static/image27.png)](master-detail-filtering-across-two-pages-cs/_static/image26.png)

**Figure 10**: demander à ObjectDataSource d’appeler la méthode `GetProductsBySupplierID(supplierID)` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image28.png))

L’étape finale de l’Assistant Configuration de la source de données nous invite à fournir la source du paramètre *`supplierID`* de la méthode `GetProductsBySupplierID(supplierID)`. Pour utiliser la valeur QueryString, définissez la source du paramètre sur QueryString et entrez le nom de la valeur QueryString à utiliser dans la zone de texte QueryStringField (`SupplierID`).

[![remplir la valeur de paramètre RéfFournisseur à partir de la valeur QueryString RéfFournisseur](master-detail-filtering-across-two-pages-cs/_static/image30.png)](master-detail-filtering-across-two-pages-cs/_static/image29.png)

**Figure 11**: remplir la valeur de paramètre *`supplierID`* à partir de la valeur QueryString `SupplierID` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image31.png))

C’est aussi simple que cela ! La figure 12 illustre la page de `ProductsForSupplierDetails.aspx` lorsqu’elle est visitée en cliquant sur le lien Tokyo traders à partir de `SupplierListMaster.aspx`.

[![les produits fournis par Tokyo traders sont affichés](master-detail-filtering-across-two-pages-cs/_static/image33.png)](master-detail-filtering-across-two-pages-cs/_static/image32.png)

**Figure 12**: les produits fournis par Tokyo traders sont affichés ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image34.png))

## <a name="displaying-supplier-information-inproductsforsupplierdetailsaspx"></a>Affichage des informations sur les fournisseurs dans`ProductsForSupplierDetails.aspx`

Comme le montre la figure 12, la page `ProductsForSupplierDetails.aspx` répertorie simplement les produits fournis par le `SupplierID` spécifié dans QueryString. Une personne directement envoyée à cette page ne sait pas que la figure 12 montre les produits de Tokyo Traders. Pour y remédier, nous pouvons également afficher des informations sur les fournisseurs dans cette page.

Commencez par ajouter un FormView au-dessus du GridView Products. Créez un nouveau contrôle ObjectDataSource nommé `SuppliersDataSource` qui appelle la méthode `GetSupplierBySupplierID(supplierID)` de la classe `SuppliersBLL`.

[![sélectionnez la classe SuppliersBLL](master-detail-filtering-across-two-pages-cs/_static/image36.png)](master-detail-filtering-across-two-pages-cs/_static/image35.png)

**Figure 13**: sélectionner la classe de `SuppliersBLL` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image37.png))

[![que ObjectDataSource appelle la méthode GetSupplierBySupplierID (RéfFournisseur)](master-detail-filtering-across-two-pages-cs/_static/image39.png)](master-detail-filtering-across-two-pages-cs/_static/image38.png)

**Figure 14**: demander à ObjectDataSource d’appeler la méthode `GetSupplierBySupplierID(supplierID)` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image40.png))

Comme pour le `ProductsBySupplierDataSource`, le paramètre *`supplierID`* est affecté à la valeur de la chaîne QueryString `SupplierID`.

[![remplir la valeur de paramètre RéfFournisseur à partir de la valeur QueryString RéfFournisseur](master-detail-filtering-across-two-pages-cs/_static/image42.png)](master-detail-filtering-across-two-pages-cs/_static/image41.png)

**Figure 15**: remplir la valeur de paramètre *`supplierID`* à partir de la valeur QueryString `SupplierID` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image43.png))

Lors de la liaison du FormView au ObjectDataSource dans le Mode Création, Visual Studio crée automatiquement les `ItemTemplate`, `InsertItemTemplate`et `EditItemTemplate` du FormView avec les contrôles Web étiquette et zone de texte pour chacun des champs de données retournés par ObjectDataSource. Étant donné que nous voulons juste afficher les informations des fournisseurs, n’hésitez pas à supprimer les `InsertItemTemplate` et les `EditItemTemplate`. Modifiez ensuite le ItemTemplate afin qu’il affiche le nom de la société du fournisseur dans un élément `<h3>` et l’adresse, la ville, le pays et le numéro de téléphone sous le nom de la société. Vous pouvez également définir manuellement le `DataSourceID` du FormView et créer le balisage de `ItemTemplate`, comme nous l’avons fait dans le didacticiel «[affichage des données avec ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)».

Une fois ces modifications apportées, le balisage déclaratif du FormView doit ressembler à ce qui suit :

[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample3.aspx)]

La figure 16 illustre une capture d’écran de la page de `ProductsForSupplierDetails.aspx` une fois que les informations sur le fournisseur détaillées ci-dessus ont été incluses.

[![la liste des produits comprend un résumé du fournisseur.](master-detail-filtering-across-two-pages-cs/_static/image45.png)](master-detail-filtering-across-two-pages-cs/_static/image44.png)

**Figure 16**: la liste des produits comprend un résumé du fournisseur ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image46.png))

## <a name="applying-the-final-touches-for-theproductsforsupplierdetailsaspxui"></a>Application des touches finales pour l’interface utilisateur`ProductsForSupplierDetails.aspx`

Pour améliorer l’expérience utilisateur pour ce rapport, nous devons effectuer quelques ajouts à la page `ProductsForSupplierDetails.aspx`. Actuellement, la seule façon d’accéder à la liste des fournisseurs à partir de la page de `ProductsForSupplierDetails.aspx` est de cliquer sur le bouton précédent du navigateur. Nous allons ajouter un contrôle de lien hypertexte à la page `ProductsForSupplierDetails.aspx` qui renvoie à `SupplierListMaster.aspx`, ce qui permet à l’utilisateur de revenir à la liste principale.

[![ajouter un contrôle de lien hypertexte pour reconnecter l’utilisateur à SupplierListMaster. aspx](master-detail-filtering-across-two-pages-cs/_static/image48.png)](master-detail-filtering-across-two-pages-cs/_static/image47.png)

**Figure 17**: ajouter un contrôle de lien hypertexte pour reconnecter l’utilisateur à `SupplierListMaster.aspx` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image49.png))

Si l’utilisateur clique sur le lien Afficher les produits pour un fournisseur qui n’a pas de produits, le `ProductsBySupplierDataSource` ObjectDataSource dans `ProductsForSupplierDetails.aspx` ne retourne aucun résultat. Le GridView lié à ObjectDataSource n’affiche aucun balisage résultant dans une zone vide de la page dans le navigateur de l’utilisateur. Pour communiquer plus clairement à l’utilisateur qu’aucun produit n’est associé au fournisseur sélectionné, nous pouvons définir la propriété de `EmptyDataText` du contrôle GridView sur le message que nous souhaitons afficher lorsqu’une telle situation se produit. J’ai défini cette propriété sur « aucun produit n’est fourni par ce fournisseur »

Par défaut, tous les fournisseurs de la base de données Northwind fournissent au moins un produit. Toutefois, pour ce didacticiel, j’ai modifié manuellement la table `Products` afin que le fournisseur escargots nouveaux ne soit plus associé à aucun produit. La figure 18 affiche la page de détails de escargots nouveaux après cette modification.

[![utilisateurs sont informés que le fournisseur ne fournit aucun produit](master-detail-filtering-across-two-pages-cs/_static/image51.png)](master-detail-filtering-across-two-pages-cs/_static/image50.png)

**Figure 18**: les utilisateurs sont informés que le fournisseur ne fournit aucun produit ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-across-two-pages-cs/_static/image52.png))

## <a name="summary"></a>Récapitulatif

Bien que les rapports maître/détail puissent afficher à la fois les enregistrements maître et détail sur une seule page, dans de nombreux sites Web, ils sont séparés sur deux pages Web. Dans ce didacticiel, nous avons vu comment implémenter ce rapport maître/détail en faisant en sorte que les fournisseurs soient listés dans un contrôle GridView dans la page Web « master » et les produits associés figurant dans la page « Détails ». Chaque ligne de fournisseur dans la page Web principale contient un lien vers la page de détails qui a été transmise le long de la valeur `SupplierID` de la ligne. De tels liens spécifiques à la ligne peuvent être ajoutés facilement à l’aide de l’HyperLinkField du contrôle GridView.

Dans la page de détails, la récupération de ces produits pour le fournisseur spécifié a été effectuée en appelant la méthode `GetProductsBySupplierID(supplierID)` de la classe `ProductsBLL`. La valeur du paramètre *`supplierID`* a été spécifiée de manière déclarative à l’aide de QueryString comme source du paramètre. Nous avons également vu comment afficher les détails du fournisseur dans la page de détails à l’aide d’un FormView.

Le [prochain didacticiel](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) est le dernier dans les rapports maître/détail. Nous allons voir comment afficher une liste de produits dans un contrôle GridView où chaque ligne a un bouton Sélectionner. Le fait de cliquer sur le bouton Sélectionner affiche les détails de ce produit dans un contrôle DetailsView sur la même page.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel était Hilton Giesenow. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](master-detail-filtering-with-two-dropdownlists-cs.md)
> [Suivant](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
