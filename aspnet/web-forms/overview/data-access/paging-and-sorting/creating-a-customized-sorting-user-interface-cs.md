---
uid: web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-cs
title: Création d’une interface utilisateur de triC#personnalisée () | Microsoft Docs
author: rick-anderson
description: Lorsque vous affichez une longue liste de données triées, il peut être très utile de regrouper les données associées en introduisant des lignes de séparation. Dans ce didacticiel, nous allons voir comment cré...
ms.author: riande
ms.date: 08/15/2006
ms.assetid: 6f81b633-9d01-4e52-ae4a-2ea6bc109475
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 93ec07a13de80e4c874ff46b5dfa626b60b632c8
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74597344"
---
# <a name="creating-a-customized-sorting-user-interface-c"></a>Création d’une interface utilisateur de tri personnalisée (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_27_CS.exe) ou [Télécharger le PDF](creating-a-customized-sorting-user-interface-cs/_static/datatutorial27cs1.pdf)

> Lorsque vous affichez une longue liste de données triées, il peut être très utile de regrouper les données associées en introduisant des lignes de séparation. Dans ce didacticiel, nous allons voir comment créer une interface utilisateur de tri de ce type.

## <a name="introduction"></a>Introduction

Lorsque vous affichez une longue liste de données triées où il n’existe qu’une poignée de valeurs différentes dans la colonne triée, un utilisateur final peut trouver difficile de discerner l’emplacement exact où les limites de différence se produisent. Par exemple, il existe 81 produits dans la base de données, mais seulement neuf choix de catégories différents (huit catégories uniques plus l’option `NULL`). Prenons l’exemple d’un utilisateur qui souhaite examiner les produits qui se trouvent dans la catégorie nourriture. Dans une page qui répertorie *tous* les produits d’un seul GridView, l’utilisateur peut décider que son meilleur résultat est de trier les résultats par catégorie, ce qui regroupe tous les produits de la mer. Après le tri par catégorie, l’utilisateur doit ensuite parcourir la liste pour Rechercher l’endroit où les produits regroupés en mer commencent et se terminent. Étant donné que les résultats sont classés par ordre alphabétique selon le nom de catégorie, la recherche des produits de la mer n’est pas difficile, mais elle nécessite toujours d’analyser attentivement la liste des éléments dans la grille.

Pour aider à mettre en évidence les limites entre les groupes triés, de nombreux sites Web utilisent une interface utilisateur qui ajoute un séparateur entre ces groupes. Les séparateurs comme ceux illustrés à la figure 1 permettent à un utilisateur de trouver plus rapidement un groupe particulier et d’identifier ses limites, ainsi que de déterminer les groupes distincts qui existent dans les données.

[![chaque groupe de catégories est clairement identifié](creating-a-customized-sorting-user-interface-cs/_static/image2.png)](creating-a-customized-sorting-user-interface-cs/_static/image1.png)

**Figure 1**: chaque groupe de catégories est clairement identifié ([cliquez pour afficher l’image en taille réelle](creating-a-customized-sorting-user-interface-cs/_static/image3.png))

Dans ce didacticiel, nous allons voir comment créer une interface utilisateur de tri de ce type.

## <a name="step-1-creating-a-standard-sortable-gridview"></a>Étape 1 : création d’un GridView standard et triable

Avant de découvrir comment compléter le GridView pour fournir l’interface de tri améliorée, créons d’abord un GridView standard et pouvant être trié qui répertorie les produits. Commencez par ouvrir la page `CustomSortingUI.aspx` dans le dossier `PagingAndSorting`. Ajoutez un GridView à la page, affectez à sa propriété `ID` la valeur `ProductList`et liez-le à un nouvel ObjectDataSource. Configurez le ObjectDataSource pour utiliser la méthode de `GetProducts()` de la classe `ProductsBLL` pour sélectionner des enregistrements.

