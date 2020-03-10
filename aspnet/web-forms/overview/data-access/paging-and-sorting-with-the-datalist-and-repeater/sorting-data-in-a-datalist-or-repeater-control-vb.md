---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-vb
title: Tri des données dans un contrôle DataList ou Repeater (VB) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons examiner comment inclure la prise en charge du tri dans le contrôle DataList et Repeater, ainsi que comment construire un contrôle DataList ou Repeater dont les données peuvent...
ms.author: riande
ms.date: 11/13/2006
ms.assetid: 97c13898-0741-45f9-b3fa-7540ab1679e6
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 81e07bec8569b9ee987dfaa84dec9eec95a2692f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78576285"
---
# <a name="sorting-data-in-a-datalist-or-repeater-control-vb"></a>Tri des données dans un contrôle DataList ou Repeater (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_45_VB.exe) ou [Télécharger le PDF](sorting-data-in-a-datalist-or-repeater-control-vb/_static/datatutorial45vb1.pdf)

> Dans ce didacticiel, nous allons examiner comment inclure la prise en charge du tri dans le contrôle DataList et Repeater, ainsi que comment construire un contrôle DataList ou Repeater dont les données peuvent être paginées et triées.

## <a name="introduction"></a>Introduction

Dans le [didacticiel précédent](paging-report-data-in-a-datalist-or-repeater-control-vb.md) , nous avons examiné comment ajouter la prise en charge de la pagination à un contrôle DataList. Nous avons créé une nouvelle méthode dans la classe `ProductsBLL` (`GetProductsAsPagedDataSource`) qui a retourné un objet `PagedDataSource`. Lorsqu’il est lié à un contrôle DataList ou Repeater, le contrôle DataList ou Repeater affiche uniquement la page de données demandée. Cette technique est similaire à ce qui est utilisé en interne par les contrôles GridView, DetailsView et FormView pour fournir leurs fonctionnalités de pagination par défaut intégrées.

Outre la prise en charge de la pagination, le GridView offre également une prise en charge de tri prête à l’emploi. Ni DataList ni Repeater ne fournissent des fonctionnalités de tri intégrées. Toutefois, les fonctionnalités de tri peuvent être ajoutées à l’aide d’un peu de code. Dans ce didacticiel, nous allons examiner comment inclure la prise en charge du tri dans le contrôle DataList et Repeater, ainsi que comment construire un contrôle DataList ou Repeater dont les données peuvent être paginées et triées.

## <a name="a-review-of-sorting"></a>Révision du tri

Comme nous l’avons vu dans le didacticiel sur les [données de rapport de pagination et de tri](../paging-and-sorting/paging-and-sorting-report-data-vb.md) , le contrôle GridView fournit une prise en charge de tri prête à l’emploi. Chaque champ GridView peut avoir un `SortExpression`associé, qui indique le champ de données selon lequel trier les données. Lorsque la propriété GridView s `AllowSorting` a la valeur `true`, chaque champ GridView qui a une valeur de propriété `SortExpression` a son en-tête rendu en tant que LinkButton. Lorsqu’un utilisateur clique sur un en-tête de champ GridView particulier, une publication (postback) se produit et les données sont triées en fonction du ou des champs `SortExpression`.

Le contrôle GridView possède également une propriété `SortExpression`, qui stocke le `SortExpression` du champ GridView par lequel les données sont triées. En outre, une propriété de `SortDirection` indique si les données doivent être triées dans l’ordre croissant ou décroissant (si un utilisateur clique deux fois sur un lien de l’en-tête du champ GridView particulier, l’ordre de tri est basculé).

Lorsque le GridView est lié à son contrôle de source de données, il transmet ses propriétés `SortExpression` et `SortDirection` au contrôle de source de données. Le contrôle de source de données récupère les données et les trie en fonction des propriétés `SortExpression` et `SortDirection` fournies. Après le tri des données, le contrôle de source de données le retourne au GridView.

Pour répliquer cette fonctionnalité avec les contrôles DataList ou Repeater, nous devons :

- Créer une interface de tri
- Mémoriser le champ de données à trier et s’il faut Trier par ordre croissant ou décroissant
- Demander à ObjectDataSource de trier les données par un champ de données particulier

Nous allons aborder ces trois tâches dans les étapes 3 et 4. Après cela, nous allons examiner comment inclure la prise en charge de la pagination et du tri dans un contrôle DataList ou Repeater.

## <a name="step-2-displaying-the-products-in-a-repeater"></a>Étape 2 : affichage des produits dans un répéteur

Avant de vous soucier de l’implémentation des fonctionnalités liées au tri, commençons par répertorier les produits dans un contrôle Repeater. Commencez par ouvrir la page `Sorting.aspx` dans le dossier `PagingSortingDataListRepeater`. Ajoutez un contrôle Repeater à la page Web, en affectant à sa propriété `ID` la valeur `SortableProducts`. À partir de la balise active de Repeater s, créez un nouvel ObjectDataSource nommé `ProductsDataSource` et configurez-le pour extraire les données de la méthode de `ProductsBLL` classe s `GetProducts()`. Sélectionnez l’option (aucune) dans les listes déroulantes des onglets insérer, mettre à jour et supprimer.

