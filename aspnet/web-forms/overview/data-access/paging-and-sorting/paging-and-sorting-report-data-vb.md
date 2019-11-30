---
uid: web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-vb
title: Pagination et tri des données de rapport (VB) | Microsoft Docs
author: rick-anderson
description: La pagination et le tri sont deux fonctionnalités très courantes lors de l’affichage des données dans une application en ligne. Dans ce didacticiel, nous allons commencer par examiner l’ajout du tri et...
ms.author: riande
ms.date: 08/15/2006
ms.assetid: b895e37e-0e69-45cc-a7e4-17ddd2e1b38d
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 6785b5cd2d4d3a2c2e7f2c2fea93f5cd5e2fdf24
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74619075"
---
# <a name="paging-and-sorting-report-data-vb"></a>Pagination et tri des données des rapports (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_24_VB.exe) ou [Télécharger le PDF](paging-and-sorting-report-data-vb/_static/datatutorial24vb1.pdf)

> La pagination et le tri sont deux fonctionnalités très courantes lors de l’affichage des données dans une application en ligne. Dans ce didacticiel, nous allons d’abord ajouter le tri et la pagination à vos rapports, que nous utiliserons ensuite dans les prochains didacticiels.

## <a name="introduction"></a>Introduction

La pagination et le tri sont deux fonctionnalités très courantes lors de l’affichage des données dans une application en ligne. Par exemple, lors de la recherche de livres ASP.NET dans une librairie en ligne, il peut y avoir des centaines de livres de ce type, mais le rapport qui répertorie les résultats de la recherche ne répertorie que dix correspondances par page. En outre, les résultats peuvent être triés par titre, par prix, par nombre de pages, par nom d’auteur, et ainsi de suite. Tandis que les 23 derniers didacticiels ont examiné comment créer un grand nombre de rapports, y compris les interfaces qui permettent d’ajouter, de modifier et de supprimer des données, nous n’avons pas vu comment trier les données, et les seuls exemples de pagination que nous avons vus ont été avec DetailsView et FormView commandes.

Dans ce didacticiel, nous allons voir comment ajouter le tri et la pagination à vos rapports, ce qui peut être accompli en cochant simplement quelques cases. Malheureusement, cette implémentation simpliste présente ses inconvénients. l’interface de tri laisse un peu de choix et les routines de pagination ne sont pas conçues pour une pagination efficace via des jeux de résultats volumineux. Les prochains didacticiels découvriront comment surmonter les limitations des solutions de pagination et de tri prêtes à l’emploi.

## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Étape 1 : ajout des pages Web du didacticiel de pagination et de tri

Avant de commencer ce didacticiel, nous allons tout d’abord prendre un moment pour ajouter les pages ASP.NET dont nous aurons besoin pour ce didacticiel et les trois prochaines. Commencez par créer un nouveau dossier dans le projet nommé `PagingAndSorting`. Ensuite, ajoutez les cinq pages ASP.NET suivantes à ce dossier, en faisant en sorte qu’elles soient toutes configurées pour utiliser la page maître `Site.master`:

- `Default.aspx`
- `SimplePagingSorting.aspx`
- `EfficientPaging.aspx`
- `SortParameter.aspx`
- `CustomSortingUI.aspx`

![Créer un dossier PagingAndSorting et ajouter les pages du didacticiel ASP.NET](paging-and-sorting-report-data-vb/_static/image1.png)

**Figure 1**: créer un dossier PagingAndSorting et ajouter les pages du didacticiel ASP.net

Ensuite, ouvrez la page `Default.aspx`, puis faites glisser le contrôle utilisateur `SectionLevelTutorialListing.ascx` à partir du dossier `UserControls` sur l’aire de conception. Ce contrôle utilisateur, que nous avons créé dans le didacticiel sur les [pages maîtres et la navigation](../introduction/master-pages-and-site-navigation-vb.md) dans les sites, énumère le plan du site et affiche ces didacticiels dans la section actuelle d’une liste à puces.