Ensuite, configurez le contrôle GridView de manière à ce qu’il ne contienne que les `ProductName`, `CategoryName`, `SupplierName`et `UnitPrice` BoundFields et les CheckBoxField abandonnés. Enfin, configurez le contrôle GridView pour prendre en charge le tri en activant la case à cocher Activer le tri dans la balise active GridView s (ou en affectant à sa propriété `AllowSorting` la valeur `true`). Après avoir apporté ces ajouts à la page `CustomSortingUI.aspx`, le balisage déclaratif doit ressembler à ce qui suit :

[!code-aspx[Main](creating-a-customized-sorting-user-interface-cs/samples/sample1.aspx)]

Prenez un moment pour voir notre progression jusqu’à présent dans un navigateur. La figure 2 montre le GridView pouvant être trié lorsque ses données sont triées par catégorie par ordre alphabétique.

[![les données de GridView triées sont classées par catégorie](creating-a-customized-sorting-user-interface-cs/_static/image5.png)](creating-a-customized-sorting-user-interface-cs/_static/image4.png)

**Figure 2**: les données GridView triées sont classées par catégorie ([cliquez pour afficher l’image en taille réelle](creating-a-customized-sorting-user-interface-cs/_static/image6.png))

## <a name="step-2-exploring-techniques-for-adding-the-separator-rows"></a>Étape 2 : exploration des techniques d’ajout des lignes de séparation

Une fois le GridView générique pouvant être trié complet, il ne reste plus qu’à pouvoir ajouter les lignes de séparation dans le contrôle GridView avant chaque groupe trié unique. Mais comment les lignes peuvent-elles être injectées dans le GridView ? Pour l’essentiel, nous devons itérer au sein des lignes du contrôle GridView, déterminer où les différences se produisent entre les valeurs de la colonne triée, puis ajouter la ligne de séparation appropriée. Lorsque vous réfléchissez à ce problème, il semble naturel que la solution se trouve quelque part dans le gestionnaire d’événements `RowDataBound` GridView s. Comme nous l’avons vu dans le didacticiel [sur la mise en forme personnalisée basée](../custom-formatting/custom-formatting-based-upon-data-cs.md) sur les données, ce gestionnaire d’événements est couramment utilisé lors de l’application d’une mise en forme au niveau des lignes basée sur les données des lignes. Toutefois, le gestionnaire d’événements `RowDataBound` n’est pas la solution ici, car les lignes ne peuvent pas être ajoutées par programmation à GridView à partir de ce gestionnaire d’événements. La collection de `Rows` GridView est en fait en lecture seule.

Pour ajouter des lignes à GridView, nous avons trois possibilités :

- Ajoutez ces lignes de séparateur de métadonnées aux données réelles qui sont liées au contrôle GridView.
- Une fois que le GridView a été lié aux données, ajoutez des instances de `TableRow` supplémentaires à la collection de contrôles GridView s
- Créer un contrôle serveur personnalisé qui étend le contrôle GridView et substitue les méthodes responsables de la construction de la structure de GridView s

La création d’un contrôle serveur personnalisé serait la meilleure approche si cette fonctionnalité était nécessaire sur de nombreuses pages Web ou sur plusieurs sites Web. Toutefois, cela impliquerait un peu de code et une exploration minutieuse de la profondeur du fonctionnement interne de GridView s. Par conséquent, nous ne prenons pas en compte cette option pour ce didacticiel.

Les deux autres options ajoutent des lignes de séparation aux données réelles liées au GridView et la manipulation de la collection de contrôles GridView s après sa liaison, attaquent le problème différemment et méritent une discussion.

## <a name="adding-rows-to-the-data-bound-to-the-gridview"></a>Ajout de lignes aux données liées au GridView

Lorsque le GridView est lié à une source de données, il crée un `GridViewRow` pour chaque enregistrement retourné par la source de données. Par conséquent, nous pouvons injecter les lignes de séparation nécessaires en ajoutant des enregistrements de séparateur à la source de données avant de la lier au GridView. La figure 3 illustre ce concept.