[![créer un ObjectDataSource et le configurer pour utiliser la méthode GetProductsAsPagedDataSource ()](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image2.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image1.png)

**Figure 1**: créer un ObjectDataSource et le configurer pour qu’il utilise la méthode `GetProductsAsPagedDataSource()` ([cliquez pour afficher l’image en taille réelle](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image3.png))

[![définir les listes déroulantes dans les onglets mettre à jour, insérer et supprimer sur (aucun)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image5.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image4.png)

**Figure 2**: définir les listes déroulantes des onglets mettre à jour, insérer et supprimer sur (aucun) ([cliquez pour afficher l’image en taille réelle](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image6.png))

Contrairement à la DataList, Visual Studio ne crée pas automatiquement un `ItemTemplate` pour le contrôle Repeater après l’avoir lié à une source de données. En outre, nous devons ajouter cette `ItemTemplate` de façon déclarative, car la balise active des contrôles Repeater ne dispose pas de l’option modifier les modèles figurant dans le contrôle DataList. Nous utilisons les mêmes `ItemTemplate` que dans le didacticiel précédent, qui affichait le nom, le fournisseur et la catégorie du produit.

Après avoir ajouté le `ItemTemplate`, le balisage déclaratif Repeater et ObjectDataSource doit ressembler à ce qui suit :

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample1.aspx)]

La figure 3 illustre cette page lorsque vous l’affichez dans un navigateur.

[![chaque nom, fournisseur et catégorie de produit est affiché](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image8.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image7.png)

**Figure 3**: chaque nom, fournisseur et catégorie de produit s’affiche ([cliquez pour afficher l’image en taille réelle](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image9.png))

## <a name="step-3-instructing-the-objectdatasource-to-sort-the-data"></a>Étape 3 : demande au ObjectDataSource de trier les données

Pour trier les données affichées dans le Repeater, nous devons informer l’ObjectDataSource de l’expression de tri par laquelle les données doivent être triées. Avant que l’ObjectDataSource récupère ses données, il déclenche tout d’abord son [événement`Selecting`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting.aspx), ce qui nous permet de spécifier une expression de tri. Le gestionnaire d’événements `Selecting` reçoit un objet de type [`ObjectDataSourceSelectingEventArgs`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.aspx), qui a une propriété nommée [`Arguments`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.arguments.aspx) de type [`DataSourceSelectArguments`](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx). La classe `DataSourceSelectArguments` est conçue pour passer des demandes liées aux données à partir d’un consommateur de données au contrôle de source de données, et comprend une [propriété`SortExpression`](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.sortexpression.aspx).

Pour passer des informations de tri de la page ASP.NET à ObjectDataSource, créez un gestionnaire d’événements pour l’événement `Selecting` et utilisez le code suivant :

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample2.vb)]

La valeur *SortExpression* doit être assignée au nom du champ de données pour trier les données (par exemple, ProductName). Il n’existe pas de propriété relative au sens du tri. par conséquent, si vous souhaitez trier les données dans l’ordre décroissant, ajoutez la description de la chaîne à la valeur *SortExpression* (comme ProductName DESC).

Poursuivez et essayez des valeurs codées en dur différentes pour *SortExpression* et testez les résultats dans un navigateur. Comme le montre la figure 4, quand vous utilisez ProductName DESC comme *SortExpression*, les produits sont triés selon leur nom dans l’ordre alphabétique inversé.

[![les produits sont triés par leur nom dans l’ordre alphabétique inversé](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image11.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image10.png)

**Figure 4**: les produits sont triés selon leur nom dans l’ordre alphabétique inversé ([cliquez pour afficher l’image en taille réelle](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image12.png))

## <a name="step-4-creating-the-sorting-interface-and-remembering-the-sort-expression-and-direction"></a>Étape 4 : création de l’interface de tri et mémorisation de l’expression de tri et de la direction

L’activation de la prise en charge du tri dans le contrôle GridView convertit chaque texte d’en-tête des champs triable en un LinkButton qui, lorsque vous cliquez dessus, trie les données en conséquence. Une telle interface de tri est logique pour le GridView, où ses données sont correctement présentées dans les colonnes. Toutefois, pour les contrôles DataList et Repeater, une interface de tri différente est nécessaire. Une interface de tri courante pour une liste de données (par opposition à une grille de données) est une liste déroulante qui fournit les champs en fonction desquels les données peuvent être triées. Nous allons implémenter une telle interface pour ce didacticiel.

Ajoutez un contrôle Web DropDownList au-dessus du `SortableProducts` Repeater et affectez à sa propriété `ID` la valeur `SortBy`. Dans la Fenêtre Propriétés, cliquez sur les ellipses dans la propriété `Items` pour afficher l’éditeur de collections ListItem. Ajoutez `ListItem` s pour trier les données à l’aide des champs `ProductName`, `CategoryName`et `SupplierName`. Ajoutez également un `ListItem` pour trier les produits par leur nom dans l’ordre alphabétique inversé.

Les propriétés `ListItem` `Text` peuvent être définies sur n’importe quelle valeur (par exemple, nom), mais les propriétés du `Value` doivent être définies sur le nom du champ de données (par exemple, ProductName). Pour trier les résultats par ordre décroissant, ajoutez la description de la chaîne au nom du champ de données, comme ProductName DESC.

![Ajoutez un ListItem pour chacun des champs de données pouvant être triés](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image13.png)

**Figure 5**: ajouter un `ListItem` pour chacun des champs de données pouvant être triés

Enfin, ajoutez un contrôle Web Button à droite du contrôle DropDownList. Définissez ses `ID` sur `RefreshRepeater` et sa propriété `Text` pour actualiser.

Après avoir créé le `ListItem` et ajouté le bouton actualiser, la syntaxe déclarative du bouton et de la DropDownList doit ressembler à ce qui suit :

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample3.aspx)]