![Ajouter le contrôle utilisateur SectionLevelTutorialListing. ascx à default. aspx](paging-and-sorting-report-data-vb/_static/image2.png)

**Figure 2**: ajouter le contrôle utilisateur SectionLevelTutorialListing. ascx à default. aspx

Pour que la liste à puces affiche les didacticiels de pagination et de tri que nous allons créer, vous devez les ajouter au plan du site. Ouvrez le fichier `Web.sitemap` et ajoutez le balisage suivant après la modification, l’insertion et la suppression du balisage de nœud de plan de site :

[!code-xml[Main](paging-and-sorting-report-data-vb/samples/sample1.xml)]

![Mettre à jour le plan du site pour inclure les nouvelles pages ASP.NET](paging-and-sorting-report-data-vb/_static/image3.png)

**Figure 3**: mettre à jour le plan du site pour inclure les nouvelles pages ASP.net

## <a name="step-2-displaying-product-information-in-a-gridview"></a>Étape 2 : affichage des informations sur le produit dans un GridView

Avant d’implémenter les fonctionnalités de pagination et de tri, nous allons tout d’abord créer un contrôle GridView non pouvant être trié et non paginable standard qui répertorie les informations sur le produit. Il s’agit d’une tâche que nous avons effectuée beaucoup de fois dans cette série de didacticiels pour que ces étapes soient familières. Commencez par ouvrir la page de `SimplePagingSorting.aspx` et faites glisser un contrôle GridView de la boîte à outils vers le concepteur, en affectant à sa propriété `ID` la valeur `Products`. Ensuite, créez un ObjectDataSource qui utilise la méthode ProductsBLL Class s `GetProducts()` pour retourner toutes les informations sur le produit.

![Récupérer des informations sur tous les produits à l’aide de la méthode GetProducts ()](paging-and-sorting-report-data-vb/_static/image4.png)

**Figure 4**: récupérer des informations sur tous les produits à l’aide de la méthode GetProducts ()

Étant donné que ce rapport est un rapport en lecture seule, il n’est pas nécessaire de mapper les méthodes de `Insert()`, `Update()`ou de `Delete()` ObjectDataSource aux méthodes `ProductsBLL` correspondantes ; par conséquent, choisissez (aucun) dans la liste déroulante des onglets mettre à jour, insérer et supprimer.

![Choisissez l’option (aucune) dans la liste déroulante des onglets mettre à jour, insérer et supprimer](paging-and-sorting-report-data-vb/_static/image5.png)

**Figure 5**: sélectionner l’option (aucune) dans la liste déroulante des onglets mettre à jour, insérer et supprimer

Ensuite, nous allons personnaliser les champs GridView s afin que seuls les noms des produits, les fournisseurs, les catégories, les prix et les États abandonnés s’affichent. En outre, n’hésitez pas à apporter des modifications de mise en forme au niveau des champs, telles que l’ajustement des propriétés de `HeaderText` ou la mise en forme du prix en tant que devise. Une fois ces modifications effectuées, votre balisage déclaratif s doit ressembler à ce qui suit :

[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample2.aspx)]

La figure 6 montre notre progression jusqu’à présent dans un navigateur. Notez que la page répertorie tous les produits dans un seul écran, indiquant chaque nom de produit, catégorie, fournisseur, prix et état abandonné.

[![chacun des produits est répertorié](paging-and-sorting-report-data-vb/_static/image7.png)](paging-and-sorting-report-data-vb/_static/image6.png)

**Figure 6**: chaque produit est répertorié ([cliquez pour afficher l’image en taille réelle](paging-and-sorting-report-data-vb/_static/image8.png))

## <a name="step-3-adding-paging-support"></a>Étape 3 : ajout de la prise en charge de la pagination

La liste de *tous* les produits sur un seul écran peut entraîner une surcharge d’informations pour l’utilisateur qui utilise les données. Pour faciliter la gestion des résultats, nous pouvons diviser les données en pages de données plus petites et permettre à l’utilisateur de parcourir les données une page à la fois. Pour ce faire, cochez la case Activer la pagination de la balise active GridView s (cela définit la [propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowpaging.aspx) gridview s`AllowPaging` sur `true`).