![L’une des techniques consiste à ajouter des lignes de séparation à la source de données](creating-a-customized-sorting-user-interface-cs/_static/image7.png)

**Figure 3**: une technique implique l’ajout de lignes de séparation à la source de données

J’utilise les enregistrements de séparateur de termes entre guillemets, car il n’y a pas d’enregistrement de séparateur spécial ; au lieu de cela, nous devons signaler qu’un enregistrement particulier dans la source de données sert de séparateur plutôt qu’en tant que ligne de données normale. Pour nos exemples, nous créons une liaison de `ProductsDataTable` instance vers le GridView, qui est composé de `ProductRows`. Nous pouvons marquer un enregistrement comme ligne de séparateur en affectant à sa propriété `CategoryID` la valeur `-1` (car une telle valeur ne peut pas exister normalement).

Pour utiliser cette technique, nous avons besoin d’effectuer les étapes suivantes :

1. Récupérer par programmation les données à lier au GridView (une instance `ProductsDataTable`)
2. Trier les données en fonction du `SortExpression` GridView et des propriétés de `SortDirection`
3. Itérez au sein de la `ProductsRows` dans le `ProductsDataTable`, en recherchant où se trouvent les différences dans la colonne triée
4. À chaque limite de groupe, injecter un enregistrement de séparateur `ProductsRow` instance dans le DataTable, une qui lui a `CategoryID` définie sur `-1` (ou toute autre désignation qui marque un enregistrement comme un enregistrement de séparateur)
5. Après avoir injecté les lignes de séparation, liez par programmation les données au GridView

En plus de ces cinq étapes, nous devons également fournir un gestionnaire d’événements pour l’événement `RowDataBound` GridView s. Ici, nous allons vérifier chaque `DataRow` et déterminer s’il s’agit d’une ligne de séparation, une ligne dont le paramètre `CategoryID` a été `-1`. Dans ce cas, nous voulons probablement ajuster sa mise en forme ou le texte affiché dans la ou les cellules.

L’utilisation de cette technique pour injecter les limites d’un groupe de tri nécessite un peu plus de travail que celui décrit ci-dessus, car vous devez également fournir un gestionnaire d’événements pour l’événement `Sorting` GridView et effectuer le suivi des valeurs de `SortExpression` et de `SortDirection`.

## <a name="manipulating-the-gridview-s-control-collection-after-it-s-been-databound"></a>Manipulation de la collection de contrôles GridView s après qu’elle a été liée aux DataBound

Au lieu de la messagerie des données avant de les lier au GridView, nous pouvons ajouter les lignes de séparation *une fois que* les données ont été liées au GridView. Le processus de liaison de données génère la hiérarchie des contrôles GridView s, qui en réalité est simplement une instance `Table` composée d’une collection de lignes, chacune d’elles étant composée d’une collection de cellules. Plus précisément, la collection de contrôles GridView s contient un objet `Table` à sa racine, un `GridViewRow` (dérivé de la classe `TableRow`) pour chaque enregistrement dans le `DataSource` lié au GridView et un objet `TableCell` dans chaque instance `GridViewRow` pour chaque champ de données dans le `DataSource`.

Pour ajouter des lignes de séparation entre chaque groupe de tri, nous pouvons directement manipuler cette hiérarchie de contrôle une fois qu’elle a été créée. Nous pouvons être sûrs que la hiérarchie des contrôles GridView s a été créée pour la dernière fois lors du rendu de la page. Par conséquent, cette approche remplace la méthode `Page` classe s `Render`, à laquelle la hiérarchie des contrôles finaux de GridView s est mise à jour pour inclure les lignes de séparation nécessaires. La figure 4 illustre ce processus.

