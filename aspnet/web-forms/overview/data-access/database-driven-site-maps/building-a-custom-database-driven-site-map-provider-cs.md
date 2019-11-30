---
uid: web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-cs
title: Création d’un fournisseur de plan de site personnalisé baséC#sur une base de données () | Microsoft Docs
author: rick-anderson
description: Le fournisseur de plan de site par défaut dans ASP.NET 2,0 récupère ses données à partir d’un fichier XML statique. Alors que le fournisseur basé sur XML est adapté à de nombreux petits et moyennes SIZ...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: 04b7591d-106f-4f05-87e9-d416cb65a8a6
msc.legacyurl: /web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-cs
msc.type: authoredcontent
ms.openlocfilehash: a3e27b37703b12c9796e8516f0d805aef1fdf8d8
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74637259"
---
# <a name="building-a-custom-database-driven-site-map-provider-c"></a>Création d’un fournisseur de plan de site personnalisé piloté par une base de données (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_62_CS.zip) ou [Télécharger le PDF](building-a-custom-database-driven-site-map-provider-cs/_static/datatutorial62cs1.pdf)

> Le fournisseur de plan de site par défaut dans ASP.NET 2,0 récupère ses données à partir d’un fichier XML statique. Bien que le fournisseur basé sur XML soit adapté à de nombreux sites Web de petite et moyenne taille, les applications Web plus volumineuses requièrent un plan de site plus dynamique. Dans ce didacticiel, nous allons créer un fournisseur de plan de site personnalisé qui récupère ses données à partir de la couche de logique métier, qui à son tour récupère les données de la base de données.

## <a name="introduction"></a>Introduction