[![activez la case à cocher Activer la pagination pour ajouter la prise en charge de la pagination](paging-and-sorting-report-data-vb/_static/image10.png)](paging-and-sorting-report-data-vb/_static/image9.png)

**Figure 7**: cochez la case Activer la pagination pour ajouter la prise en charge de la pagination ([cliquez pour afficher l’image en taille réelle](paging-and-sorting-report-data-vb/_static/image11.png))

L’activation de la pagination limite le nombre d’enregistrements affichés par page et ajoute une *interface de pagination* au GridView. L’interface de pagination par défaut, illustrée à la figure 7, est une série de numéros de page, ce qui permet à l’utilisateur de naviguer rapidement d’une page de données à une autre. Cette interface de pagination doit vous sembler familière, comme nous l’avons vu lors de l’ajout de la prise en charge de la pagination aux contrôles DetailsView et FormView dans les didacticiels précédents.

Les contrôles DetailsView et FormView n’affichent qu’un seul enregistrement par page. Toutefois, le GridView consulte sa [propriété`PageSize`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.pagesize.aspx) pour déterminer le nombre d’enregistrements à afficher par page (la valeur par défaut de cette propriété est 10).

Cette interface de pagination GridView, DetailsView et FormView peut être personnalisée à l’aide des propriétés suivantes :

- `PagerStyle` indique les informations de style pour l’interface de pagination ; peut spécifier des paramètres comme `BackColor`, `ForeColor`, `CssClass`, `HorizontalAlign`, etc.
- `PagerSettings` contient un multitude de propriétés qui peut personnaliser les fonctionnalités de l’interface de pagination. `PageButtonCount` indique le nombre maximal de numéros de page numériques affichés dans l’interface de pagination (la valeur par défaut est 10); la [propriété`Mode`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pagersettings.mode.aspx) indique le fonctionnement de l’interface de pagination et peut avoir la valeur : 

    - `NextPrevious` affiche les boutons suivant et précédent, ce qui permet à l’utilisateur d’avancer ou de reculer d’une page à la fois.
    - `NextPreviousFirstLast` en plus des boutons suivant et précédent, le premier et le dernier bouton sont également inclus, ce qui permet à l’utilisateur de passer rapidement à la première ou dernière page de données.
    - `Numeric` affiche une série de numéros de page, ce qui permet à l’utilisateur d’accéder immédiatement à n’importe quelle page
    - `NumericFirstLast` en plus des numéros de page, comprend les boutons premier et dernier, ce qui permet à l’utilisateur de passer rapidement à la première ou à la dernière page de données ; les premier et dernier boutons s’affichent uniquement si tous les numéros de page numériques ne sont pas adaptés

En outre, les propriétés GridView, DetailsView et FormView offrent toutes les propriétés `PageIndex` et `PageCount`, qui indiquent respectivement la page actuelle affichée et le nombre total de pages de données. La propriété `PageIndex` est indexée à partir de 0, ce qui signifie que lorsque vous affichez la première page de données, `PageIndex` est égal à 0. `PageCount`, en revanche, commence à compter à 1, ce qui signifie que `PageIndex` est limitée aux valeurs comprises entre 0 et `PageCount - 1`.

Essayons quelques instants pour améliorer l’apparence par défaut de notre interface de pagination de GridView s. En particulier, faites en sorte que l’interface de pagination soit alignée à droite avec un arrière-plan gris clair. Au lieu de définir ces propriétés directement par le biais de la propriété GridView s `PagerStyle`, créez une classe CSS dans `Styles.css` nommée `PagerRowStyle`, puis affectez la propriété `PagerStyle` s `CssClass` par le biais de notre thème. Commencez par ouvrir `Styles.css` et ajoutez la définition de classe CSS suivante :

[!code-css[Main](paging-and-sorting-report-data-vb/samples/sample3.css)]