Une fois le DropDownList de tri terminé, nous devons ensuite mettre à jour le gestionnaire d’événements ObjectDataSource s `Selecting` pour qu’il utilise la propriété `Value` `SortBy``ListItem` s sélectionnée par opposition à une expression de tri codée en dur.

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample4.vb)]

À ce stade, lors de la première visite de la page, les produits sont triés initialement par le champ de données `ProductName`, car il s’agit de la `SortBy` `ListItem` sélectionnée par défaut (voir figure 6). Si vous sélectionnez une option de tri différente, par exemple catégorie et que vous cliquez sur Actualiser, une publication (postback) est exécutée et les données sont à nouveau triées par nom de catégorie, comme illustré dans la figure 7.

[![les produits sont triés initialement par leur nom](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image15.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image14.png)

**Figure 6**: les produits sont initialement triés par nom ([cliquez pour afficher l’image en taille réelle](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image16.png))

[![les produits sont désormais triés par catégorie](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image18.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image17.png)

**Figure 7**: les produits sont désormais triés par catégorie ([cliquez pour afficher l’image en taille réelle](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image19.png))

> [!NOTE]
> Si vous cliquez sur le bouton actualiser, les données sont automatiquement retriées, car l’état d’affichage de l’option Repeater s a été désactivé, ce qui amène le Repeater à effectuer une nouvelle liaison à sa source de données à chaque publication (postback). Si vous laissez l’état d’affichage de Repeater s activé, la modification de la liste déroulante de tri n’a aucune incidence sur l’ordre de tri. Pour remédier à cela, créez un gestionnaire d’événements pour le bouton actualiser les `Click` événement et liez de nouveau le répéteur à sa source de données (en appelant la méthode Repeater s `DataBind()`).

## <a name="remembering-the-sort-expression-and-direction"></a>Mémorisation de l’expression de tri et de la direction

Lors de la création d’un contrôle DataList ou Repeater pouvant être trié sur une page où des publications non liées à un tri peuvent se produire, il est impératif que l’expression de tri et la direction soient mémorisées entre les publications. Par exemple, imaginez que nous avons mis à jour le Repeater dans ce didacticiel pour inclure un bouton supprimer avec chaque produit. Quand l’utilisateur clique sur le bouton supprimer, nous allons exécuter du code pour supprimer le produit sélectionné, puis lier de nouveau les données au répéteur. Si les détails de tri ne sont pas rendus persistants sur la publication (postback), les données affichées à l’écran sont rétablies dans l’ordre de tri d’origine.

Pour ce didacticiel, le DropDownList enregistre implicitement l’expression de tri et le sens dans son état d’affichage pour nous. Si nous avions utilisé une interface de tri différente avec, disons, LinkButtons qui fournissait les différentes options de tri, nous devons veiller à mémoriser l’ordre de tri entre les publications. Cela peut être accompli en stockant les paramètres de tri dans l’état d’affichage de la page, en incluant le paramètre de tri dans la chaîne de chaîne ou via une autre technique de persistance de l’État.

Les exemples futurs de ce didacticiel explorent comment rendre les détails de tri persistants dans l’état d’affichage de la page.

## <a name="step-5-adding-sorting-support-to-a-datalist-that-uses-default-paging"></a>Étape 5 : ajout de la prise en charge du tri à un contrôle DataList qui utilise la pagination par défaut

Dans le [didacticiel précédent](paging-report-data-in-a-datalist-or-repeater-control-vb.md) , nous avons examiné comment implémenter la pagination par défaut avec un contrôle DataList. Nous allons étendre cet exemple précédent pour inclure la possibilité de trier les données paginées. Commencez par ouvrir les pages `SortingWithDefaultPaging.aspx` et `Paging.aspx` dans le dossier `PagingSortingDataListRepeater`. Dans la page `Paging.aspx`, cliquez sur le bouton source pour afficher le balisage déclaratif de la page. Copiez le texte sélectionné (voir figure 8) et collez-le dans le balisage déclaratif de `SortingWithDefaultPaging.aspx` entre les balises `<asp:Content>`.

[![répliquer le balisage déclaratif dans les balises &lt;asp : content&gt; de paging. aspx vers SortingWithDefaultPaging. aspx](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image21.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image20.png)

**Figure 8**: répliquer la balise déclarative dans les balises `<asp:Content>` de `Paging.aspx` à `SortingWithDefaultPaging.aspx` ([cliquez pour afficher l’image en taille réelle](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image22.png))

Après avoir copié le balisage déclaratif, copiez les méthodes et les propriétés de la `Paging.aspx` classe code-behind de la page dans la classe code-behind pour `SortingWithDefaultPaging.aspx`. Ensuite, prenez un moment pour afficher la page `SortingWithDefaultPaging.aspx` dans un navigateur. Elle doit présenter les mêmes fonctionnalités et apparences que `Paging.aspx`.

## <a name="enhancing-productsbll-to-include-a-default-paging-and-sorting-method"></a>Amélioration de ProductsBLL pour inclure une méthode de tri et de pagination par défaut

Dans le didacticiel précédent, nous avons créé une méthode `GetProductsAsPagedDataSource(pageIndex, pageSize)` dans la classe `ProductsBLL` qui a retourné un objet `PagedDataSource`. Cet objet `PagedDataSource` a été rempli avec *tous* les produits (via la méthode BLL s `GetProducts()`), mais lorsqu’il est lié au contrôle DataList, seuls les enregistrements correspondant aux paramètres d’entrée *pageIndex* et *pageSize* spécifiés ont été affichés.

Plus haut dans ce didacticiel, nous avons ajouté la prise en charge du tri en spécifiant l’expression de tri du gestionnaire d’événements `Selecting` ObjectDataSource s. Cela fonctionne bien lorsque ObjectDataSource est retourné en tant qu’objet pouvant être trié, comme le `ProductsDataTable` retourné par la méthode `GetProducts()`. Toutefois, l’objet `PagedDataSource` retourné par la méthode `GetProductsAsPagedDataSource` ne prend pas en charge le tri de sa source de données interne. Au lieu de cela, nous devons trier les résultats retournés par la méthode `GetProducts()` *avant* de les placer dans le `PagedDataSource`.

Pour ce faire, créez une nouvelle méthode dans la classe `ProductsBLL`, `GetProductsSortedAsPagedDataSource(sortExpression, pageIndex, pageSize)`. Pour trier le `ProductsDataTable` retourné par la méthode `GetProducts()`, spécifiez la propriété `Sort` de son `DataTableView`par défaut :

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample5.vb)]

La méthode `GetProductsSortedAsPagedDataSource` diffère légèrement de la méthode `GetProductsAsPagedDataSource` créée dans le didacticiel précédent. En particulier, `GetProductsSortedAsPagedDataSource` accepte un paramètre d’entrée supplémentaire `sortExpression` et attribue cette valeur à la propriété `Sort` de l' `DefaultView``ProductDataTable` s. Quelques lignes de code par la suite, le `DefaultView``ProductDataTable` s est affecté à la source de `PagedDataSource` objet s.

## <a name="calling-the-getproductssortedaspageddatasource-method-and-specifying-the-value-for-the-sortexpression-input-parameter"></a>Appel de la méthode GetProductsSortedAsPagedDataSource et spécification de la valeur du paramètre d’entrée SortExpression

Une fois la méthode `GetProductsSortedAsPagedDataSource` terminée, l’étape suivante consiste à fournir la valeur de ce paramètre. L’ObjectDataSource dans `SortingWithDefaultPaging.aspx` est actuellement configuré pour appeler la méthode `GetProductsAsPagedDataSource` et passe les deux paramètres d’entrée par le biais de ses deux `QueryStringParameters`, qui sont spécifiés dans la collection `SelectParameters`. Ces deux `QueryStringParameters` indiquent que la source des paramètres de la méthode `GetProductsAsPagedDataSource` s *pageIndex* et *pageSize* provient des champs QueryString `pageIndex` et `pageSize`.

Mettez à jour la propriété ObjectDataSource s `SelectMethod` pour qu’elle appelle la nouvelle méthode `GetProductsSortedAsPagedDataSource`. Ajoutez ensuite un nouveau `QueryStringParameter` pour que le paramètre d’entrée *SortExpression* soit accessible à partir du champ QueryString `sortExpression`. Définissez le `DefaultValue` de `QueryStringParameter` s sur ProductName.

Une fois ces modifications effectuées, le balisage déclaratif s doit ressembler à ce qui suit :

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample6.aspx)]