La fonctionnalité de plan de site ASP.NET 2,0 permet à un développeur de pages de définir un plan de site d’application Web dans un support permanent, comme dans un fichier XML. Une fois définis, les données de plan de site sont accessibles par programme par le biais de la [classe`SiteMap`](https://msdn.microsoft.com/library/system.web.sitemap.aspx) dans l' [espace de noms`System.Web`](https://msdn.microsoft.com/library/system.web.aspx) ou par l’intermédiaire de divers contrôles Web de navigation, tels que les contrôles SiteMapPath, menu et TreeView. Le système de plan de site utilise le [modèle de fournisseur](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) afin que différentes implémentations de sérialisation de plan de site puissent être créées et connectées à une application Web. Le fournisseur de plan de site par défaut fourni avec ASP.NET 2,0 rend persistante la structure de plan de site dans un fichier XML. De retour dans le didacticiel sur les [pages maîtres et la navigation dans les sites,](../introduction/master-pages-and-site-navigation-cs.md) nous avons créé un fichier nommé `Web.sitemap` qui contenait cette structure et mis à jour son code XML avec chaque nouvelle section du didacticiel.

Le fournisseur de plan de site XML par défaut fonctionne bien si la structure du plan du site est relativement statique, par exemple pour ces didacticiels. Toutefois, dans de nombreux scénarios, un plan de site plus dynamique est nécessaire. Prenons l’exemple du plan de site illustré dans la figure 1, où chaque catégorie et produit s’affiche sous la forme de sections dans la structure des sites Web. Avec ce plan de site, la page Web correspondant au nœud racine peut répertorier toutes les catégories, tandis que la visite d’une page Web de catégorie s particulière répertorie les produits de la catégorie et l’affichage d’une page Web de produit spécifique.

[![les catégories et les produits qui compensent la structure du plan du site](building-a-custom-database-driven-site-map-provider-cs/_static/image1.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image1.png)

**Figure 1**: catégories et produits qui comportent la structure du plan du site ([cliquez pour afficher l’image en taille réelle](building-a-custom-database-driven-site-map-provider-cs/_static/image2.png))

Bien que cette structure basée sur le produit et la catégorie puisse être codée en dur dans le fichier `Web.sitemap`, le fichier doit être mis à jour chaque fois qu’une catégorie ou un produit a été ajouté, supprimé ou renommé. Par conséquent, la maintenance du plan de site est considérablement simplifiée si sa structure a été extraite de la base de données ou, dans l’idéal, à partir de la couche de logique métier de l’architecture des applications. Ainsi, à mesure que des produits et des catégories ont été ajoutés, renommés ou supprimés, le plan de site est automatiquement mis à jour pour refléter ces modifications.

Étant donné que la sérialisation de plan de site ASP.NET 2,0 s est construite au-dessus du modèle de fournisseur, nous pouvons créer notre propre fournisseur de plan de site personnalisé qui extrait ses données d’un autre magasin de données, tel que la base de données ou l’architecture. Dans ce didacticiel, nous allons créer un fournisseur personnalisé qui récupère ses données à partir de la couche BLL. Commençons !

> [!NOTE]
> Le fournisseur de plan de site personnalisé créé dans ce didacticiel est étroitement couplé à l’architecture et au modèle de données de l’application. Jeff Prosise [stocke les plans de site dans SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) et [le fournisseur de plan de site SQL que vous avez attendu pour](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx) les articles, en examinant une approche généralisée du stockage des données de plan de site dans SQL Server.

## <a name="step-1-creating-the-custom-site-map-provider-web-pages"></a>Étape 1 : création des pages Web du fournisseur de plan de site personnalisé

Avant de commencer à créer un fournisseur de plan de site personnalisé, nous allons d’abord ajouter les pages ASP.NET dont nous aurons besoin pour ce didacticiel. Commencez par ajouter un nouveau dossier nommé `SiteMapProvider`. Ajoutez ensuite les pages ASP.NET suivantes à ce dossier, en veillant à associer chaque page à la page maître `Site.master` :

- `Default.aspx`
- `ProductsByCategory.aspx`
- `ProductDetails.aspx`

Ajoutez également un sous-dossier `CustomProviders` au dossier `App_Code`.

![Ajouter les pages ASP.NET pour les didacticiels relatifs au fournisseur de plan de site](building-a-custom-database-driven-site-map-provider-cs/_static/image2.gif)

**Figure 2**: ajouter les pages ASP.net pour les didacticiels relatifs au fournisseur de plan de site

Étant donné qu’il n’y a qu’un seul didacticiel pour cette section, nous n’avons pas besoin de `Default.aspx` pour répertorier les didacticiels de la section. Au lieu de cela, `Default.aspx` affichera les catégories dans un contrôle GridView. Nous allons l’aborder à l’étape 2.

Ensuite, mettez à jour `Web.sitemap` pour inclure une référence à la page `Default.aspx`. Plus précisément, ajoutez le balisage suivant après la `<siteMapNode>`de mise en cache :

[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample1.xml)]

Après avoir mis à jour `Web.sitemap`, prenez un moment pour afficher le site Web des didacticiels via un navigateur. Le menu sur la gauche comprend désormais un élément pour le seul didacticiel du fournisseur de plan de site.

![Le plan de site comprend désormais une entrée pour le didacticiel du fournisseur de plan de site](building-a-custom-database-driven-site-map-provider-cs/_static/image3.gif)

**Figure 3**: le plan de site contient désormais une entrée pour le didacticiel du fournisseur de plan de site

Ce didacticiel s’intéresse principalement à la création d’un fournisseur de plan de site personnalisé et à la configuration d’une application Web pour utiliser ce fournisseur. En particulier, nous allons créer un fournisseur qui retourne un plan de site qui comprend un nœud racine avec un nœud pour chaque catégorie et produit, comme illustré à la figure 1. En général, chaque nœud du plan de site peut spécifier une URL. Pour notre plan de site, l’URL des nœuds racine est `~/SiteMapProvider/Default.aspx`, qui répertorie toutes les catégories dans la base de données. Chaque nœud de catégorie dans le plan de site aura une URL qui pointe vers `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID`, qui répertorie tous les produits de la *catégorie CategoryID*spécifiée. Enfin, chaque nœud de plan de site de produit pointera vers `~/SiteMapProvider/ProductDetails.aspx?ProductID=productID`, qui affichera les détails spécifiques du produit.

Pour commencer, nous devons créer les pages `Default.aspx`, `ProductsByCategory.aspx`et `ProductDetails.aspx`. Ces pages sont remplies aux étapes 2, 3 et 4, respectivement. Dans la mesure où la poussée de ce didacticiel est sur les fournisseurs de plan de site, et comme les didacticiels précédents ont abordé la création de ces types de rapports maîtres/détails de plusieurs pages, nous nous contenterons de suivre les étapes 2 à 4. Si vous avez besoin d’un actualisateur pour créer des rapports maître/détail qui s’étendent sur plusieurs pages, reportez-vous au didacticiel [maître/détail pour le filtrage dans deux pages](../masterdetail/master-detail-filtering-across-two-pages-cs.md) .

## <a name="step-2-displaying-a-list-of-categories"></a>Étape 2 : affichage d’une liste de catégories

Ouvrez la page `Default.aspx` dans le dossier `SiteMapProvider` et faites glisser un contrôle GridView de la boîte à outils vers le concepteur, en affectant à sa `ID` la valeur `Categories`. À partir de la balise active GridView s, liez-le à un nouvel ObjectDataSource nommé `CategoriesDataSource` et configurez-le de sorte qu’il récupère ses données à l’aide de la méthode `CategoriesBLL` classe s `GetCategories`. Étant donné que ce GridView affiche uniquement les catégories et ne fournit pas de fonctionnalités de modification des données, définissez les listes déroulantes dans les onglets mettre à jour, insérer et supprimer sur (aucun).

[![configurer ObjectDataSource pour retourner des catégories à l’aide de la méthode GetCategories](building-a-custom-database-driven-site-map-provider-cs/_static/image4.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image3.png)

**Figure 4**: configurer ObjectDataSource pour retourner des catégories à l’aide de la méthode `GetCategories` ([cliquez pour afficher l’image en taille réelle](building-a-custom-database-driven-site-map-provider-cs/_static/image4.png))

[![définir les listes déroulantes dans les onglets mettre à jour, insérer et supprimer sur (aucun)](building-a-custom-database-driven-site-map-provider-cs/_static/image5.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image5.png)

**Figure 5**: définir les listes déroulantes des onglets mettre à jour, insérer et supprimer sur (aucun) ([cliquez pour afficher l’image en taille réelle](building-a-custom-database-driven-site-map-provider-cs/_static/image6.png))

À l’issue de l’exécution de l’Assistant Configuration de la source de données, Visual Studio ajoute un BoundField pour `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`et `BrochurePath`. Modifiez le contrôle GridView afin qu’il ne contienne que les `CategoryName` et `Description` BoundFields et mettez à jour la propriété `CategoryName` BoundField s `HeaderText` en Category.

Ensuite, ajoutez un HyperLinkField et positionnez-le pour qu’il s’agit du champ le plus à gauche. Affectez à la propriété `DataNavigateUrlFields` la valeur `CategoryID` et à la propriété `DataNavigateUrlFormatString` la valeur `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID={0}`. Définissez la propriété `Text` pour afficher les produits.

![Ajouter un HyperLinkField à la catégorie GridView](building-a-custom-database-driven-site-map-provider-cs/_static/image6.gif)

**Figure 6**: ajouter un HyperLinkField à la `Categories` GridView

Après avoir créé l’ObjectDataSource et personnalisé les champs GridView s, les deux contrôles de balisage déclaratifs se présentent comme suit :

[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample2.aspx)]

La figure 7 illustre `Default.aspx` affichées dans un navigateur. Le fait de cliquer sur un lien Afficher les produits des catégories vous amène à `ProductsByCategory.aspx?CategoryID=categoryID`, que nous allons générer à l’étape 3.

[![chaque catégorie est indiquée avec un lien Afficher les produits](building-a-custom-database-driven-site-map-provider-cs/_static/image7.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image7.png)

**Figure 7**: chaque catégorie est indiquée avec un lien Afficher les produits ([cliquez pour afficher l’image en taille réelle](building-a-custom-database-driven-site-map-provider-cs/_static/image8.png))

## <a name="step-3-listing-the-selected-category-s-products"></a>Étape 3 : affichage des produits des catégories sélectionnées

Ouvrez la page `ProductsByCategory.aspx` et ajoutez un GridView, en lui attribuant un nom `ProductsByCategory`. À partir de sa balise active, liez le contrôle GridView à un nouvel ObjectDataSource nommé `ProductsByCategoryDataSource`. Configurez le ObjectDataSource pour utiliser la méthode `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` et définissez les listes déroulantes sur (aucune) dans les onglets mettre à jour, insérer et supprimer.

[![utilisez la méthode ProductsBLL de la classe s GetProductsByCategoryID (categoryID)](building-a-custom-database-driven-site-map-provider-cs/_static/image8.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image9.png)

**Figure 8**: utiliser la méthode `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` ([cliquez pour afficher l’image en taille réelle](building-a-custom-database-driven-site-map-provider-cs/_static/image10.png))

La dernière étape de l’Assistant Configuration de la source de données vous invite à entrer une source de paramètres pour *CategoryID*. Étant donné que ces informations sont transmises via le champ QueryString `CategoryID`, sélectionnez QueryString dans la liste déroulante et entrez CategoryID dans la zone de texte QueryStringField, comme illustré à la figure 9. Cliquez sur Terminer pour terminer l'Assistant.

[![utiliser le champ CategoryID QueryString pour le paramètre categoryID](building-a-custom-database-driven-site-map-provider-cs/_static/image9.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image11.png)

**Figure 9**: utilisation du champ `CategoryID` QueryString pour le paramètre *CategoryID* ([cliquez pour afficher l’image en taille réelle](building-a-custom-database-driven-site-map-provider-cs/_static/image12.png))

Une fois l’Assistant terminé, Visual Studio ajoute les BoundFields correspondants et un CheckBoxField au GridView pour les champs de données de produit. Supprimez tout sauf les `ProductName`, `UnitPrice`et `SupplierName` BoundFields. Personnalisez ces trois propriétés de `HeaderText` BoundFields pour lire respectivement le produit, le prix et le fournisseur. Mettez en forme la `UnitPrice` BoundField comme une devise.

Ensuite, ajoutez un HyperLinkField et déplacez-le à la position la plus à gauche. Définissez sa propriété `Text` pour afficher les détails, sa propriété `DataNavigateUrlFields` à `ProductID`, et sa propriété `DataNavigateUrlFormatString` à `~/SiteMapProvider/ProductDetails.aspx?ProductID={0}`.

![Ajouter un HyperLinkField détails de la vue qui pointe vers ProductDetails. aspx](building-a-custom-database-driven-site-map-provider-cs/_static/image10.gif)

**Figure 10**: ajouter un HyperLinkField détails de la vue qui pointe vers `ProductDetails.aspx`

Après avoir effectué ces personnalisations, les balises déclaratives GridView et ObjectDataSource doivent ressembler à ce qui suit :

[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample3.aspx)]

Revenez à l’affichage des `Default.aspx` via un navigateur et cliquez sur le lien Afficher les produits pour les boissons. Cela vous permet d’accéder à `ProductsByCategory.aspx?CategoryID=1`, en affichant les noms, les prix et les fournisseurs des produits de la base de données Northwind qui appartiennent à la catégorie boissons (voir figure 11). N’hésitez pas à améliorer cette page pour inclure un lien permettant de renvoyer des utilisateurs vers la page de liste de catégories (`Default.aspx`) et un contrôle DetailsView ou FormView qui affiche le nom et la description de la catégorie sélectionnée.

[![les noms des boissons, les prix et les fournisseurs sont affichés](building-a-custom-database-driven-site-map-provider-cs/_static/image11.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image13.png)

**Figure 11**: affichage des noms des boissons, des prix et des fournisseurs ([cliquez pour afficher l’image en taille réelle](building-a-custom-database-driven-site-map-provider-cs/_static/image14.png))

## <a name="step-4-showing-a-product-s-details"></a>Étape 4 : présentation des détails du produit

La dernière page, `ProductDetails.aspx`, affiche les détails des produits sélectionnés. Ouvrez `ProductDetails.aspx` et faites glisser un DetailsView de la boîte à outils vers le concepteur. Affectez à la propriété DetailsView s `ID` la valeur `ProductInfo` et effacez ses valeurs de propriété `Height` et `Width`. À partir de sa balise active, liez le DetailsView à un nouvel ObjectDataSource nommé `ProductDataSource`, en configurant ObjectDataSource pour extraire ses données de la méthode de `ProductsBLL` classe s `GetProductByProductID(productID)`. Comme pour les pages Web précédentes créées aux étapes 2 et 3, définissez les listes déroulantes dans les onglets mettre à jour, insérer et supprimer sur (aucun).

[![configurer ObjectDataSource pour utiliser la méthode GetProductByProductID (productID)](building-a-custom-database-driven-site-map-provider-cs/_static/image12.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image15.png)

**Figure 12**: configurer ObjectDataSource pour utiliser la méthode `GetProductByProductID(productID)` ([cliquez pour afficher l’image en taille réelle](building-a-custom-database-driven-site-map-provider-cs/_static/image16.png))

La dernière étape de l’Assistant Configuration de la source de données vous invite à entrer la source du paramètre *ProductID* . Étant donné que ces données proviennent du champ QueryString `ProductID`, définissez la liste déroulante sur QueryString et la zone de texte QueryStringField sur ProductID. Enfin, cliquez sur le bouton Terminer pour terminer l’Assistant.

[![configurez le paramètre productID pour extraire sa valeur du champ ProductID QueryString](building-a-custom-database-driven-site-map-provider-cs/_static/image13.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image17.png)

**Figure 13**: configurer le paramètre *ProductID* pour extraire sa valeur du champ `ProductID` QueryString ([cliquez pour afficher l’image en taille réelle](building-a-custom-database-driven-site-map-provider-cs/_static/image18.png))

À l’issue de l’exécution de l’Assistant Configuration de la source de données, Visual Studio crée les BoundFields correspondants et un CheckBoxField dans le DetailsView pour les champs de données de produit. Supprimez le `ProductID`, `SupplierID`et `CategoryID` BoundFields et configurez les champs restants à votre convenance. Après quelques configurations esthétiques, le balisage déclaratif de mes DetailsView et ObjectDataSource ressemble à ce qui suit :

[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample4.aspx)]

Pour tester cette page, revenez à `Default.aspx` et cliquez sur Afficher les produits pour la catégorie boissons. Dans la liste des produits de boisson, cliquez sur le lien Afficher les détails pour le thé chai. Cela vous permet d’accéder à `ProductDetails.aspx?ProductID=1`, qui affiche les détails de la thé chai (voir figure 14).

[![Chai thé, le fournisseur, la catégorie, le prix et d’autres informations s’affichent](building-a-custom-database-driven-site-map-provider-cs/_static/image14.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image19.png)

**Figure 14**: le fournisseur, la catégorie, le prix et d’autres informations du thé chai sont affichés ([cliquez pour afficher l’image en taille réelle](building-a-custom-database-driven-site-map-provider-cs/_static/image20.png))

## <a name="step-5-understanding-the-inner-workings-of-a-site-map-provider"></a>Étape 5 : comprendre le fonctionnement interne d’un fournisseur de plan de site

Le plan de site est représenté dans la mémoire du serveur Web sous la forme d’une collection d’instances de `SiteMapNode` formant une hiérarchie. Il doit y avoir exactement une racine, tous les nœuds non racine doivent avoir un seul nœud parent, et tous les nœuds peuvent avoir un nombre arbitraire d’enfants. Chaque objet `SiteMapNode` représente une section dans la structure du site Web. ces sections comportent généralement une page Web correspondante. Par conséquent, la [classe`SiteMapNode`](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) possède des propriétés telles que `Title`, `Url`et `Description`, qui fournissent des informations pour la section que le `SiteMapNode` représente. Il existe également une propriété `Key` qui identifie de façon unique chaque `SiteMapNode` dans la hiérarchie, ainsi que les propriétés utilisées pour établir cette hiérarchie `ChildNodes`, `ParentNode`, `NextSibling`, `PreviousSibling`, etc.

La figure 15 illustre la structure générale du plan de site de la figure 1, mais avec les détails de l’implémentation esquissés plus en détail.

[![chaque SiteMapNode a des propriétés telles que titre, URL, clé, etc.](building-a-custom-database-driven-site-map-provider-cs/_static/image16.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image15.gif)

**Figure 15**: chaque `SiteMapNode` possède des propriétés comme `Title`, `Url`, `Key`, etc. ([cliquez pour afficher l’image en taille réelle](building-a-custom-database-driven-site-map-provider-cs/_static/image17.gif))

Le plan de site est accessible via la [classe`SiteMap`](https://msdn.microsoft.com/library/system.web.sitemap.aspx) de l' [espace de noms`System.Web`](https://msdn.microsoft.com/library/system.web.aspx). Cette classe s `RootNode` propriété retourne l’instance de `SiteMapNode` racine du plan du site. `CurrentNode` retourne le `SiteMapNode` dont la propriété `Url` correspond à l’URL de la page actuellement demandée. Cette classe est utilisée en interne par les contrôles Web de navigation ASP.NET 2,0 s.

Lors de l’accès aux propriétés de la classe `SiteMap`, elle doit sérialiser la structure du plan de site à partir d’un support persistant en mémoire. Toutefois, la logique de sérialisation du plan de site n’est pas codée en dur dans la classe `SiteMap`. Au moment de l’exécution, la classe `SiteMap` détermine le *fournisseur* de plan de site à utiliser pour la sérialisation. Par défaut, la [classe`XmlSiteMapProvider`](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx) est utilisée, qui lit la structure du plan de site à partir d’un fichier XML correctement mis en forme. Toutefois, avec un peu de travail, nous pouvons créer notre propre fournisseur de plan de site personnalisé.

Tous les fournisseurs de plan de site doivent être dérivés de la [classe`SiteMapProvider`](https://msdn.microsoft.com/library/system.web.sitemapprovider.aspx), qui comprend les méthodes et propriétés essentielles requises pour les fournisseurs de plan de site, mais omet un grand nombre des détails d’implémentation. Une deuxième classe, [`StaticSiteMapProvider`](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.aspx), étend la classe `SiteMapProvider` et contient une implémentation plus robuste des fonctionnalités nécessaires. En interne, le `StaticSiteMapProvider` stocke les instances de `SiteMapNode` du plan de site dans un `Hashtable` et fournit des méthodes telles que `AddNode(child, parent)`, `RemoveNode(siteMapNode),` et `Clear()` qui ajoutent et suppriment des `SiteMapNode` dans le `Hashtable`interne. `XmlSiteMapProvider` est dérivé de `StaticSiteMapProvider`.

Lors de la création d’un fournisseur de plan de site personnalisé qui étend `StaticSiteMapProvider`, il existe deux méthodes abstraites qui doivent être remplacées : [`BuildSiteMap`](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.buildsitemap.aspx) et [`GetRootNodeCore`](https://msdn.microsoft.com/library/system.web.sitemapprovider.getrootnodecore.aspx). `BuildSiteMap`, comme son nom l’indique, est responsable du chargement de la structure de plan de site à partir d’un stockage persistant et de sa construction en mémoire. `GetRootNodeCore` retourne le nœud racine dans le plan de site.

Pour qu’une application Web puisse utiliser un fournisseur de plan de site, elle doit être inscrite dans la configuration de l’application. Par défaut, la classe `XmlSiteMapProvider` est inscrite à l’aide du nom `AspNetXmlSiteMapProvider`. Pour inscrire des fournisseurs de plan de site supplémentaires, ajoutez le balisage suivant à `Web.config`:

[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample5.xml)]

La valeur *Name* assigne un nom explicite au fournisseur tandis que *type* spécifie le nom de type complet du fournisseur de plan de site. Nous allons explorer les valeurs concrètes pour le *nom* et les valeurs de *type* à l’étape 7, une fois que nous avons créé notre fournisseur de plan de site personnalisé.

La classe du fournisseur de plan de site est instanciée la première fois qu’elle est accessible à partir de la classe `SiteMap` et reste en mémoire pendant toute la durée de vie de l’application Web. Étant donné qu’il n’existe qu’une seule instance du fournisseur de plan de site qui peut être appelée à partir de plusieurs visiteurs de sites Web simultanés, il est impératif que les méthodes du fournisseur soient *thread-safe*.

Pour des raisons de performances et d’évolutivité, il est important de mettre en cache la structure de plan de site en mémoire et de retourner cette structure mise en cache plutôt que de la recréer chaque fois que la méthode `BuildSiteMap` est appelée. `BuildSiteMap` peut être appelé plusieurs fois par demande de page par utilisateur, en fonction des contrôles de navigation en cours d’utilisation sur la page et de la profondeur de la structure de plan de site. Dans tous les cas, si la structure de plan du site n’est pas mise en cache dans `BuildSiteMap`, chaque fois qu’elle est appelée, nous aurions besoin de récupérer à nouveau les informations sur les produits et les catégories à partir de l’architecture (ce qui entraînerait une requête dans la base de données). Comme nous l’avons vu dans les didacticiels de mise en cache précédents, les données mises en cache peuvent devenir obsolètes. Pour combattre cela, nous pouvons utiliser des expirations basées sur la dépendance de l’heure ou du cache SQL.

> [!NOTE]
> Un fournisseur de plan de site peut éventuellement substituer la [méthode`Initialize`](https://msdn.microsoft.com/library/system.web.sitemapprovider.initialize.aspx). `Initialize` est appelé lorsque le fournisseur de plan de site est instancié pour la première fois et reçoit tous les attributs personnalisés affectés au fournisseur dans `Web.config` dans l’élément `<add>` comme : `<add name="name" type="type" customAttribute="value" />`. Cela est utile si vous souhaitez autoriser un développeur de pages à spécifier différents paramètres liés au fournisseur de plan de site sans avoir à modifier le code du fournisseur. Par exemple, si nous lisons la catégorie et les données des produits directement à partir de la base de données, par opposition à l’architecture, nous voulons que le développeur de la page spécifie la chaîne de connexion de la base de données via `Web.config` plutôt que d’utiliser une valeur codée en dur dans le code du fournisseur. Le fournisseur de plan de site personnalisé que nous allons générer à l’étape 6 ne remplace pas cette `Initialize` méthode. Pour obtenir un exemple d’utilisation de la méthode `Initialize`, reportez-vous à l’article sur le [stockage des cartes de site de](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) [Jeff Prosise](http://www.wintellect.com/Weblogs/CategoryView,category,Jeff%20Prosise.aspx) dans SQL Server.

## <a name="step-6-creating-the-custom-site-map-provider"></a>Étape 6 : création du fournisseur de plan de site personnalisé

Pour créer un fournisseur de plan de site personnalisé qui génère le plan de site à partir des catégories et produits de la base de données Northwind, nous devons créer une classe qui étend `StaticSiteMapProvider`. À l’étape 1, j’ai demandé d’ajouter un dossier `CustomProviders` dans le dossier `App_Code` : ajoutez une nouvelle classe à ce dossier nommé `NorthwindSiteMapProvider`. Ajoutez le code suivant à la classe `NorthwindSiteMapProvider` :

[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample6.cs)]

Commençons par explorer cette classe s `BuildSiteMap` méthode, qui commence par une [instruction`lock`](https://msdn.microsoft.com/library/c5kehkcz.aspx). L’instruction `lock` autorise un seul thread à la fois à entrer, ce qui permet de sérialiser l’accès à son code et d’empêcher deux threads simultanés de s’exécuter pas à pas sur un autre orteils.

La variable de `SiteMapNode` au niveau de la classe `root` est utilisée pour mettre en cache la structure du plan de site. Quand le plan de site est construit pour la première fois, ou pour la première fois après la modification des données sous-jacentes, `root` est `null` et la structure de plan de site est construite. Le nœud racine des plans de site est affecté à `root` pendant le processus de construction afin que la prochaine fois que cette méthode est appelée, `root` ne soit pas `null`. Par conséquent, tant que `root` n’est pas `null` la structure du plan de site est retournée à l’appelant sans avoir à le recréer.

Si la racine est `null`, la structure du plan de site est créée à partir des informations sur le produit et la catégorie. Le plan de site est construit en créant les instances `SiteMapNode`, puis en formant la hiérarchie par le biais d’appels à la méthode de la classe `StaticSiteMapProvider` s `AddNode`. `AddNode` effectue la comptabilité interne, en stockant les instances de `SiteMapNode` assorties dans un `Hashtable`. Avant de commencer à construire la hiérarchie, nous commençons par appeler la méthode `Clear`, qui efface les éléments du `Hashtable`interne. Ensuite, la méthode de la classe `ProductsBLL` s `GetProducts` et les `ProductsDataTable` qui en résultent sont stockées dans des variables locales.

La construction des plans de site commence par la création du nœud racine et son affectation à `root`. La surcharge du [constructeur`SiteMapNode` s](https://msdn.microsoft.com/library/system.web.sitemapnode.sitemapnode.aspx) utilisé ici et dans ce `BuildSiteMap` reçoit les informations suivantes :

- Référence au fournisseur de plan de site (`this`).
- `Key``SiteMapNode`. Cette valeur obligatoire doit être unique pour chaque `SiteMapNode`.
- `Url``SiteMapNode`. `Url` est facultatif, mais s’il est fourni, chaque valeur `SiteMapNode` s `Url` doit être unique.
- Le `SiteMapNode` s `Title`, qui est requis.

L’appel de la méthode `AddNode(root)` ajoute la `root` `SiteMapNode` au plan du site en tant que racine. Ensuite, chaque `ProductRow` dans le `ProductsDataTable` est énuméré. S’il existe déjà un `SiteMapNode` pour la catégorie du produit actuel, il est référencé. Dans le cas contraire, une nouvelle `SiteMapNode` pour la catégorie est créée et ajoutée en tant qu’enfant du `SiteMapNode``root` via l’appel de la méthode `AddNode(categoryNode, root)`. Une fois que la catégorie appropriée `SiteMapNode` nœud a été trouvée ou créée, un `SiteMapNode` est créé pour le produit actuel et ajouté en tant qu’enfant de la catégorie `SiteMapNode` via `AddNode(productNode, categoryNode)`. Notez que la valeur de la propriété Category `SiteMapNode` s `Url` est `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID` alors que la propriété Product `SiteMapNode` s `Url` est affectée `~/SiteMapNode/ProductDetails.aspx?ProductID=productID`.

> [!NOTE]
> Les produits qui ont une valeur de `NULL` de base de données pour leur `CategoryID` sont regroupés sous une catégorie `SiteMapNode` dont la propriété `Title` est définie sur None et dont la propriété `Url` est définie sur une chaîne vide. J’ai décidé de définir `Url` sur une chaîne vide dans la mesure où la méthode `ProductBLL` classe `GetProductsByCategory(categoryID)` s ne dispose pas actuellement de la capacité de retourner uniquement les produits avec une valeur `CategoryID` `NULL`. Je voulais également montrer comment les contrôles de navigation affichent une `SiteMapNode` qui n’a pas de valeur pour sa propriété `Url`. Je vous encourage à étendre ce didacticiel pour que la propriété None `SiteMapNode` s `Url` pointe vers `ProductsByCategory.aspx`, mais affiche uniquement les produits avec `NULL` valeurs `CategoryID`.

Après avoir construit le plan de site, un objet arbitraire est ajouté au cache de données à l’aide d’une dépendance de cache SQL sur le `Categories` et `Products` tables via un objet `AggregateCacheDependency`. Nous avons exploré l’utilisation des dépendances de cache SQL dans le didacticiel précédent, *à l’aide des dépendances de cache SQL*. Toutefois, le fournisseur de plan de site personnalisé utilise une surcharge de la méthode `Insert` du cache de données que nous avons encore explorée. Cette surcharge accepte comme paramètre d’entrée final un délégué qui est appelé lorsque l’objet est supprimé du cache. Plus précisément, nous passons un nouveau [délégué`CacheItemRemovedCallback`](https://msdn.microsoft.com/library/system.web.caching.cacheitemremovedcallback.aspx) qui pointe vers la méthode `OnSiteMapChanged` définie plus loin dans la classe `NorthwindSiteMapProvider`.

> [!NOTE]
> La représentation en mémoire du plan de site est mise en cache via la variable au niveau de la classe `root`. Étant donné qu’il n’existe qu’une seule instance de la classe de fournisseur de plan de site personnalisée et que cette instance est partagée entre tous les threads de l’application Web, cette variable de classe sert de cache. La méthode `BuildSiteMap` utilise également le cache de données, mais uniquement en tant que moyen de recevoir une notification lorsque les données de la base de données sous-jacente sont modifiées dans les tables `Categories` ou `Products`. Notez que la valeur placée dans le cache de données est uniquement la date et l’heure actuelles. Les données de plan de site réelles ne sont *pas* placées dans le cache de données.

La méthode `BuildSiteMap` se termine en retournant le nœud racine du plan de site.

Les méthodes restantes sont relativement simples. `GetRootNodeCore` est chargé de retourner le nœud racine. Étant donné que `BuildSiteMap` retourne la racine, `GetRootNodeCore` retourne simplement la valeur de retour de `BuildSiteMap` s. La méthode `OnSiteMapChanged` définit `root` à `null` lorsque l’élément du cache est supprimé. Avec la racine définie sur `null`, la prochaine fois que `BuildSiteMap` est appelé, la structure de plan de site sera reconstruite. Enfin, la propriété `CachedDate` retourne la valeur de date et d’heure stockée dans le cache de données, si une telle valeur existe. Cette propriété peut être utilisée par un développeur de pages pour déterminer à quel moment les données de plan de site ont été mises en cache pour la dernière fois.

## <a name="step-7-registering-thenorthwindsitemapprovider"></a>Étape 7 : inscription du`NorthwindSiteMapProvider`

Pour que notre application Web utilise le fournisseur de plan de site `NorthwindSiteMapProvider` créé à l’étape 6, vous devez l’inscrire dans la section `<siteMap>` de `Web.config`. Plus précisément, ajoutez le balisage suivant dans l’élément `<system.web>` dans `Web.config`:

[!code-xml[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample7.xml)]

Ce balisage effectue deux opérations : tout d’abord, il indique que le `AspNetXmlSiteMapProvider` intégré est le fournisseur de plan de site par défaut ; Deuxièmement, il inscrit le fournisseur de plan de site personnalisé créé à l’étape 6 avec le nom convivial Northwind.

> [!NOTE]
> Pour les fournisseurs de plan de site situés dans le dossier application s `App_Code`, la valeur de l’attribut `type` est simplement le nom de la classe. Le fournisseur de plan de site personnalisé peut également avoir été créé dans un projet de bibliothèque de classes distinct avec l’assembly compilé placé dans le répertoire de l’application Web s `/Bin`. Dans ce cas, la valeur de l’attribut `type` est l' *espace de noms*. *ClassName*, *AssemblyName* .

Après avoir mis à jour `Web.config`, prenez un moment pour afficher les pages des didacticiels dans un navigateur. Notez que l’interface de navigation sur la gauche affiche toujours les sections et les didacticiels définis dans `Web.sitemap`. Cela est dû au fait que nous n’avons pas `AspNetXmlSiteMapProvider` comme fournisseur par défaut. Pour créer un élément d’interface utilisateur de navigation qui utilise la `NorthwindSiteMapProvider`, nous devrons spécifier explicitement que le fournisseur de plan de site Northwind doit être utilisé. Nous allons voir comment procéder à l’étape 8.

## <a name="step-8-displaying-site-map-information-using-the-custom-site-map-provider"></a>Étape 8 : affichage des informations de plan de site à l’aide du fournisseur de plan de site personnalisé

Une fois le fournisseur de plan de site personnalisé créé et inscrit dans `Web.config`, nous sommes prêts à ajouter des contrôles de navigation aux pages `Default.aspx`, `ProductsByCategory.aspx`et `ProductDetails.aspx` dans le dossier `SiteMapProvider`. Commencez par ouvrir la page `Default.aspx` et faites glisser un `SiteMapPath` de la boîte à outils vers le concepteur. Le contrôle SiteMapPath se trouve dans la section navigation de la boîte à outils.

[![ajouter un SiteMapPath à default. aspx](building-a-custom-database-driven-site-map-provider-cs/_static/image19.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image18.gif)

**Figure 16**: ajouter un SiteMapPath à `Default.aspx` ([cliquez pour afficher l’image en taille réelle](building-a-custom-database-driven-site-map-provider-cs/_static/image20.gif))

Le contrôle SiteMapPath affiche un fil d’Ariane indiquant l’emplacement de la page en cours dans le plan du site. Nous avons ajouté un SiteMapPath en haut de la page maître dans le didacticiel *pages maîtres et navigation* dans le site.

Prenez un moment pour afficher cette page par le biais d’un navigateur. Le SiteMapPath ajouté à la figure 16 utilise le fournisseur de plan de site par défaut, en extrayant ses données de `Web.sitemap`. Par conséquent, le chemin d’exploration affiche la page d’hébergement &gt; la personnalisation du plan de site, tout comme le fil d’Ariane dans le coin supérieur droit.

[![le fil d’Ariane utilise le fournisseur de plan de site par défaut](building-a-custom-database-driven-site-map-provider-cs/_static/image22.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image21.gif)

**Figure 17**: le fil d’Ariane utilise le fournisseur de plan de site par défaut ([cliquez pour afficher l’image en taille réelle](building-a-custom-database-driven-site-map-provider-cs/_static/image23.gif))

Pour ajouter le SiteMapPath à la figure 16, utilisez le fournisseur de plan de site personnalisé que nous avons créé à l’étape 6, définissez sa [propriété`SiteMapProvider`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemappath.sitemapprovider.aspx) sur Northwind, le nom que nous avons affecté à la `NorthwindSiteMapProvider` dans `Web.config`. Malheureusement, le concepteur continue d’utiliser le fournisseur de plan de site par défaut, mais si vous accédez à la page via un navigateur après avoir modifié cette propriété, vous verrez que le fil d’Ariane utilise désormais le fournisseur de plan de site personnalisé.

[![le fil d’Ariane utilise désormais le fournisseur de plan de site personnalisé NorthwindSiteMapProvider](building-a-custom-database-driven-site-map-provider-cs/_static/image25.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image24.gif)

**Figure 18**: le fil d’Ariane utilise désormais le fournisseur de plan de Site personnalisé `NorthwindSiteMapProvider` ([cliquez pour afficher l’image en taille réelle](building-a-custom-database-driven-site-map-provider-cs/_static/image26.gif))

Le contrôle SiteMapPath affiche une interface utilisateur plus fonctionnelle dans les pages `ProductsByCategory.aspx` et `ProductDetails.aspx`. Ajoutez un SiteMapPath à ces pages, en affectant la valeur Northwind à la propriété `SiteMapProvider`. Dans `Default.aspx` cliquez sur le lien Afficher les produits pour les boissons, puis sur le lien Afficher les détails pour le thé chai. Comme le montre la figure 19, le fil d’Ariane comprend la section du plan de site actuel (Chai thé) et ses ancêtres : boissons et toutes les catégories.

[![le fil d’Ariane utilise désormais le fournisseur de plan de site personnalisé NorthwindSiteMapProvider](building-a-custom-database-driven-site-map-provider-cs/_static/image27.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image21.png)

**Figure 19**: le fil d’Ariane utilise désormais le fournisseur de plan de Site personnalisé `NorthwindSiteMapProvider` ([cliquez pour afficher l’image en taille réelle](building-a-custom-database-driven-site-map-provider-cs/_static/image22.png))

D’autres éléments de l’interface utilisateur de navigation peuvent être utilisés en plus de l’élément SiteMapPath, tel que les contrôles menu et TreeView. Les pages `Default.aspx`, `ProductsByCategory.aspx`et `ProductDetails.aspx` du téléchargement de ce didacticiel, par exemple, incluent toutes les commandes de menu (voir figure 20). Pour plus d’informations sur les contrôles de navigation et le système de plan de site dans ASP.NET 2,0, consultez [examiner les fonctionnalités de navigation de site ASP.NET 2,0 s](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) et la section [utilisation des contrôles de navigation de site](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/navigation/sitenavcontrols.aspx) des [Démarrages rapides ASP.NET 2,0](https://quickstarts.asp.net/QuickStartv20/aspnet/) .

[![le contrôle Menu répertorie chaque catégorie et chaque produit](building-a-custom-database-driven-site-map-provider-cs/_static/image29.gif)](building-a-custom-database-driven-site-map-provider-cs/_static/image28.gif)

**Figure 20**: le contrôle de menu répertorie chaque catégorie et chaque produit ([cliquez pour afficher l’image en taille réelle](building-a-custom-database-driven-site-map-provider-cs/_static/image30.gif))

Comme mentionné plus haut dans ce didacticiel, la structure de plan de site est accessible par programme par le biais de la classe `SiteMap`. Le code suivant retourne le `SiteMapNode` racine du fournisseur par défaut :

[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample8.cs)]

Étant donné que le `AspNetXmlSiteMapProvider` est le fournisseur par défaut pour notre application, le code ci-dessus retourne le nœud racine défini dans `Web.sitemap`. Pour faire référence à un fournisseur de plan de site autre que celui par défaut, utilisez la propriété `SiteMap` classe s [`Providers`](https://msdn.microsoft.com/library/system.web.sitemap.providers.aspx) comme suit :

[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample9.cs)]

Où *nom* est le nom du fournisseur de plan de site personnalisé (Northwind, pour notre application Web).

Pour accéder à un membre spécifique à un fournisseur de plan de site, utilisez `SiteMap.Providers["name"]` pour récupérer l’instance du fournisseur, puis effectuez un cast de celui-ci vers le type approprié. Par exemple, pour afficher la propriété `NorthwindSiteMapProvider` s `CachedDate` dans une page ASP.NET, utilisez le code suivant :

[!code-csharp[Main](building-a-custom-database-driven-site-map-provider-cs/samples/sample10.cs)]

> [!NOTE]
> Veillez à tester la fonctionnalité de dépendance de cache SQL. Après avoir consulté les pages `Default.aspx`, `ProductsByCategory.aspx`et `ProductDetails.aspx`, accédez à l’un des didacticiels de la section modification, insertion et suppression, puis modifiez le nom d’une catégorie ou d’un produit. Revenez ensuite à l’une des pages du dossier `SiteMapProvider`. En supposant que suffisamment de temps s’est écoulé pour que le mécanisme d’interrogation note la modification apportée à la base de données sous-jacente, le plan de site doit être mis à jour pour afficher le nouveau nom de produit ou de catégorie.

## <a name="summary"></a>Récapitulatif

Les fonctionnalités de plan du site ASP.NET 2,0 s incluent une classe `SiteMap`, un certain nombre de contrôles Web de navigation intégrés et un fournisseur de plan de site par défaut qui attend que les informations de plan de site soient rendues persistantes dans un fichier XML. Pour utiliser les informations de plan de site d’une autre source telle que celle d’une base de données, de l’architecture de l’application ou d’un service Web distant, nous devons créer un fournisseur de plan de site personnalisé. Cela implique la création d’une classe qui dérive, directement ou indirectement, de la classe `SiteMapProvider`.

Dans ce didacticiel, nous avons vu comment créer un fournisseur de plan de site personnalisé basé sur le plan de site sur le produit et les informations de catégorie tirées de l’architecture de l’application. Notre fournisseur a étendu la classe `StaticSiteMapProvider` et a entraîné la création d’une méthode `BuildSiteMap` qui a récupéré les données, construit la hiérarchie du plan du site et mis en cache la structure résultante dans une variable au niveau de la classe. Nous avons utilisé une dépendance de cache SQL avec une fonction de rappel pour invalider la structure mise en cache lors de la modification des données `Categories` ou `Products` sous-jacentes.

Bonne programmation !

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Stockage des plans de site dans SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) et [le fournisseur de plan de site SQL que vous avez attendu](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx)
- [Présentation du modèle de fournisseur ASP.NET 2,0 s](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)
- [Boîte à outils du fournisseur](https://msdn.microsoft.com/asp.net/aa336558.aspx)
- [Examen des fonctionnalités de navigation de site ASP.NET 2,0 s](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel étaient Dave Gardner, Zack Jones, Teresa Murphy et Bernadette Leigh. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Suivant](building-a-custom-database-driven-site-map-provider-vb.md)