Ensuite, ouvrez le fichier `GridView.skin` dans le dossier `DataWebControls` dans le dossier `App_Themes`. Comme nous l’avons vu dans le didacticiel sur les *pages maîtres et la navigation* dans les sites, les fichiers d’apparence peuvent être utilisés pour spécifier les valeurs de propriété par défaut d’un contrôle Web. Par conséquent, augmentez les paramètres existants pour inclure la définition de la propriété `PagerStyle` s `CssClass` sur `PagerRowStyle`. En outre, configurez l’interface de pagination pour afficher au maximum cinq boutons de page numériques à l’aide de l’interface de pagination `NumericFirstLast`.

[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample4.aspx)]

## <a name="the-paging-user-experience"></a>L’expérience utilisateur de pagination

La figure 8 illustre la page Web lorsqu’elle est visitée via un navigateur après la vérification de la case à cocher de la pagination de GridView s et que les configurations `PagerStyle` et `PagerSettings` ont été effectuées par le biais du fichier `GridView.skin`. Notez que seuls dix enregistrements sont affichés et que l’interface de pagination indique que nous affichons la première page de données.

[![avec la pagination activée, seul un sous-ensemble des enregistrements s’affiche à la fois.](paging-and-sorting-report-data-vb/_static/image13.png)](paging-and-sorting-report-data-vb/_static/image12.png)

**Figure 8**: lorsque la pagination est activée, seul un sous-ensemble des enregistrements s’affiche à la fois ([cliquez pour afficher l’image en taille réelle](paging-and-sorting-report-data-vb/_static/image14.png))

Lorsque l’utilisateur clique sur l’un des numéros de page dans l’interface de pagination, une publication (postback) est effectuée et les rechargements de page indiquent les enregistrements de la page demandée. La figure 9 montre les résultats après avoir choisi d’afficher la dernière page de données. Notez que la dernière page n’a qu’un seul enregistrement ; Cela est dû au fait qu’il y a 81 enregistrements au total, ce qui aboutit à huit pages de 10 enregistrements par page, plus une page avec un enregistrement solitaire.

[![le fait de cliquer sur un numéro de page provoque une publication et affiche le sous-ensemble approprié d’enregistrements](paging-and-sorting-report-data-vb/_static/image16.png)](paging-and-sorting-report-data-vb/_static/image15.png)

**Figure 9**: un clic sur un numéro de page entraîne une publication (postback) et affiche le sous-ensemble approprié d’enregistrements ([cliquez pour afficher l’image en taille réelle](paging-and-sorting-report-data-vb/_static/image17.png))

## <a name="paging-s-server-side-workflow"></a>Pagination du flux de travail côté serveur

Lorsque l’utilisateur final clique sur un bouton dans l’interface de pagination, une publication (postback) est effectuée et le flux de travail côté serveur suivant commence :

1. L’événement `PageIndexChanging` GridView s (ou DetailsView ou FormView) est déclenché
2. ObjectDataSource redemande *toutes* les données de la couche BLL ; les valeurs de propriété GridView s `PageIndex` et `PageSize` sont utilisées pour déterminer les enregistrements retournés par la couche BLL qui doivent être affichés dans le contrôle GridView.
3. L’événement `PageIndexChanged` GridView s est déclenché

À l’étape 2, ObjectDataSource redemande toutes les données de sa source de données. Ce style de pagination est communément appelé *pagination par défaut*, car il s’agit du comportement de pagination utilisé par défaut lors de la définition de la propriété `AllowPaging` sur `true`. Avec la pagination par défaut, le contrôle Web de données naïvement récupère tous les enregistrements pour chaque page de données, même si seul un sous-ensemble d’enregistrements est effectivement rendu dans le code HTML qui est envoyé au navigateur. À moins que les données de la base de données ne soient mises en cache par la couche BLL ou ObjectDataSource, la pagination par défaut est impossible pour les jeux de résultats suffisamment volumineux ou pour les applications Web avec un grand nombre d’utilisateurs simultanés.

