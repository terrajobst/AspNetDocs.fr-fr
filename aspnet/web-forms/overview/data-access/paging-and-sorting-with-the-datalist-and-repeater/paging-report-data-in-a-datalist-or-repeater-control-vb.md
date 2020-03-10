---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-vb
title: Pagination des données de rapport dans un contrôle DataList ou Repeater (VB) | Microsoft Docs
author: rick-anderson
description: Bien que le contrôle DataList et le Repeater n’offrent pas la prise en charge de la pagination ou du tri automatique, ce didacticiel montre comment ajouter la prise en charge de la pagination au contrôle DataList ou Repeater,...
ms.author: riande
ms.date: 11/13/2006
ms.assetid: bbd6b7f7-b98a-48b4-93f3-341d6a4f53c0
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 5c65ca1f263e41748d99323dbdf1c28fdd077246
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78620441"
---
# <a name="paging-report-data-in-a-datalist-or-repeater-control-vb"></a>Pagination des données d’un rapport dans un contrôle DataList ou Repeater (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_44_VB.exe) ou [Télécharger le PDF](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/datatutorial44vb1.pdf)

> Bien que le contrôle DataList et le Repeater n’offrent pas la prise en charge de la pagination ou du tri automatique, ce didacticiel montre comment ajouter la prise en charge de la pagination au contrôle DataList ou Repeater, ce qui permet une pagination et des interfaces d’affichage des données bien plus flexibles.

## <a name="introduction"></a>Introduction

La pagination et le tri sont deux fonctionnalités très courantes lors de l’affichage des données dans une application en ligne. Par exemple, lors de la recherche de livres ASP.NET dans une librairie en ligne, il peut y avoir des centaines de livres de ce type, mais le rapport qui répertorie les résultats de la recherche ne répertorie que dix correspondances par page. En outre, les résultats peuvent être triés par titre, par prix, par nombre de pages, par nom d’auteur, et ainsi de suite. Comme nous l’avons vu dans le didacticiel sur les [données de rapport de pagination et de tri](../paging-and-sorting/paging-and-sorting-report-data-vb.md) , les contrôles GridView, DetailsView et FormView fournissent tous une prise en charge intégrée de la pagination qui peut être activée au niveau d’une case à cocher. Le GridView prend également en charge le tri.

Malheureusement, ni le contrôle DataList ni le Repeater n’offrent la prise en charge de la pagination ou du tri automatique. Dans ce didacticiel, nous allons examiner comment ajouter la prise en charge de la pagination au contrôle DataList ou Repeater. Nous devons créer l’interface de pagination manuellement, afficher la page d’enregistrements appropriée et mémoriser la page visitée entre les publications. Bien que cela prenne plus de temps et de code qu’avec GridView, DetailsView ou FormView, le contrôle DataList et Repeater autorisent des interfaces de pagination et d’affichage de données bien plus flexibles.

> [!NOTE]
> Ce didacticiel se concentre exclusivement sur la pagination. Dans le didacticiel suivant, nous allons attirer l’attention sur l’ajout de fonctionnalités de tri.

## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Étape 1 : ajout des pages Web du didacticiel de pagination et de tri

Avant de commencer ce didacticiel, nous allons tout d’abord prendre un moment pour ajouter les pages ASP.NET dont nous aurons besoin pour ce didacticiel et pour le suivant. Commencez par créer un nouveau dossier dans le projet nommé `PagingSortingDataListRepeater`. Ensuite, ajoutez les cinq pages ASP.NET suivantes à ce dossier, en faisant en sorte qu’elles soient toutes configurées pour utiliser la page maître `Site.master`:

- `Default.aspx`
- `Paging.aspx`
- `Sorting.aspx`
- `SortingWithDefaultPaging.aspx`
- `SortingWithCustomPaging.aspx`

![Créer un dossier PagingSortingDataListRepeater et ajouter les pages du didacticiel ASP.NET](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image1.png)

**Figure 1**: créer un dossier `PagingSortingDataListRepeater` et ajouter les pages du didacticiel ASP.net