[![une autre technique manipule la hiérarchie des contrôles GridView s](creating-a-customized-sorting-user-interface-cs/_static/image9.png)](creating-a-customized-sorting-user-interface-cs/_static/image8.png)

**Figure 4**: une autre technique manipule la hiérarchie des contrôles GridView s ([cliquez pour afficher l’image en taille réelle](creating-a-customized-sorting-user-interface-cs/_static/image10.png))

Pour ce didacticiel, nous allons utiliser cette dernière approche pour personnaliser l’expérience utilisateur de tri.

> [!NOTE]
> Le code que j’ai présenté dans ce didacticiel est basé sur l’exemple fourni dans l’entrée de blog [Teemu Keiski](http://aspadvice.com/blogs/joteke/default.aspx) s, en [lisant un peu avec le regroupement de tri de GridView](http://aspadvice.com/blogs/joteke/archive/2006/02/11/15130.aspx).

## <a name="step-3-adding-the-separator-rows-to-the-gridview-s-control-hierarchy"></a>Étape 3 : ajout des lignes de séparation à la hiérarchie des contrôles GridView s

Étant donné que nous voulons uniquement ajouter les lignes de séparation à la hiérarchie des contrôles GridView s après la création et la création de sa hiérarchie de contrôles pour la dernière fois sur cette page, nous voulons effectuer cette addition à la fin du cycle de vie de la page, mais avant le contrôle GridView réel la hiérarchie de la hiérarchie a été rendue en HTML. Le dernier point possible à partir duquel nous pouvons y parvenir est l’événement `Page` Class s `Render`, que nous pouvons remplacer dans notre classe code-behind à l’aide de la signature de méthode suivante :

[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample2.cs)]

Lorsque la méthode d' `Render` d’origine de la classe `Page` est appelée `base.Render(writer)` chacun des contrôles de la page est restitué, ce qui génère le balisage en fonction de la hiérarchie des contrôles. Par conséquent, il est impératif que nous appelions `base.Render(writer)`, afin que la page soit rendue, et que nous manipulions la hiérarchie des contrôles GridView s avant d’appeler `base.Render(writer)`, afin que les lignes de séparation aient été ajoutées à la hiérarchie des contrôles GridView s avant d’être restituées.

Pour injecter les en-têtes de groupe de tri, nous devons d’abord vérifier que l’utilisateur a demandé que les données soient triées. Par défaut, le contenu de GridView s n’est pas trié et, par conséquent, nous n’avons pas besoin d’entrer d’en-têtes de tri de groupe.

> [!NOTE]
> Si vous souhaitez que le contrôle GridView soit trié par une colonne particulière lorsque la page est chargée pour la première fois, appelez la méthode GridView s `Sort` sur la première page visitée (mais pas sur les publications suivantes). Pour ce faire, ajoutez cet appel dans le gestionnaire d’événements `Page_Load` dans un `if (!Page.IsPostBack)` conditionnel. Reportez-vous aux informations de didacticiel sur la [pagination et le tri des données du rapport](paging-and-sorting-report-data-cs.md) pour plus d’informations sur la méthode `Sort`.

En supposant que les données ont été triées, la tâche suivante consiste à déterminer la colonne sur laquelle les données ont été triées, puis à analyser les lignes pour rechercher les différences dans les valeurs de ces colonnes. Le code suivant garantit que les données ont été triées et trouve la colonne selon laquelle les données ont été triées :

[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample3.cs)]

Si le GridView n’a pas encore été trié, la propriété GridView s `SortExpression` n’a pas été définie. Par conséquent, il vous suffit d’ajouter les lignes de séparation si cette propriété a une certaine valeur. Si c’est le cas, nous devons ensuite déterminer l’index de la colonne par laquelle les données ont été triées. Pour ce faire, vous devez parcourir la collection de `Columns` GridView, en recherchant la colonne dont la propriété `SortExpression` est égale à la propriété GridView s `SortExpression`. En plus de l’index de la colonne, nous récupérons également la propriété `HeaderText`, qui est utilisée lors de l’affichage des lignes de séparation.