À ce stade, la page de `SortingWithDefaultPaging.aspx` trie les résultats par ordre alphabétique selon le nom du produit (voir figure 9). Cela est dû au fait que, par défaut, la valeur ProductName est transmise en tant que `GetProductsSortedAsPagedDataSource` méthode s *SortExpression* .

[![par défaut, les résultats sont triés par NomProduit](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image24.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image23.png)

**Figure 9**: par défaut, les résultats sont triés par `ProductName` ([cliquez pour afficher l’image en taille réelle](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image25.png))

Si vous ajoutez manuellement un champ de chaîne de requête `sortExpression` comme `SortingWithDefaultPaging.aspx?sortExpression=CategoryName` les résultats seront triés par le `sortExpression`spécifié. Toutefois, ce paramètre de `sortExpression` n’est pas inclus dans la chaîne de chaîne lors du déplacement vers une autre page de données. En fait, un clic sur les boutons suivant ou précédent de la page nous ramène à `Paging.aspx`! En outre, il n’existe actuellement aucune interface de tri. La seule façon dont un utilisateur peut modifier l’ordre de tri des données paginées est de manipuler directement la chaîne de chaîne.

## <a name="creating-the-sorting-interface"></a>Création de l’interface de tri

Nous devons tout d’abord mettre à jour la méthode `RedirectUser` pour envoyer l’utilisateur à `SortingWithDefaultPaging.aspx` (au lieu de `Paging.aspx`) et inclure la valeur `sortExpression` dans la chaîne de recherche. Nous devons également ajouter une propriété `SortExpression` nommée au niveau de la page, en lecture seule. Cette propriété, similaire aux propriétés `PageIndex` et `PageSize` créées dans le didacticiel précédent, retourne la valeur du champ `sortExpression` QueryString s’il existe, et la valeur par défaut (ProductName) dans le cas contraire.

Actuellement, la méthode `RedirectUser` n’accepte qu’un seul paramètre d’entrée, l’index de la page à afficher. Toutefois, il peut arriver que vous souhaitiez rediriger l’utilisateur vers une page de données particulière à l’aide d’une expression de tri autre que celle spécifiée dans la chaîne de chaîne. Dans un moment, nous allons créer l’interface de tri pour cette page, qui inclura une série de contrôles Web Button pour trier les données selon une colonne spécifiée. Quand l’utilisateur clique sur l’un de ces boutons, nous souhaitons rediriger l’utilisateur qui passe la valeur d’expression de tri appropriée. Pour fournir cette fonctionnalité, créez deux versions de la méthode `RedirectUser`. Le premier doit accepter uniquement l’index de page à afficher, tandis que le second accepte l’index de page et l’expression de tri.

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample7.vb)]