Dans le didacticiel suivant, nous allons examiner comment implémenter la *pagination personnalisée*. Avec la pagination personnalisée, vous pouvez demander spécifiquement à ObjectDataSource de récupérer uniquement l’ensemble précis des enregistrements nécessaires pour la page de données demandée. Comme vous pouvez l’imaginer, la pagination personnalisée améliore considérablement l’efficacité de la pagination par le biais de jeux de résultats volumineux.

> [!NOTE]
> Bien que la pagination par défaut ne soit pas appropriée lors de la pagination de jeux de résultats suffisamment volumineux ou pour des sites avec de nombreux utilisateurs simultanés, sachez que la pagination personnalisée nécessite plus de modifications et d’efforts pour implémenter et n’est pas aussi simple que d’activer une case à cocher (par défaut pagination). Par conséquent, la pagination par défaut peut être le choix idéal pour les petits sites Web à faible trafic ou pour la pagination via des jeux de résultats relativement petits, car elle est beaucoup plus facile et plus rapide à mettre en œuvre.

Par exemple, si nous savons que nous n’aurons jamais plus de 100 produits dans notre base de données, le gain de performances minimal apprécié par la pagination personnalisée est probablement compensé par l’effort nécessaire pour l’implémenter. Toutefois, si nous avons un jour qui a des milliers ou des dizaines de milliers de produits, le *non* -déploiement de la pagination personnalisée risquerait de nuire à l’évolutivité de notre application.

## <a name="step-4-customizing-the-paging-experience"></a>Étape 4 : personnalisation de l’expérience de pagination

Les contrôles Web de données fournissent un certain nombre de propriétés qui peuvent être utilisées pour améliorer l’expérience de pagination des utilisateurs. La propriété `PageCount`, par exemple, indique le nombre total de pages, tandis que la propriété `PageIndex` indique la page actuellement visitée et peut être définie pour déplacer rapidement un utilisateur vers une page spécifique. Pour illustrer comment utiliser ces propriétés pour améliorer l’expérience de pagination des utilisateurs, ajoutez un contrôle Web Label à notre page pour informer l’utilisateur de la page visitée, ainsi qu’un contrôle DropDownList qui lui permet d’accéder rapidement à une page donnée. .

Tout d’abord, ajoutez un contrôle Web Label à votre page, affectez à sa propriété `ID` la valeur `PagingInformation`et effacez sa propriété `Text`. Ensuite, créez un gestionnaire d’événements pour l’événement `DataBound` GridView s et ajoutez le code suivant :

[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample5.vb)]

Ce gestionnaire d’événements affecte la propriété `PagingInformation` étiquette s `Text` à un message informant l’utilisateur que la page sur laquelle il est actuellement visité `Products.PageIndex + 1` le nombre total de pages `Products.PageCount` (nous ajoutons 1 à la propriété `Products.PageIndex`, car `PageIndex` est indexé à partir de 0). J’ai choisi la propriété assigner cette étiquette `Text` dans le gestionnaire d’événements `DataBound` au lieu du gestionnaire d’événements `PageIndexChanged`, car l’événement `DataBound` se déclenche chaque fois que des données sont liées au GridView alors que le gestionnaire d’événements `PageIndexChanged` se déclenche uniquement lorsque l’index de page est modifié. Lorsque le GridView est initialement lié aux données sur la première page, l’événement `PageIndexChanging` ne se déclenche pas (contrairement à l’événement `DataBound`).

Avec cet ajout, l’utilisateur affiche maintenant un message indiquant la page visitée et le nombre total de pages de données qu’il contient.

[![le numéro de page actuel et le nombre total de pages affichées](paging-and-sorting-report-data-vb/_static/image19.png)](paging-and-sorting-report-data-vb/_static/image18.png)

**Figure 10**: le numéro de page actuel et le nombre total de pages s’affichent ([cliquez pour afficher l’image en taille réelle](paging-and-sorting-report-data-vb/_static/image20.png))