Ensuite, ouvrez la page `Default.aspx`, puis faites glisser le contrôle utilisateur `SectionLevelTutorialListing.ascx` à partir du dossier `UserControls` sur l’aire de conception. Ce contrôle utilisateur, que nous avons créé dans le didacticiel sur les [pages maîtres et la navigation](../introduction/master-pages-and-site-navigation-vb.md) dans les sites, énumère le plan du site et affiche ces didacticiels dans la section actuelle d’une liste à puces.

[![ajouter le contrôle utilisateur SectionLevelTutorialListing. ascx à default. aspx](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image3.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image2.png)

**Figure 2**: ajouter le contrôle utilisateur `SectionLevelTutorialListing.ascx` à `Default.aspx` ([cliquez pour afficher l’image en taille réelle](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image4.png))

Pour que la liste à puces affiche les didacticiels de pagination et de tri que nous allons créer, vous devez les ajouter au plan du site. Ouvrez le fichier `Web.sitemap` et ajoutez le balisage suivant après la modification et la suppression avec la balise de nœud de plan de site DataList :

[!code-xml[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample1.xml)]

![Mettre à jour le plan du site pour inclure les nouvelles pages ASP.NET](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image5.png)

**Figure 3**: mettre à jour le plan du site pour inclure les nouvelles pages ASP.net

## <a name="a-review-of-paging"></a>Révision de la pagination

Dans les didacticiels précédents, nous avons vu comment parcourir les données dans les contrôles GridView, DetailsView et FormView. Ces trois contrôles offrent une forme simple de pagination appelée *pagination par défaut* qui peut être implémentée en activant simplement l’option Activer la pagination dans la balise active des contrôles. Avec la pagination par défaut, chaque fois qu’une page de données est demandée lors de la première visite de la page ou lorsque l’utilisateur accède à une page de données différente, le contrôle GridView, DetailsView ou FormView demande à nouveau *toutes* les données de l’ObjectDataSource. Il extrait ensuite l’ensemble d’enregistrements particulier à afficher en fonction de l’index de la page demandée et du nombre d’enregistrements à afficher par page. Nous avons abordé la pagination par défaut en détail dans le didacticiel [pagination et tri des données du rapport](../paging-and-sorting/paging-and-sorting-report-data-vb.md) .

Étant donné que la pagination par défaut redemande tous les enregistrements pour chaque page, il n’est pas pratique de paginer des quantités de données suffisamment volumineuses. Par exemple, imaginez la pagination des enregistrements 50 000 avec une taille de page de 10. Chaque fois que l’utilisateur passe à une nouvelle page, tous les enregistrements 50 000 doivent être récupérés de la base de données, même si seulement dix d’entre eux sont affichés.

La *pagination personnalisée* résout les problèmes de performances de la pagination par défaut en saisissant uniquement le sous-ensemble précis d’enregistrements à afficher sur la page demandée. Lors de l’implémentation de la pagination personnalisée, nous devons écrire la requête SQL qui renverra efficacement le bon ensemble d’enregistrements. Nous avons vu comment créer une requête de ce type à l’aide de SQL Server 2005 s nouveau [`ROW_NUMBER()` mot clé](http://www.4guysfromrolla.com/webtech/010406-1.shtml) dans le didacticiel de [pagination efficace de grandes quantités de données](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) .

Pour implémenter la pagination par défaut dans les contrôles DataList ou Repeater, nous pouvons utiliser la [classe`PagedDataSource`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.aspx) comme un wrapper autour du `ProductsDataTable` dont le contenu est paginé. La classe `PagedDataSource` a une propriété `DataSource` qui peut être assignée à n’importe quel objet énumérable et [`PageSize`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.pagesize.aspx) et [`CurrentPageIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.currentpageindex.aspx) propriétés qui indiquent le nombre d’enregistrements à afficher par page et l’index de page actuel. Une fois ces propriétés définies, le `PagedDataSource` peut être utilisé comme source de données d’un contrôle Web de données. La `PagedDataSource`, lorsqu’elle est énumérée, retourne uniquement le sous-ensemble approprié d’enregistrements de son `DataSource` interne en fonction des propriétés `PageSize` et `CurrentPageIndex`. La figure 4 illustre les fonctionnalités de la classe `PagedDataSource`.

![Le PagedDataSource encapsule un objet énumérable avec une interface paginable](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image6.png)

**Figure 4**: le `PagedDataSource` encapsule un objet énumérable avec une interface paginable

L’objet `PagedDataSource` peut être créé et configuré directement à partir de la couche de logique métier et lié à un contrôle DataList ou Repeater via un ObjectDataSource, ou peut être créé et configuré directement dans la classe code-behind de la page ASP.NET. Si cette dernière approche est utilisée, nous devons renoncer à utiliser ObjectDataSource et lier les données paginées au contrôle DataList ou Repeater par programmation.

L’objet `PagedDataSource` possède également des propriétés pour prendre en charge la pagination personnalisée. Toutefois, nous pouvons contourner l’utilisation d’une `PagedDataSource` pour la pagination personnalisée, car nous avons déjà des méthodes BLL dans la classe `ProductsBLL` conçue pour la pagination personnalisée qui retourne les enregistrements précis à afficher.

Dans ce didacticiel, nous allons étudier l’implémentation de la pagination par défaut dans un contrôle DataList en ajoutant une nouvelle méthode à la classe `ProductsBLL` qui retourne un objet `PagedDataSource` correctement configuré. Dans le didacticiel suivant, nous verrons comment utiliser la pagination personnalisée.

## <a name="step-2-adding-a-default-paging-method-in-the-business-logic-layer"></a>Étape 2 : ajout d’une méthode de pagination par défaut dans la couche de logique métier

La classe `ProductsBLL` a actuellement une méthode permettant de retourner toutes les informations sur le produit `GetProducts()` et une autre pour retourner un sous-ensemble particulier de produits à un index de départ `GetProductsPaged(startRowIndex, maximumRows)`. Avec la pagination par défaut, les contrôles GridView, DetailsView et FormView utilisent tous la méthode `GetProducts()` pour récupérer tous les produits, mais utilisent un `PagedDataSource` en interne pour afficher uniquement le sous-ensemble correct d’enregistrements. Pour répliquer cette fonctionnalité avec les contrôles DataList et Repeater, nous pouvons créer une nouvelle méthode dans la couche BLL qui imite ce comportement.

Ajoutez une méthode à la classe `ProductsBLL` nommée `GetProductsAsPagedDataSource` qui accepte deux paramètres d’entrée d’entier :

- `pageIndex` l’index de la page à afficher, indexé à zéro, et
- `pageSize` le nombre d’enregistrements à afficher par page.

`GetProductsAsPagedDataSource` commence par récupérer *tous* les enregistrements de `GetProducts()`. Il crée ensuite un objet `PagedDataSource`, en affectant aux propriétés `CurrentPageIndex` et `PageSize` les valeurs des paramètres `pageIndex` et `pageSize` transmis. La méthode conclut en retournant cette `PagedDataSource`configurée :

[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample2.vb)]

## <a name="step-3-displaying-product-information-in-a-datalist-using-default-paging"></a>Étape 3 : affichage des informations sur le produit dans un contrôle DataList à l’aide de la pagination par défaut

Une fois la méthode `GetProductsAsPagedDataSource` ajoutée à la classe `ProductsBLL`, nous pouvons maintenant créer un contrôle DataList ou Repeater qui fournit la pagination par défaut. Commencez par ouvrir la page `Paging.aspx` dans le dossier `PagingSortingDataListRepeater` et faites glisser un contrôle DataList de la boîte à outils vers le concepteur, en affectant à la propriété DataList s `ID` la valeur `ProductsDefaultPaging`. À partir de la balise active de DataList s, créez un nouvel ObjectDataSource nommé `ProductsDefaultPagingDataSource` et configurez-le afin qu’il récupère les données à l’aide de la méthode `GetProductsAsPagedDataSource`.

[![créer un ObjectDataSource et le configurer pour utiliser la méthode GetProductsAsPagedDataSource ()](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image8.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image7.png)

**Figure 5**: créer un ObjectDataSource et le configurer pour qu’il utilise la méthode `GetProductsAsPagedDataSource` `()` ([cliquez pour afficher l’image en taille réelle](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image9.png))

Définissez les listes déroulantes dans les onglets mettre à jour, insérer et supprimer sur (aucun).

[![définir les listes déroulantes dans les onglets mettre à jour, insérer et supprimer sur (aucun)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image11.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image10.png)

**Figure 6**: définir les listes déroulantes des onglets mettre à jour, insérer et supprimer sur (aucun) ([cliquez pour afficher l’image en taille réelle](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image12.png))

Étant donné que la méthode `GetProductsAsPagedDataSource` attend deux paramètres d’entrée, l’Assistant nous invite à entrer la source de ces valeurs de paramètre.

Les valeurs d’index de page et de taille de page doivent être mémorisées entre les publications. Elles peuvent être stockées dans l’état d’affichage, rendues persistantes dans la chaîne de chaîne, stockées dans des variables de session ou mémorisées à l’aide d’une autre technique. Pour ce didacticiel, nous allons utiliser QueryString, qui présente l’avantage de permettre à une page de données particulière d’être associée à un signet.

En particulier, utilisez les champs QueryString pageIndex et PageSize pour les paramètres `pageIndex` et `pageSize`, respectivement (voir la figure 7). Prenez un moment pour définir les valeurs par défaut de ces paramètres, car les valeurs QueryString ne seront pas présentes quand un utilisateur consulte cette page pour la première fois. Pour `pageIndex`, définissez la valeur par défaut sur 0 (qui affiche la première page de données) et `pageSize` valeur par défaut de 4.

[![utiliser QueryString comme source pour les paramètres pageIndex et PageSize](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image14.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image13.png)

**Figure 7**: utiliser QueryString comme source pour les paramètres `pageIndex` et `pageSize` ([cliquez pour afficher l’image en taille réelle](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image15.png))

Après la configuration de l’ObjectDataSource, Visual Studio crée automatiquement un `ItemTemplate` pour la DataList. Personnalisez le `ItemTemplate` afin que seuls le nom, la catégorie et le fournisseur du produit soient affichés. Affectez également la valeur 2 à la propriété DataList s `RepeatColumns`, son `Width` à 100% et ses `ItemStyle` `Width` à 50%. Ces paramètres de largeur fournissent un espacement égal pour les deux colonnes.

Après avoir apporté ces modifications, le balisage de DataList et ObjectDataSource doit ressembler à ce qui suit :

[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample3.aspx)]

> [!NOTE]
> Étant donné que nous n’effectuons pas de fonctionnalité de mise à jour ou de suppression dans ce didacticiel, vous pouvez désactiver l’état d’affichage de DataList pour réduire la taille de la page rendue.

Lorsque vous accédez initialement à cette page par le biais d’un navigateur, ni les paramètres `pageIndex` ni `pageSize` QueryString ne sont fournis. Par conséquent, les valeurs par défaut 0 et 4 sont utilisées. Comme le montre la figure 8, il en résulte un contrôle DataList qui affiche les quatre premiers produits.

[![les quatre premiers produits sont répertoriés](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image17.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image16.png)

**Figure 8**: les quatre premiers produits sont répertoriés ([cliquez pour afficher l’image en taille réelle](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image18.png))

Sans interface de pagination, il n’existe actuellement aucun moyen simple pour un utilisateur de naviguer jusqu’à la deuxième page de données. Nous allons créer une interface de pagination à l’étape 4. Pour le moment, cependant, la pagination ne peut être accomplie qu’en spécifiant directement les critères de pagination dans la chaîne de chaîne. Par exemple, pour afficher la deuxième page, modifiez l’URL dans la barre d’adresse du navigateur de `Paging.aspx` à `Paging.aspx?pageIndex=2` et appuyez sur entrée. Cela entraîne l’affichage de la deuxième page de données (voir la figure 9).

[![la deuxième page de données s’affiche.](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image20.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image19.png)

**Figure 9**: la deuxième page de données s’affiche ([cliquez pour afficher l’image en taille réelle](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image21.png))

## <a name="step-4-creating-the-paging-interface"></a>Étape 4 : création de l’interface de pagination

Il existe une variété d’interfaces de pagination différentes qui peuvent être implémentées. Les contrôles GridView, DetailsView et FormView fournissent quatre interfaces différentes :

- **Ensuite,** les utilisateurs précédents peuvent déplacer une page à la fois vers le suivant ou le précédent.
- **Suivant, précédent, premier,** en plus des boutons suivant et précédent, cette interface comprend les boutons premier et dernier pour passer à la toute première ou dernière page.
- **Numeric** répertorie les numéros de page dans l’interface de pagination, ce qui permet à un utilisateur d’accéder rapidement à une page particulière.
- **Numérique, tout d’abord,** en plus des numéros de page numériques, contient des boutons pour passer à la toute première ou dernière page.

Pour les contrôles DataList et Repeater, nous sommes responsables de la détermination d’une interface de pagination et de son implémentation. Cela implique de créer les contrôles Web nécessaires dans la page et d’afficher la page demandée lorsqu’un clic est effectué sur un bouton de l’interface de pagination particulier. En outre, certains contrôles d’interface de pagination devront peut-être être désactivés. Par exemple, lors de l’affichage de la première page de données à l’aide de l’interface suivante, précédente, première et dernière, le premier et le précédent bouton sont désactivés.

Pour ce didacticiel, utilisez l’interface suivante, précédente, la première, la dernière. Ajoutez quatre contrôles Web Button à la page et définissez leurs `ID` s sur `FirstPage`, `PrevPage`, `NextPage`et `LastPage`. Définissez les propriétés de `Text` sur &lt;&lt; premier, &lt; précédent, suivant &gt;et &gt;&gt;.

[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample4.aspx)]

Ensuite, créez un gestionnaire d’événements `Click` pour chacun de ces boutons. Dans un moment, nous allons ajouter le code nécessaire pour afficher la page demandée.

## <a name="remembering-the-total-number-of-records-being-paged-through"></a>Mémorisation du nombre total d’enregistrements paginés

Quelle que soit l’interface de pagination sélectionnée, nous devons calculer et mémoriser le nombre total d’enregistrements paginés. Le nombre total de lignes (conjointement avec la taille de la page) détermine le nombre total de pages de données paginées, qui détermine les contrôles d’interface de pagination ajoutés ou activés. Dans l’interface suivante, précédente, première, dernière, que nous créons, le nombre de pages est utilisé de deux manières :

- Pour déterminer si la dernière page est affichée, dans ce cas, les boutons suivant et précédent sont désactivés.
- Si l’utilisateur clique sur le dernier bouton, nous devons le faire décéder à la dernière page, dont l’index est inférieur au nombre de pages.

Le nombre de pages est calculé comme le plafond du nombre total de lignes divisé par la taille de la page. Par exemple, si nous paginons les enregistrements 79 avec quatre enregistrements par page, le nombre de pages est de 20 (le plafond de 79/4). Si vous utilisez l’interface de pagination numérique, ces informations indiquent le nombre de boutons de page numériques à afficher. Si notre interface de pagination comprend des boutons suivant ou précédent, le nombre de pages est utilisé pour déterminer quand désactiver les boutons suivant ou précédent.

Si l’interface de pagination comprend un dernier bouton, il est impératif que le nombre total d’enregistrements paginés dans la page soit mémorisé sur les publications, de sorte que lorsque vous cliquez sur le dernier bouton, nous pouvons déterminer le dernier index de page. Pour faciliter cette tâche, créez une `TotalRowCount` propriété dans la classe code-behind de la page ASP.NET, qui conserve sa valeur dans l’état d’affichage :

[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample5.vb)]

En plus de `TotalRowCount`, prenez une minute pour créer des propriétés de niveau page en lecture seule pour accéder facilement à l’index de page, à la taille de page et au nombre de pages :

[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample6.vb)]

## <a name="determining-the-total-number-of-records-being-paged-through"></a>Détermination du nombre total d’enregistrements paginés

L’objet `PagedDataSource` renvoyé par la méthode ObjectDataSource s `Select()` contient *tous* les enregistrements de produit, même si un seul sous-ensemble est affiché dans le contrôle DataList. La propriété `PagedDataSource` s [`Count`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.count.aspx) retourne uniquement le nombre d’éléments qui seront affichés dans le contrôle DataList. la [propriété`DataSourceCount`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.datasourcecount.aspx) retourne le nombre total d’éléments dans le `PagedDataSource`. Par conséquent, nous devons affecter à la propriété ASP.NET des `TotalRowCount` pages de la valeur de la propriété `PagedDataSource` s `DataSourceCount`.

Pour ce faire, créez un gestionnaire d’événements pour l’événement ObjectDataSource s `Selected`. Dans le gestionnaire d’événements `Selected`, nous avons accès à la valeur de retour de la méthode ObjectDataSource s `Select()` dans ce cas, le `PagedDataSource`.

[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample7.vb)]

## <a name="displaying-the-requested-page-of-data"></a>Affichage de la page de données demandée

Quand l’utilisateur clique sur l’un des boutons de l’interface de pagination, nous devons afficher la page de données demandée. Étant donné que les paramètres de pagination sont spécifiés via QueryString, pour afficher la page de données demandée, utilisez `Response.Redirect(url)` pour que le navigateur de l’utilisateur redemande la page `Paging.aspx` avec les paramètres de pagination appropriés. Par exemple, pour afficher la deuxième page de données, nous allons rediriger l’utilisateur vers `Paging.aspx?pageIndex=1`.

Pour faciliter cette tâche, créez une méthode de `RedirectUser(sendUserToPageIndex)` qui redirige l’utilisateur vers `Paging.aspx?pageIndex=sendUserToPageIndex`. Ensuite, appelez cette méthode à partir des quatre boutons `Click` gestionnaires d’événements. Dans le gestionnaire d’événements `FirstPage` `Click`, appelez `RedirectUser(0)`pour les envoyer à la première page. dans le gestionnaire d’événements `PrevPage` `Click`, utilisez `PageIndex - 1` comme index de page ; et ainsi de suite.

[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample8.vb)]

Une fois les gestionnaires d’événements `Click` terminés, les enregistrements de DataList peuvent être paginés en cliquant sur les boutons. Prenez un moment pour essayer !

## <a name="disabling-paging-interface-controls"></a>Désactivation des contrôles d’interface de pagination

Actuellement, les quatre boutons sont activés quelle que soit la page affichée. Toutefois, nous voulons désactiver le premier bouton et les boutons précédents lors de l’indication de la première page de données, ainsi que les boutons suivant et précédent lors de l’illustration de la dernière page. L’objet `PagedDataSource` renvoyé par la méthode ObjectDataSource s `Select()` possède des propriétés [`IsFirstPage`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.isfirstpage.aspx) et [`IsLastPage`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.islastpage.aspx) que nous pouvons examiner pour déterminer si nous affichons la première ou la dernière page de données.

Ajoutez le code suivant au gestionnaire d’événements `Selected` ObjectDataSource s :

[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample9.vb)]

Avec cet ajout, les premier et dernier boutons sont désactivés lors de l’affichage de la première page, tandis que les boutons suivant et précédent sont désactivés lors de l’affichage de la dernière page.

Procédez à l’exécution de l’interface de pagination en informant l’utilisateur de la page qu’il consulte actuellement et du nombre total de pages disponibles. Ajoutez un contrôle Web Label à la page et affectez à sa propriété `ID` la valeur `CurrentPageNumber`. Définissez sa propriété `Text` dans le gestionnaire d’événements sélectionné ObjectDataSource, de telle sorte qu’elle comprenne la page en cours d’affichage (`PageIndex + 1`) et le nombre total de pages (`PageCount`).

[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample10.vb)]

La figure 10 montre `Paging.aspx` lors de la première visite. Étant donné que la chaîne de chaîne est vide, la DataList a par défaut affiché les quatre premiers produits. les premier et dernier boutons sont désactivés. Cliquez sur suivant pour afficher les quatre enregistrements suivants (voir figure 11). les premier et dernier boutons sont désormais activés.

[![la première page de données est affichée.](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image23.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image22.png)

**Figure 10**: la première page de données s’affiche ([cliquez pour afficher l’image en taille réelle](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image24.png))

[![la deuxième page de données s’affiche.](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image26.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image25.png)

**Figure 11**: la deuxième page de données s’affiche ([cliquez pour afficher l’image en taille réelle](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image27.png))

> [!NOTE]
> L’interface de pagination peut encore être améliorée en permettant à l’utilisateur de spécifier le nombre de pages à afficher par page. Par exemple, un DropDownList peut être ajouté à la liste des options de taille de page comme 5, 10, 25, 50 et tout. Lors de la sélection d’une taille de page, l’utilisateur doit être redirigé vers `Paging.aspx?pageIndex=0&pageSize=selectedPageSize`. Je laisse mettre en œuvre cette amélioration en guise d’exercice pour le lecteur.

## <a name="using-custom-paging"></a>Utilisation de la pagination personnalisée

La DataList pagine les données à l’aide de la technique de pagination par défaut inefficace. Lors de la pagination de quantités de données suffisamment volumineuses, il est impératif d’utiliser la pagination personnalisée. Bien que les détails de l’implémentation diffèrent légèrement, les concepts qui sous-tendent l’implémentation d’une pagination personnalisée dans un contrôle DataList sont les mêmes que dans la pagination par défaut. Avec la pagination personnalisée, utilisez la méthode de la classe `ProductBLL` s `GetProductsPaged` (au lieu de `GetProductsAsPagedDataSource`). Comme indiqué dans le didacticiel [pagination efficace dans de grandes quantités de données](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) , `GetProductsPaged` devez recevoir l’index de la ligne de début et le nombre maximal de lignes à retourner. Ces paramètres peuvent être gérés à l’aide de QueryString comme les paramètres `pageIndex` et `pageSize` utilisés dans la pagination par défaut.

Étant donné qu’il n’y a aucune `PagedDataSource` avec la pagination personnalisée, d’autres techniques doivent être utilisées pour déterminer le nombre total d’enregistrements paginés et si nous affichons la première ou la dernière page de données. La méthode `TotalNumberOfProducts()` de la classe `ProductsBLL` retourne le nombre total de produits paginés. Pour déterminer si la première page de données est affichée, examinez l’index de début de ligne s’il est égal à zéro, puis la première page est affichée. La dernière page est affichée si l’index de la ligne de début plus le nombre maximal de lignes à retourner est supérieur ou égal au nombre total d’enregistrements paginés.

Nous allons étudier l’implémentation de la pagination personnalisée plus en détail dans le didacticiel suivant.

## <a name="summary"></a>Récapitulatif

Bien que ni le contrôle DataList ni le Repeater n’offrent la prise en charge de la pagination prête à l’emploi dans les contrôles GridView, DetailsView et FormView, de telles fonctionnalités peuvent être ajoutées avec un minimum d’effort. Le moyen le plus simple d’implémenter la pagination par défaut consiste à encapsuler l’ensemble des produits dans un `PagedDataSource`, puis à lier le `PagedDataSource` au contrôle DataList ou Repeater. Dans ce didacticiel, nous avons ajouté la méthode `GetProductsAsPagedDataSource` à la classe `ProductsBLL` pour retourner le `PagedDataSource`. La classe `ProductsBLL` contient déjà les méthodes nécessaires pour la pagination personnalisée `GetProductsPaged` et `TotalNumberOfProducts`.

En plus de récupérer l’ensemble précis d’enregistrements à afficher pour la pagination personnalisée ou tous les enregistrements d’une `PagedDataSource` pour la pagination par défaut, nous devons également ajouter manuellement l’interface de pagination. Pour ce didacticiel, nous avons créé une interface suivante, précédente, première et dernière avec quatre contrôles Web Button. En outre, un contrôle Label affichant le numéro de page actuel et le nombre total de pages a été ajouté.

Dans le didacticiel suivant, nous verrons comment ajouter la prise en charge du tri au contrôle DataList et au Repeater. Nous verrons également comment créer un contrôle DataList qui peut être paginé et trié (avec des exemples utilisant la pagination par défaut et personnalisée).

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel étaient Liz Shulok, Ken Pespisa et Bernadette Leigh. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](sorting-data-in-a-datalist-or-repeater-control-cs.md)
> [Suivant](sorting-data-in-a-datalist-or-repeater-control-vb.md)