Dans le premier exemple de ce didacticiel, nous avons créé une interface de tri à l’aide d’un DropDownList. Pour cet exemple, Let utilise trois contrôles Web Button placés au-dessus de DataList pour le tri par `ProductName`, un pour `CategoryName`et un pour `SupplierName`. Ajoutez les trois contrôles Web Button, en définissant leurs propriétés `ID` et `Text` de manière appropriée :

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample8.aspx)]

Ensuite, créez un gestionnaire d’événements `Click` pour chaque. Les gestionnaires d’événements doivent appeler la méthode `RedirectUser`, en retournant l’utilisateur à la première page à l’aide de l’expression de tri appropriée.

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample9.vb)]

Lors de la première visite de la page, les données sont triées par nom de produit par ordre alphabétique (reportez-vous à la figure 9). Cliquez sur le bouton suivant pour passer à la deuxième page de données, puis cliquez sur le bouton Trier par catégorie. Cela nous renvoie à la première page de données, triée par nom de catégorie (voir figure 10). De même, si vous cliquez sur le bouton Trier par fournisseur, les données par fournisseur sont triées à partir de la première page de données. Le choix de tri est mémorisé lorsque les données sont paginées. La figure 11 illustre la page après le tri par catégorie et l’avancement jusqu’à la treizième page de données.