En plus du contrôle Label, les autres ajoutent également un contrôle DropDownList qui répertorie les numéros de page dans le GridView avec la page actuellement affichée sélectionnée. L’idée est que l’utilisateur peut rapidement passer de la page actuelle à une autre en sélectionnant simplement le nouvel index de page dans le DropDownList. Commencez par ajouter un contrôle DropDownList au concepteur, en affectant à sa propriété `ID` la valeur `PageList` et en activant l’option Activer AutoPostBack à partir de sa balise active.

Ensuite, revenez au gestionnaire d’événements `DataBound` et ajoutez le code suivant :

[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample6.vb)]

Ce code commence par effacer les éléments dans le `PageList` DropDownList. Cela peut paraître superflu, car il n’est pas prévu que le nombre de pages change, mais d’autres utilisateurs utilisent le système simultanément, en ajoutant ou en supprimant des enregistrements de la table `Products`. De telles insertions ou suppressions pourraient altérer le nombre de pages de données.

Ensuite, nous devons recréer les numéros de page et avoir celui qui est mappé au GridView actuel `PageIndex` sélectionné par défaut. Nous faisons cela avec une boucle comprise entre 0 et `PageCount - 1`, en ajoutant un nouveau `ListItem` dans chaque itération et en définissant sa propriété `Selected` sur true si l’index d’itération actuel est égal à la propriété GridView s `PageIndex`.

Enfin, nous devons créer un gestionnaire d’événements pour l’événement DropDownList s `SelectedIndexChanged`, qui se déclenche chaque fois que l’utilisateur choisit un élément différent dans la liste. Pour créer ce gestionnaire d’événements, il vous suffit de double-cliquer sur le DropDownList dans le concepteur, puis d’ajouter le code suivant :

[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample7.vb)]

Comme le montre la figure 11, le simple fait de modifier la propriété GridView s `PageIndex` entraîne la reliaison des données au contrôle GridView. Dans le gestionnaire d’événements `DataBound` GridView s, la `ListItem` DropDownList appropriée est sélectionnée.

[![l’utilisateur est automatiquement dirigé vers la sixième page lors de la sélection de l’élément de liste déroulante page 6](paging-and-sorting-report-data-vb/_static/image22.png)](paging-and-sorting-report-data-vb/_static/image21.png)

**Figure 11**: l’utilisateur est automatiquement dirigé vers la sixième page lors de la sélection de l’élément de liste déroulante page 6 ([cliquez pour afficher l’image en taille réelle](paging-and-sorting-report-data-vb/_static/image23.png))

## <a name="step-5-adding-bi-directional-sorting-support"></a>Étape 5 : ajout de la prise en charge du tri bidirectionnel

L’ajout de la prise en charge du tri bidirectionnel est aussi simple que l’ajout de la prise en charge de la pagination. activez simplement l’option Activer le tri à partir de la balise active de GridView s (qui affecte à la propriété GridView s [`AllowSorting`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowsorting.aspx) la valeur `true`). Cela génère le rendu de chacun des en-têtes des champs GridView s en tant que LinkButtons qui, lorsque vous cliquez dessus, provoquent une publication (postback) et retournent les données triées par la colonne sur laquelle l’utilisateur a cliqué dans l’ordre croissant. Le fait de cliquer sur le même LinkButton d’en-tête retrie à nouveau les données dans l’ordre décroissant.

> [!NOTE]
> Si vous utilisez une couche d’accès aux données personnalisée plutôt qu’un DataSet typé, vous ne disposez peut-être pas de l’option Activer le tri dans la balise active GridView s. Seuls les GridViews liés à des sources de données qui prennent en charge le tri en mode natif disposent de cette case à cocher disponible. Le DataSet typé fournit une prise en charge de tri prête à l’emploi puisque le DataTable ADO.NET fournit une méthode `Sort` qui, lorsqu’elle est appelée, trie les DataRow s à l’aide des critères spécifiés.

Si votre couche DAL ne retourne pas d’objets qui prennent en charge le tri en mode natif, vous devez configurer ObjectDataSource pour transmettre les informations de tri à la couche de logique métier, qui peut soit trier les données, soit faire en sorte que les données soient triées par la couche DAL. Nous allons découvrir comment trier les données au niveau de la logique métier et des couches d’accès aux données dans un prochain didacticiel.