Avec l’index de la colonne selon laquelle les données sont triées, la dernière étape consiste à énumérer les lignes du contrôle GridView. Pour chaque ligne, nous devons déterminer si la valeur s de la colonne triée est différente de la valeur s de la colonne triée précédente. Si c’est le cas, nous devons injecter une nouvelle instance de `GridViewRow` dans la hiérarchie des contrôles. Pour ce faire, vous devez utiliser le code suivant :

[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample4.cs)]

Ce code commence par programmer à la référence de l’objet `Table` qui se trouve à la racine de la hiérarchie de contrôle GridView s et à la création d’une variable de chaîne nommée `lastValue`. `lastValue` est utilisé pour comparer la valeur actuelle de la colonne triée avec la valeur s de la ligne précédente. Ensuite, la collection `Rows` GridView s est énumérée et, pour chaque ligne, la valeur de la colonne triée est stockée dans la variable `currentValue`.

> [!NOTE]
> Pour déterminer la valeur de la colonne de lignes triée en particulier, j’utilise la propriété Cell s `Text`. Cela fonctionne bien pour BoundFields, mais ne fonctionne pas comme vous le souhaitez pour TemplateFields, CheckBoxFields, etc. Nous allons voir comment prendre en compte d’autres champs GridView sous peu.

Les variables `currentValue` et `lastValue` sont ensuite comparées. Si elles diffèrent, nous devons ajouter une nouvelle ligne de séparation à la hiérarchie des contrôles. Pour ce faire, vous devez déterminer l’index de la `GridViewRow` dans la collection d’objets `Table` `Rows`, créer des instances `GridViewRow` et `TableCell`, puis ajouter le `TableCell` et le `GridViewRow` à la hiérarchie des contrôles.

Notez que la ligne de séparation s seul `TableCell` est mise en forme de sorte qu’elle s’étend sur toute la largeur du contrôle GridView, qu’elle est mise en forme à l’aide de la classe CSS `SortHeaderRowStyle` et que sa propriété `Text` affiche à la fois le nom du groupe de tri (par exemple, Category) et la valeur du groupe (par exemple, boissons). Enfin, `lastValue` est mis à jour à la valeur de `currentValue`.

La classe CSS utilisée pour mettre en forme la ligne d’en-tête de groupe de tri `SortHeaderRowStyle` doit être spécifiée dans le fichier `Styles.css`. N’hésitez pas à utiliser les paramètres de style qui vous intéressent. J’ai utilisé les éléments suivants :

[!code-css[Main](creating-a-customized-sorting-user-interface-cs/samples/sample5.css)]

Avec le code actuel, l’interface de tri ajoute des en-têtes de groupe de tri lors du tri par un BoundField (voir la figure 5, qui affiche une capture d’écran lors du tri par fournisseur). Toutefois, lors du tri par tout autre type de champ (par exemple, CheckBoxField ou TemplateField), les en-têtes de groupe de tri ne sont pas trouvés (voir figure 6).

[![l’interface de tri comprend des en-têtes de groupe de tri lors du tri par BoundFields](creating-a-customized-sorting-user-interface-cs/_static/image12.png)](creating-a-customized-sorting-user-interface-cs/_static/image11.png)

**Figure 5**: l’interface de tri comprend des en-têtes de groupe de tri lors du tri par BoundFields ([cliquez pour afficher l’image en taille réelle](creating-a-customized-sorting-user-interface-cs/_static/image13.png))

[![les en-têtes de groupe de tri sont manquants lors du tri d’un CheckBoxField](creating-a-customized-sorting-user-interface-cs/_static/image15.png)](creating-a-customized-sorting-user-interface-cs/_static/image14.png)