[![les produits sont triés par catégorie](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image27.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image26.png)

**Figure 10**: les produits sont triés par catégorie ([cliquez pour afficher l’image en taille réelle](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image28.png))

[![l’expression de tri est mémorisée lors de la pagination des données](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image30.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image29.png)

**Figure 11**: l’expression de tri est mémorisée lors de la pagination des données ([cliquez pour afficher l’image en taille réelle](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image31.png))

## <a name="step-6-custom-paging-through-records-in-a-repeater"></a>Étape 6 : pagination personnalisée des enregistrements dans un Repeater

L’exemple DataList examiné à l’étape 5 pagine ses données à l’aide de la technique de pagination par défaut inefficace. Lors de la pagination de quantités de données suffisamment volumineuses, il est impératif d’utiliser la pagination personnalisée. De retour dans la [pagination efficace de grandes quantités de données](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) et de tri des didacticiels de [données personnalisés](../paging-and-sorting/sorting-custom-paged-data-vb.md) , nous avons examiné les différences entre la pagination par défaut et la pagination personnalisée et les méthodes créées dans la couche BLL pour l’utilisation de la pagination personnalisée et le tri des données paginées personnalisées. En particulier, dans ces deux didacticiels précédents, nous avons ajouté les trois méthodes suivantes à la classe `ProductsBLL` :

- `GetProductsPaged(startRowIndex, maximumRows)` retourne un sous-ensemble particulier d’enregistrements à partir de *StartRowIndex* et ne dépassant pas *MaximumRows*.
- `GetProductsPagedAndSorted(sortExpression, startRowIndex, maximumRows)` retourne un sous-ensemble particulier d’enregistrements triés selon le paramètre d’entrée *SortExpression* spécifié.
- `TotalNumberOfProducts()` fournit le nombre total d’enregistrements dans la table de base de données `Products`.

Ces méthodes peuvent être utilisées pour mettre en page et trier efficacement des données à l’aide d’un contrôle DataList ou Repeater. Pour illustrer cela, commençons par créer un contrôle Repeater avec prise en charge de la pagination personnalisée. Nous allons ensuite ajouter des fonctionnalités de tri.

Ouvrez la page `SortingWithCustomPaging.aspx` dans le dossier `PagingSortingDataListRepeater` et ajoutez un répéteur à la page, en affectant à sa propriété `ID` la valeur `Products`. À partir de la balise active de Repeater s, créez un nouvel ObjectDataSource nommé `ProductsDataSource`. Configurez-le pour qu’il sélectionne ses données à partir de la méthode `ProductsBLL` classe s `GetProductsPaged`.

[![configurer ObjectDataSource pour utiliser la méthode ProductsBLL de la classe s GetProductsPaged](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image33.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image32.png)

**Figure 12**: configurer ObjectDataSource pour utiliser la méthode `ProductsBLL` classe s `GetProductsPaged` ([cliquez pour afficher l’image en taille réelle](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image34.png))

Définissez les listes déroulantes dans les onglets mettre à jour, insérer et supprimer sur (aucun), puis cliquez sur le bouton suivant. L’Assistant Configuration de la source de données vous invite à indiquer les sources des paramètres d’entrée *StartRowIndex* et *MaximumRows* de `GetProductsPaged` la méthode. En réalité, ces paramètres d’entrée sont ignorés. Au lieu de cela, les valeurs *StartRowIndex* et *MaximumRows* sont transmises via la propriété `Arguments` dans le gestionnaire d’événements ObjectDataSource s `Selecting`, tout comme nous avons spécifié le *SortExpression* dans ce didacticiel, la première démo. Par conséquent, laissez les listes déroulantes source de paramètres dans l’Assistant définies sur aucun.

[![laissez les sources de paramètres définies sur aucun](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image36.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image35.png)

**Figure 13**: conserver les sources de paramètres définies sur aucun ([cliquez pour afficher l’image en taille réelle](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image37.png))

> [!NOTE]
> N’affectez *pas* la valeur `true`à la propriété ObjectDataSource s `EnablePaging`. En conséquence, ObjectDataSource inclut automatiquement ses propres paramètres *StartRowIndex* et *MaximumRows* dans la liste de paramètres existants du `SelectMethod`. La propriété `EnablePaging` est utile lors de la liaison de données paginées personnalisées à un contrôle GridView, DetailsView ou FormView, car ces contrôles attendent certains comportements de l’ObjectDataSource qui est uniquement disponible lorsque `EnablePaging` propriété est `true`. Étant donné que nous devons ajouter manuellement la prise en charge de la pagination pour les contrôles DataList et Repeater, laissez cette propriété définie sur `false` (valeur par défaut), car nous intégrerons les fonctionnalités nécessaires directement dans notre page ASP.NET.

Enfin, définissez le `ItemTemplate` Repeater afin que le nom, la catégorie et le fournisseur du produit soient affichés. Après ces modifications, la syntaxe déclarative de Repeater et ObjectDataSource doit ressembler à ce qui suit :

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample10.aspx)]

Prenez un moment pour accéder à la page via un navigateur et Notez qu’aucun enregistrement n’est retourné. Cela est dû au fait que nous n’avons pas encore spécifié les valeurs des paramètres *StartRowIndex* et *MaximumRows* ; par conséquent, les valeurs 0 sont transmises pour les deux. Pour spécifier ces valeurs, créez un gestionnaire d’événements pour l’événement ObjectDataSource s `Selecting` et définissez ces valeurs de paramètres par programmation sur les valeurs codées en dur de 0 et 5, respectivement :

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample11.vb)]

Avec cette modification, la page, affichée dans un navigateur, affiche les cinq premiers produits.