Les LinkButtonss de tri sont rendus sous forme de liens hypertexte HTML, dont les couleurs actuelles (bleu pour un lien non visité et un lien rouge foncé pour un lien visité) sont en conflit avec la couleur d’arrière-plan de la ligne d’en-tête. À la place, laissez tous les liens d’en-tête de ligne s’afficher en blanc, qu’ils aient été ou non visités. Pour ce faire, vous pouvez ajouter le code suivant à la classe `Styles.css` :

[!code-css[Main](paging-and-sorting-report-data-vb/samples/sample8.css)]

Cette syntaxe indique d’utiliser le texte blanc lors de l’affichage de ces liens hypertexte dans un élément qui utilise la classe HeaderStyle.

Après cet ajout de CSS, lors de la visite de la page via un navigateur, votre écran doit ressembler à la figure 12. En particulier, la figure 12 montre les résultats une fois que vous avez cliqué sur le lien de l’en-tête s des champs de prix.

[![les résultats ont été triés par prix unitaire dans l’ordre croissant](paging-and-sorting-report-data-vb/_static/image25.png)](paging-and-sorting-report-data-vb/_static/image24.png)

**Figure 12**: les résultats ont été triés par prix unitaire dans l’ordre croissant ([cliquez pour afficher l’image en taille réelle](paging-and-sorting-report-data-vb/_static/image26.png))

## <a name="examining-the-sorting-workflow"></a>Examen du flux de travail de tri

Tous les champs GridView, BoundField, CheckBoxField, TemplateField, etc., ont une propriété `SortExpression` qui indique l’expression à utiliser pour trier les données lorsque l’utilisateur clique sur le lien d’en-tête de tri de ce champ. Le GridView possède également une propriété `SortExpression`. Quand l’utilisateur clique sur un contrôle LinkButton d’en-tête de tri, le contrôle GridView affecte à la propriété `SortExpression` la valeur de ce `SortExpression` champ. Ensuite, les données sont récupérées à partir de ObjectDataSource et triées en fonction de la propriété GridView s `SortExpression`. La liste suivante décrit en détail la séquence d’étapes qui s’écoule lorsqu’un utilisateur final trie les données dans un GridView :

1. L’événement de [Tri](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorting(VS.80).aspx) GridView s est déclenché
2. La propriété GridView s [`SortExpression`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) est définie sur le `SortExpression` du champ dont l’en-tête de tri LinkButton a été cliqué
3. ObjectDataSource récupère à nouveau toutes les données de la couche BLL, puis trie les données à l’aide du `SortExpression` GridView s
4. La propriété de la `PageIndex` GridView s est réinitialisée à 0, ce qui signifie que lorsque le tri de l’utilisateur est retourné à la première page de données (en supposant que la prise en charge de la pagination a été implémentée)
5. L' [événement`Sorted`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorted(VS.80).aspx) GridView s est déclenché

Comme avec la pagination par défaut, l’option de tri par défaut récupère *tous* les enregistrements de la couche BLL. Lors de l’utilisation du tri sans pagination ou lors de l’utilisation du tri avec la pagination par défaut, il n’existe aucun moyen de contourner ce gain de performance (à l’instar de la mise en cache des données de base de données) Toutefois, comme nous le verrons dans un prochain didacticiel, il est possible de trier efficacement les données lors de l’utilisation de la pagination personnalisée.

Lors de la liaison d’un ObjectDataSource au contrôle GridView via la liste déroulante de la balise active GridView s, la propriété `SortExpression` de chaque champ GridView est automatiquement assignée au nom du champ de données dans la classe `ProductsRow`. Par exemple, le `ProductName` BoundField s `SortExpression` a la valeur `ProductName`, comme indiqué dans le balisage déclaratif suivant :

[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample9.aspx)]