**Figure 6**: les en-têtes de groupe de tri sont manquants lors du tri d’un CheckBoxField ([cliquez pour afficher l’image en taille réelle](creating-a-customized-sorting-user-interface-cs/_static/image16.png))

La raison pour laquelle les en-têtes de groupe de tri sont manquants lors du tri par un CheckBoxField est que le code utilise actuellement simplement la propriété `TableCell` s `Text` pour déterminer la valeur de la colonne triée pour chaque ligne. Pour CheckBoxFields, la propriété `TableCell` s `Text` est une chaîne vide ; au lieu de cela, la valeur est disponible via un contrôle Web CheckBox qui réside dans la collection `TableCell` s `Controls`.

Pour gérer les types de champs autres que BoundFields, nous devons augmenter le code où la variable `currentValue` est assignée pour vérifier l’existence d’une case à cocher dans la collection `TableCell` s `Controls`. Au lieu d’utiliser `currentValue = gvr.Cells[sortColumnIndex].Text`, remplacez ce code par ce qui suit :

[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample6.cs)]

Ce code examine la colonne triée `TableCell` pour la ligne actuelle afin de déterminer s’il existe des contrôles dans la collection `Controls`. Si c’est le cas et que le premier contrôle est une case à cocher, la variable `currentValue` est définie sur Oui ou non, selon la propriété CheckBox s `Checked`. Dans le cas contraire, la valeur est extraite de la propriété `TableCell` s `Text`. Cette logique peut être répliquée pour gérer le tri des TemplateFields qui peuvent exister dans le GridView.

Avec l’ajout de code ci-dessus, les en-têtes de groupe de tri sont maintenant présents lors du tri par le CheckBoxField abandonné (voir la figure 7).

[![les en-têtes de groupe de tri sont maintenant présents lors du tri d’un CheckBoxField](creating-a-customized-sorting-user-interface-cs/_static/image18.png)](creating-a-customized-sorting-user-interface-cs/_static/image17.png)

**Figure 7**: les en-têtes de groupe de tri sont maintenant présents lors du tri d’un CheckBoxField ([cliquez pour afficher l’image en taille réelle](creating-a-customized-sorting-user-interface-cs/_static/image19.png))

> [!NOTE]
> Si vous avez des produits avec des valeurs de `NULL` de base de données pour les champs `CategoryID`, `SupplierID`ou `UnitPrice`, ces valeurs apparaissent sous forme de chaînes vides dans le GridView par défaut, ce qui signifie que le texte des lignes de séparation pour ces produits avec les valeurs de `NULL` est le suivant : (autrement dit, il n’existe aucun nom après la catégorie : comme avec Category : boissons). Si vous souhaitez afficher une valeur ici, vous pouvez définir la [propriété BoundFields`NullDisplayText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.nulldisplaytext.aspx) sur le texte que vous souhaitez afficher ou vous pouvez ajouter une instruction conditionnelle dans la méthode Render lors de l’affectation de la `currentValue` à la propriété `Text` de la ligne de séparation.

## <a name="summary"></a>Récapitulatif

Le contrôle GridView n’inclut pas de nombreuses options intégrées pour la personnalisation de l’interface de tri. Toutefois, avec un peu de code de bas niveau, il est possible de modifier la hiérarchie des contrôles GridView s pour créer une interface plus personnalisée. Dans ce didacticiel, nous avons vu comment ajouter une ligne de séparation de groupe de tri pour un GridView pouvant être trié, ce qui permet d’identifier plus facilement les groupes distincts et les limites de ces groupes. Pour obtenir d’autres exemples d’interfaces de tri personnalisées, consultez l’entrée de blog [conseils et astuces de tri](https://weblogs.asp.net/scottgu/archive/2006/02/11/437995.aspx) de [Scott Guthrie](https://weblogs.asp.net/scottgu/) ASP.NET 2,0 GridView.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Précédent](sorting-custom-paged-data-cs.md)
> [Suivant](paging-and-sorting-report-data-vb.md)