[![les cinq premiers enregistrements sont affichés](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image39.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image38.png)

**Figure 14**: les cinq premiers enregistrements sont affichés ([cliquez pour afficher l’image en taille réelle](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image40.png))

> [!NOTE]
> Les produits listés dans la figure 14 sont triés par nom de produit, car la procédure stockée `GetProductsPaged` qui effectue la requête de pagination personnalisée efficace trie les résultats en `ProductName`.

Pour permettre à l’utilisateur d’effectuer un pas à pas détaillé des pages, nous devons suivre l’index de début de ligne et le nombre maximal de lignes et mémoriser ces valeurs entre les publications. Dans l’exemple de pagination par défaut, nous avons utilisé des champs QueryString pour conserver ces valeurs. pour cette démonstration, laissez ces informations persistantes dans l’état d’affichage de la page. Créez les deux propriétés suivantes :

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample12.vb)]

Ensuite, mettez à jour le code dans le gestionnaire d’événements de sélection afin qu’il utilise les propriétés `StartRowIndex` et `MaximumRows` à la place des valeurs codées en dur de 0 et 5 :

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample13.vb)]

À ce stade, notre page affiche toujours uniquement les cinq premiers enregistrements. Toutefois, avec ces propriétés en place, nous sommes prêts à créer notre interface de pagination.

## <a name="adding-the-paging-interface"></a>Ajout de l’interface de pagination

Ils utilisent la même interface de pagination, précédente, suivante et la dernière que celle utilisée dans l’exemple de pagination par défaut, y compris le contrôle Web Label qui affiche la page de données affichée et le nombre total de pages disponibles. Ajoutez les contrôles Web et l’étiquette à quatre boutons sous le répéteur.

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample14.aspx)]

Ensuite, créez `Click` gestionnaires d’événements pour les quatre boutons. Lorsque vous cliquez sur l’un de ces boutons, nous devons mettre à jour les `StartRowIndex` et relier les données au répéteur. Le code pour les boutons premier, précédent et suivant est assez simple, mais pour le dernier bouton, comment déterminer l’index de la ligne de début pour la dernière page de données ? Pour calculer cet index et être en mesure de déterminer si les boutons suivant et précédent doivent être activés, nous devons savoir combien d’enregistrements du total sont paginés. Nous pouvons le déterminer en appelant la méthode `ProductsBLL` classe s `TotalNumberOfProducts()`. Créons une propriété en lecture seule, au niveau de la page nommée `TotalRowCount` qui retourne les résultats de la méthode `TotalNumberOfProducts()` :

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample15.vb)]

Avec cette propriété, nous pouvons maintenant déterminer l’index de ligne de début de la dernière page. Plus précisément, il s’agit du résultat de l’entier du `TotalRowCount` moins 1 divisé par `MaximumRows`, multiplié par `MaximumRows`. Nous pouvons maintenant écrire les gestionnaires d’événements `Click` pour les quatre boutons d’interface de pagination :

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample16.vb)]

Enfin, nous devons désactiver le premier et le précédent bouton dans l’interface de pagination lors de l’affichage de la première page de données, ainsi que des boutons suivant et précédent lors de l’affichage de la dernière page. Pour ce faire, ajoutez le code suivant au gestionnaire d’événements `Selecting` ObjectDataSource s :

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample17.vb)]

Après avoir ajouté ces `Click` gestionnaires d’événements et le code pour activer ou désactiver les éléments de l’interface de pagination en fonction de l’index de la ligne de début en cours, testez la page dans un navigateur. Comme illustré à la figure 15, lorsque vous accédez à la page pour la première fois, le premier bouton et le précédent sont désactivés. Cliquez sur suivant pour afficher la deuxième page de données, tout en cliquant sur dernier pour afficher la dernière page (voir figures 16 et 17). Lorsque vous affichez la dernière page de données, les boutons suivant et précédent sont désactivés.

[![le précédent et le dernier bouton sont désactivés lors de l’affichage de la première page de produits](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image42.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image41.png)

**Figure 15**: les boutons précédent et précédent sont désactivés lors de l’affichage de la première page de produits ([cliquez pour afficher l’image en taille réelle](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image43.png))

[![la deuxième page de produits s’affiche](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image45.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image44.png)

**Figure 16**: la deuxième page de produits s’affiche ([cliquez pour afficher l’image en taille réelle](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image46.png))

[![cliquant sur dernier affiche la dernière page de données](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image48.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image47.png)

**Figure 17**: cliquer sur dernier affiche la dernière page de données ([cliquez pour afficher l’image en taille réelle](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image49.png))

## <a name="step-7-including-sorting-support-with-the-custom-paged-repeater"></a>Étape 7 : inclure la prise en charge du tri avec le répéteur paginé personnalisé

Maintenant que la pagination personnalisée a été implémentée, nous sommes prêts à inclure la prise en charge du tri. La méthode `ProductsBLL` classe s `GetProductsPagedAndSorted` a les mêmes paramètres d’entrée *StartRowIndex* et *MaximumRows* que `GetProductsPaged`, mais autorise un paramètre d’entrée *SortExpression* supplémentaire. Pour utiliser la méthode `GetProductsPagedAndSorted` à partir de `SortingWithCustomPaging.aspx`, nous devons effectuer les étapes suivantes :

1. Modifiez la propriété ObjectDataSource s `SelectMethod` de `GetProductsPaged` en `GetProductsPagedAndSorted`.
2. Ajoutez un objet `Parameter` *SortExpression* à la collection d' `SelectParameters` ObjectDataSource s.
3. Créez une propriété de `SortExpression` au niveau de la page privée qui rend sa valeur persistante sur les publications (postback) à l’état d’affichage de la page.
4. Mettez à jour le gestionnaire d’événements ObjectDataSource s `Selecting` pour attribuer au paramètre ObjectDataSource s *SortExpression* la valeur de la propriété `SortExpression` au niveau de la page.
5. Créez l’interface de tri.

Commencez par mettre à jour la propriété ObjectDataSource s `SelectMethod` et ajoutez un `Parameter`*SortExpression* . Assurez-vous que la propriété *sortExpression* `Parameter` s `Type` est définie sur `String`. Une fois ces deux premières tâches terminées, le balisage déclaratif s doit se présenter comme suit :

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample18.aspx)]

Ensuite, nous avons besoin d’une propriété de `SortExpression` au niveau de la page dont la valeur est sérialisée à l’état d’affichage. Si aucune valeur d’expression de tri n’a été définie, utilisez ProductName comme valeur par défaut :

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample19.vb)]