Un champ peut être configuré de sorte qu’il ne puisse pas être trié en effaçant sa propriété `SortExpression` (en l’assignant à une chaîne vide). Pour illustrer cela, imaginez que nous ne souhaitions pas permettre à nos clients de trier nos produits par prix. La propriété `UnitPrice` BoundField s `SortExpression` peut être supprimée soit du balisage déclaratif, soit de la boîte de dialogue champs (accessible en cliquant sur le lien modifier les colonnes dans la balise active de GridView s).

![Les résultats ont été triés par prix unitaire dans l’ordre croissant](paging-and-sorting-report-data-vb/_static/image27.png)

**Figure 13**: les résultats ont été triés par prix unitaire dans l’ordre croissant

Une fois que la propriété `SortExpression` a été supprimée pour le `UnitPrice` BoundField, l’en-tête est restitué sous forme de texte et non en tant que lien, ce qui empêche les utilisateurs de trier les données par prix.

[![en supprimant la propriété SortExpression, les utilisateurs ne peuvent plus trier les produits par prix](paging-and-sorting-report-data-vb/_static/image29.png)](paging-and-sorting-report-data-vb/_static/image28.png)

**Figure 14**: en supprimant la propriété SortExpression, les utilisateurs ne peuvent plus trier les produits par prix ([cliquez pour afficher l’image en taille réelle](paging-and-sorting-report-data-vb/_static/image30.png))

## <a name="programmatically-sorting-the-gridview"></a>Tri par programmation du contrôle GridView

Vous pouvez également trier par programmation le contenu de GridView à l’aide de la [méthode`Sort`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sort.aspx)GridView s. Transmettez simplement la valeur `SortExpression` pour effectuer un tri en même temps que la [`SortDirection`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sortdirection.aspx) (`Ascending` ou `Descending`), et les données GridView s seront retriées.

Imaginez que la raison pour laquelle nous avons désactivé le tri par le `UnitPrice` était parce que nous avons peur que nos clients achètent simplement les produits les plus chers. Toutefois, nous souhaitons les inciter à acheter les produits les plus chers. nous aimerions donc pouvoir trier les produits par prix, mais uniquement du prix le plus onéreux au moins.

Pour ce faire, ajoutez un contrôle Web Button à la page, affectez à sa propriété `ID` la valeur `SortPriceDescending`et à sa propriété `Text` la valeur Trier par Price. Ensuite, créez un gestionnaire d’événements pour le bouton s `Click` événement en double-cliquant sur le contrôle bouton dans le concepteur. Ajoutez le code suivant à ce gestionnaire d’événements :

[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample10.vb)]

En cliquant sur ce bouton, vous redonnez à l’utilisateur la première page contenant les produits triés par prix, de la plus coûteuse à la moins coûteuse (voir figure 15).

[![cliquant sur le bouton classe les produits du plus onéreux au moins](paging-and-sorting-report-data-vb/_static/image32.png)](paging-and-sorting-report-data-vb/_static/image31.png)

**Figure 15**: cliquer sur le bouton classe les produits du plus onéreux au moins ([cliquez pour afficher l’image en plein écran](paging-and-sorting-report-data-vb/_static/image33.png))

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, nous avons vu comment implémenter les fonctionnalités de pagination et de tri par défaut, qui étaient aussi simples que de vérifier une case à cocher. Lorsqu’un utilisateur effectue un tri ou des pages de données, un flux de travail similaire est déplié :

1. Une publication (postback) résulte
2. L’événement de préniveau des contrôles Web de données est déclenché (`PageIndexChanging` ou `Sorting`)
3. Toutes les données sont récupérées à nouveau par ObjectDataSource
4. L’événement de publication des contrôles Web de données est déclenché (`PageIndexChanged` ou `Sorted`)

Bien que l’implémentation de la pagination et du tri de base soit un peu, l’effort nécessaire pour utiliser la pagination personnalisée plus efficace ou pour améliorer l’interface de pagination ou de tri est plus poussé. Les prochains didacticiels exploreront ces rubriques.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Précédent](creating-a-customized-sorting-user-interface-cs.md)
> [Suivant](efficiently-paging-through-large-amounts-of-data-vb.md)