Avant que ObjectDataSource n’appelle la méthode `GetProductsPagedAndSorted`, nous devons définir la `Parameter` *SortExpression* sur la valeur de la propriété `SortExpression`. Dans le gestionnaire d’événements `Selecting`, ajoutez la ligne de code suivante :

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample20.vb)]

Il ne reste plus qu’à implémenter l’interface de tri. Comme nous l’avons fait dans le dernier exemple, nous allons faire en sorte que l’interface de tri soit implémentée à l’aide de trois contrôles Web Button qui permettent à l’utilisateur de trier les résultats par nom de produit, catégorie ou fournisseur.

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample21.aspx)]

Créez `Click` gestionnaires d’événements pour ces trois contrôles Button. Dans le gestionnaire d’événements, réinitialisez la `StartRowIndex` sur 0, affectez la valeur appropriée à la `SortExpression` et liez les données au répétiteur :

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample22.vb)]

C’est tout ! Bien qu’il y ait un certain nombre d’étapes pour mettre en œuvre la pagination et le tri personnalisés, les étapes étaient très similaires à celles nécessaires pour la pagination par défaut. La figure 18 montre les produits lors de l’affichage de la dernière page de données lorsqu’ils sont triés par catégorie.

[![la dernière page de données, triée par catégorie, s’affiche](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image51.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image50.png)

**Figure 18**: la dernière page de données, triée par catégorie, s’affiche ([cliquez pour afficher l’image en taille réelle](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image52.png))

> [!NOTE]
> Dans les exemples précédents, lorsque le tri par fournisseur NomFournisseur était utilisé comme expression de tri. Toutefois, pour l’implémentation personnalisée de la pagination, nous devons utiliser CompanyName. En effet, la procédure stockée responsable de l’implémentation de la pagination personnalisée `GetProductsPagedAndSorted` passe l’expression de tri dans le mot clé `ROW_NUMBER()`, le mot clé `ROW_NUMBER()` requiert le nom réel de la colonne plutôt qu’un alias. Par conséquent, nous devons utiliser `CompanyName` (le nom de la colonne dans la table `Suppliers`) plutôt que l’alias utilisé dans la requête de `SELECT` (`SupplierName`) pour l’expression de tri.

## <a name="summary"></a>Récapitulatif

Ni DataList ni Repeater n’offrent une prise en charge de tri intégrée, mais avec un peu de code et une interface de tri personnalisée, de telles fonctionnalités peuvent être ajoutées. Lors de l’implémentation du tri, mais pas de la pagination, l’expression de tri peut être spécifiée via l’objet `DataSourceSelectArguments` passé dans la méthode d' `Select` ObjectDataSource s. Cette propriété `DataSourceSelectArguments` objet s `SortExpression` peut être assignée dans le gestionnaire d’événements ObjectDataSource s `Selecting`.

Pour ajouter des fonctionnalités de tri à un contrôle DataList ou Repeater qui fournit déjà la prise en charge de la pagination, l’approche la plus simple consiste à personnaliser la couche de logique métier pour inclure une méthode qui accepte une expression de tri. Ces informations peuvent ensuite être transmises par le biais d’un paramètre dans l' `SelectParameters`ObjectDataSource s.

Ce didacticiel termine notre examen de la pagination et du tri avec les contrôles DataList et Repeater. Notre prochain et dernier didacticiel examinera comment ajouter des contrôles Web Button aux modèles DataList et Repeater, afin de fournir des fonctionnalités personnalisées, initiées par l’utilisateur, pour chaque élément.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel était David SURU. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](paging-report-data-in-a-datalist-or-repeater-control-vb.md)
