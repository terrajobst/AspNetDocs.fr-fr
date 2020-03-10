---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
title: Maître/détail à l’aide d’une liste à puces d’enregistrements principaux avec un contrôle des détails DataList (VB) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons compresser le rapport maître/détail sur deux pages du didacticiel précédent sur une seule page, en présentant une liste à puces de noms de catégories sur t...
ms.author: riande
ms.date: 10/17/2006
ms.assetid: ee20742f-6fb7-49a0-a009-058fe363aacb
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 81d72c666925e89729464e7ea696bde8d323e277
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78605958"
---
# <a name="masterdetail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb"></a>Représentation maître/détail avec une liste à puces des enregistrements maîtres avec une DataList des détails (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_35_VB.exe) ou [Télécharger le PDF](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/datatutorial35vb1.pdf)

> Dans ce didacticiel, nous allons compresser le rapport maître/détail sur deux pages du didacticiel précédent sur une seule page, en présentant une liste à puces de noms de catégories sur le côté gauche de l’écran et les produits de la catégorie sélectionnée à droite de l’écran.

## <a name="introduction"></a>Introduction

Dans le [didacticiel précédent](master-detail-filtering-acess-two-pages-datalist-vb.md) , nous avons vu comment séparer un rapport maître/détail sur deux pages. Dans la page maître, nous avons utilisé un contrôle Repeater pour afficher une liste à puces de catégories. Chaque nom de catégorie était un lien hypertexte qui, lorsque vous cliquez dessus, ferait passer l’utilisateur à la page de détails, où un contrôle DataList à deux colonnes affichait les produits appartenant à la catégorie sélectionnée.

Dans ce didacticiel, nous allons compresser le didacticiel en deux pages sur une seule page, en affichant une liste à puces de noms de catégories sur le côté gauche de l’écran avec chaque nom de catégorie affiché en tant que LinkButton. Cliquer sur l’un des noms de catégorie LinkButtons induit une publication et lie les produits de la catégorie sélectionnée à un contrôle DataList à deux colonnes à droite de l’écran. En plus d’afficher chaque nom de catégorie, le répéteur sur la gauche indique le nombre total de produits pour une catégorie donnée (voir la figure 1).

[![le nom de la catégorie s et le nombre total de produits s’affichent à gauche](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image2.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image1.png)

**Figure 1**: le nom de la catégorie et le nombre total de produits s’affichent sur la gauche ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image3.png))

## <a name="step-1-displaying-a-repeater-in-the-left-portion-of-the-screen"></a>Étape 1 : affichage d’un répéteur dans la partie gauche de l’écran

Pour ce didacticiel, nous avons besoin que la liste à puces des catégories apparaisse à gauche des produits de la catégorie sélectionnée. Le contenu d’une page Web peut être positionné à l’aide de balises de paragraphe d’éléments HTML standard, d’espaces insécables, d' `<table>` s, etc. ou par le biais de techniques de feuille de style en cascade (CSS). Tous nos didacticiels, jusqu’à présent, ont utilisé des techniques CSS pour le positionnement. Lorsque nous avons créé l’interface utilisateur de navigation dans notre page maître dans le didacticiel sur les [pages maîtres et la navigation](../introduction/master-pages-and-site-navigation-vb.md) dans les sites, nous avons utilisé un *positionnement absolu*, indiquant l’offset de pixel précis pour la liste de navigation et le contenu principal. Vous pouvez également utiliser CSS pour positionner un élément à droite ou à gauche d’un autre par le biais d’une *variable flottante*. La liste à puces des catégories peut apparaître à gauche des produits des catégories sélectionnées en flottant le répéteur à gauche du contrôle DataList.

Ouvrez la page `CategoriesAndProducts.aspx` dans le dossier `DataListRepeaterFiltering` et ajoutez à la page un répéteur et un contrôle DataList. Définissez le `ID` du répétiteur sur `Categories` et la DataList s sur `CategoryProducts`. Accédez à la vue source et placez les contrôles Repeater et DataList dans leurs propres éléments `<div>`. Autrement dit, placez d’abord le Repeater dans un élément `<div>`, puis le contrôle DataList dans son propre élément `<div>` directement après le Repeater. À ce stade, votre balise doit ressembler à ce qui suit :

[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample1.aspx)]

Pour faire flotter le répéteur à gauche du contrôle DataList, nous devons utiliser l’attribut de style CSS `float`, comme suit :

[!code-html[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample2.html)]

Le `float: left;` flotte le premier élément `<div>` à gauche du deuxième. Les paramètres `width` et `padding-right` indiquent le premier `width` de `<div>` et le degré de remplissage entre le contenu de l’élément de `<div>` et sa marge de droite. Pour plus d’informations sur les éléments flottants dans CSS, consultez [Floatutorial](http://css.maxdesign.com.au/floatutorial/).

Au lieu de spécifier le paramètre de style directement à l’aide du premier `<p>` élément s `style`, il est préférable de créer une nouvelle classe CSS dans `Styles.css` `FloatLeft`nommée :

[!code-css[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample3.css)]

Nous pouvons ensuite remplacer le `<div>` par `<div class="FloatLeft">`.

Après avoir ajouté la classe CSS et configuré le balisage dans la page `CategoriesAndProducts.aspx`, accédez au concepteur. Vous devez voir le répéteur flottant à gauche du contrôle DataList (bien que les deux à droite apparaissent sous forme de zones grises puisque nous avions encore configuré leurs sources de données ou modèles).

[![le répéteur est dissocié à gauche du contrôle DataList](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image5.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image4.png)

**Figure 2**: le répéteur est dissocié à gauche du contrôle DataList ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image6.png))

## <a name="step-2-determining-the-number-of-products-for-each-category"></a>Étape 2 : détermination du nombre de produits pour chaque catégorie

Une fois les balises Repeater et DataList entourant le balisage terminé, nous sommes prêts à lier les données de catégorie au contrôle Repeater. Toutefois, comme le montre la liste à puces de catégories dans la figure 1, en plus de chaque nom de catégorie, nous devons également afficher le nombre de produits associés à la catégorie. Pour accéder à ces informations, nous pouvons effectuer l’une des opérations suivantes :

- **Déterminez ces informations à partir de la classe code-behind de la page ASP.NET.** À partir d’un *`categoryID`* particulier, nous pouvons déterminer le nombre de produits associés en appelant la méthode `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)`. Cette méthode retourne un objet `ProductsDataTable` dont la propriété `Count` indique le nombre de `ProductsRow` s existe, qui est le nombre de produits pour le *`categoryID`* spécifié. Nous pouvons créer un gestionnaire d’événements `ItemDataBound` pour le répéteur qui, pour chaque catégorie liée au Repeater, appelle la méthode `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` et comprend son nombre dans la sortie.
- **Mettez à jour le `CategoriesDataTable` dans le DataSet typé pour inclure une colonne `NumberOfProducts`.** Nous pouvons ensuite mettre à jour la méthode `GetCategories()` dans le `CategoriesDataTable` afin d’inclure ces informations ou, si vous le souhaitez, conserver les `GetCategories()` en l’absence et créer une nouvelle méthode `CategoriesDataTable` appelée `GetCategoriesAndNumberOfProducts()`.

Nous allons explorer ces deux techniques. La première approche est plus simple à implémenter, car nous n’avons pas besoin de mettre à jour la couche d’accès aux données ; Toutefois, elle nécessite davantage de communications avec la base de données. L’appel à la méthode `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` dans le gestionnaire d’événements `ItemDataBound` ajoute un appel de base de données supplémentaire pour chaque catégorie affichée dans le Repeater. Avec cette technique, il existe *n* + 1 appels de base de données, où *n* est le nombre de catégories affichées dans le répéteur. Avec la deuxième approche, le nombre de produits est retourné avec des informations sur chaque catégorie à partir de la méthode `CategoriesBLL` Class s `GetCategories()` (ou `GetCategoriesAndNumberOfProducts()`), entraînant ainsi un seul voyage à la base de données.

## <a name="determining-the-number-of-products-in-the-itemdatabound-event-handler"></a>Détermination du nombre de produits dans le gestionnaire d’événements ItemDataBound

La détermination du nombre de produits pour chaque catégorie dans le gestionnaire d’événements Repeater s `ItemDataBound` ne nécessite aucune modification de la couche d’accès aux données existante. Toutes les modifications peuvent être apportées directement dans la page de `CategoriesAndProducts.aspx`. Commencez par ajouter un nouvel ObjectDataSource nommé `CategoriesDataSource` via la balise active de Repeater s. Ensuite, configurez le `CategoriesDataSource` ObjectDataSource afin qu’il récupère ses données à partir de la méthode de la classe `CategoriesBLL` s `GetCategories()`.

[![configurer ObjectDataSource pour utiliser la méthode CategoriesBLL de la classe s GetCategories ()](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image8.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image7.png)

**Figure 3**: configurer ObjectDataSource pour utiliser la méthode `CategoriesBLL` classe s `GetCategories()` ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image9.png))

Chaque élément du répéteur `Categories` doit être cliquable et, lorsque vous cliquez dessus, l' `CategoryProducts` DataList affiche ces produits pour la catégorie sélectionnée. Pour ce faire, vous pouvez faire en sorte que chaque catégorie soit un lien hypertexte, en reconnectant à cette même page (`CategoriesAndProducts.aspx`), mais en passant le `CategoryID` via la chaîne de chaîne, comme nous l’avons vu dans le didacticiel précédent. L’avantage de cette approche est qu’une page affichant des produits de catégorie particulière peut être marquée et indexée par un moteur de recherche.

Vous pouvez également faire de chaque catégorie un LinkButton, qui est l’approche que nous allons utiliser pour ce didacticiel. Le LinkButton est rendu dans le navigateur de l’utilisateur en tant que lien hypertexte, mais, lorsque vous cliquez dessus, une publication (postback) est générée. lors de la publication (postback), les objets ObjectDataSource de DataList doivent être actualisés pour afficher les produits appartenant à la catégorie sélectionnée. Pour ce didacticiel, l’utilisation d’un lien hypertexte est plus logique que l’utilisation d’un LinkButton. Toutefois, il peut y avoir d’autres scénarios dans lesquels l’utilisation d’un LinkButton est plus avantageuse. Bien que l’approche de lien hypertexte soit idéale pour cet exemple, faites-le plutôt à l’aide du LinkButton. Comme nous le verrons, l’utilisation d’un LinkButton présente des défis qui ne se présenteront pas avec un lien hypertexte. Par conséquent, l’utilisation d’un LinkButton dans ce didacticiel met en évidence ces défis et permet de fournir des solutions aux scénarios où nous pouvons utiliser un LinkButton au lieu d’un lien hypertexte.

> [!NOTE]
> Il est recommandé de répéter ce didacticiel à l’aide d’un contrôle HyperLink ou `<a>` élément à la place du LinkButton.

Le balisage suivant montre la syntaxe déclarative pour le Repeater et l’ObjectDataSource. Notez que les modèles Repeater s affichent une liste à puces avec chaque élément comme LinkButton :

[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample4.aspx)]

> [!NOTE]
> Pour ce didacticiel, l’état d’affichage du répétiteur doit être activé (Notez l’omission de la `EnableViewState="False"` de la syntaxe déclarative de Repeater). À l’étape 3, nous allons créer un gestionnaire d’événements pour l’événement Repeater s `ItemCommand` dans lequel nous allons mettre à jour la collection de `SelectParameters` DataList s ObjectDataSource. Toutefois, le `ItemCommand`Repeater ne se déclenchera pas si l’état d’affichage est désactivé. Pour plus d’informations sur la raison pour laquelle l’état d’affichage doit être activé pour l’activation d’un événement `ItemCommand` Repeater, consultez un plus grand nombre [d’une Question ASP.net](http://scottonwriting.net/sowblog/posts/1263.aspx) et [sa solution](http://scottonwriting.net/sowBlog/posts/1268.aspx) .

La propriété `Text` du LinkButton avec la valeur de propriété `ID` de `ViewCategory` n’est pas définie. Si nous voulions simplement afficher le nom de la catégorie, nous aurions défini la propriété Text de façon déclarative, par le biais de la syntaxe DataBinding, comme suit :

[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample5.aspx)]

Toutefois, nous souhaitons afficher le nom de la catégorie *et* le nombre de produits appartenant à cette catégorie. Ces informations peuvent être récupérées à partir du gestionnaire d’événements Repeater s `ItemDataBound` en effectuant un appel à la méthode `ProductBLL` classe s `GetCategoriesByProductID(categoryID)` et en déterminant le nombre d’enregistrements retournés dans le `ProductsDataTable`résultant, comme l’illustre le code suivant :

[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample6.vb)]

Nous commençons par nous assurer que nous travaillons à l’aide d’un élément de données (dont le `ItemType` est `Item` ou `AlternatingItem`), puis à référencer l’instance de `CategoriesRow` qui vient d’être liée à la `RepeaterItem`actuelle. Ensuite, nous déterminons le nombre de produits pour cette catégorie en créant une instance de la classe `ProductsBLL`, en appelant sa méthode `GetCategoriesByProductID(categoryID)` et en déterminant le nombre d’enregistrements retournés à l’aide de la propriété `Count`. Enfin, le `ViewCategory` LinkButton du ItemTemplate est References et sa propriété `Text` est définie sur *CategoryName* (*NumberOfProductsInCategory*), où *NumberOfProductsInCategory* est mis en forme sous la forme d’un nombre avec zéro décimal.

> [!NOTE]
> Vous pouvez également ajouter une *fonction de mise en forme* à la classe code-behind de la page ASP.net qui accepte une catégorie s `CategoryName` et `CategoryID` valeurs et retourne le `CategoryName` concaténé avec le nombre de produits dans la catégorie (comme déterminé par l’appel de la méthode `GetCategoriesByProductID(categoryID)`). Les résultats d’une fonction de mise en forme de ce type peuvent être affectés de façon déclarative à la propriété Text du texte LinkButton, remplaçant ainsi la nécessité du gestionnaire d’événements `ItemDataBound`. Pour plus d’informations sur l’utilisation des fonctions de mise en forme, reportez-vous à la rubrique [utilisation de TemplateFields dans le contrôle GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) ou [mise en forme des contrôles DataList et Repeater en fonction](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb.md) des didacticiels de données.

Après avoir ajouté ce gestionnaire d’événements, prenez un moment pour tester la page via un navigateur. Notez la manière dont chaque catégorie est répertoriée dans une liste à puces, en affichant le nom de la catégorie et le nombre de produits associés à la catégorie (voir figure 4).

[![chaque nom de catégorie et le nombre de produits affichés](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image11.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image10.png)

**Figure 4**: affichage de chaque nom de catégorie et nombre de produits ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image12.png))

## <a name="updating-thecategoriesdatatableandcategoriestableadapterto-include-the-number-of-products-for-each-category"></a>Mise à jour des`CategoriesDataTable`et des`CategoriesTableAdapter`pour inclure le nombre de produits pour chaque catégorie

Au lieu de déterminer le nombre de produits pour chaque catégorie au fur et à mesure de leur liaison au répétiteur, nous pouvons simplifier ce processus en ajustant le `CategoriesDataTable` et `CategoriesTableAdapter` dans la couche d’accès aux données pour inclure ces informations en mode natif. Pour ce faire, nous devons ajouter une nouvelle colonne à `CategoriesDataTable` pour contenir le nombre de produits associés. Pour ajouter une nouvelle colonne à un DataTable, ouvrez le DataSet typé (`App_Code\DAL\Northwind.xsd`), cliquez avec le bouton droit sur le DataTable à modifier, puis choisissez Ajouter/colonne. Ajoutez une nouvelle colonne au `CategoriesDataTable` (voir figure 5).

[![ajouter une nouvelle colonne au CategoriesDataSource](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image14.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image13.png)

**Figure 5**: ajouter une nouvelle colonne au `CategoriesDataSource` ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image15.png))

Cette opération ajoute une nouvelle colonne nommée `Column1`, que vous pouvez modifier en tapant simplement un nom différent. Renommez cette nouvelle colonne `NumberOfProducts`. Ensuite, nous devons configurer les propriétés de cette colonne. Cliquez sur la nouvelle colonne et accédez à la Fenêtre Propriétés. Modifiez la propriété de la colonne s `DataType` de `System.String` en `System.Int32` et affectez à la propriété `ReadOnly` la valeur `True`, comme illustré à la figure 6.

![Définir le type de données et les propriétés ReadOnly de la nouvelle colonne](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image16.png)

**Figure 6**: définir les propriétés `DataType` et `ReadOnly` de la nouvelle colonne

Alors que la `CategoriesDataTable` a maintenant une colonne `NumberOfProducts`, sa valeur n’est pas définie par les requêtes du TableAdapter correspondantes. Nous pouvons mettre à jour la méthode `GetCategories()` pour retourner ces informations si vous souhaitez que ces informations soient renvoyées chaque fois que les informations de catégorie sont récupérées. Toutefois, si nous devons uniquement saisir le nombre de produits associés pour les catégories dans de rares cas (par exemple uniquement pour ce didacticiel), nous pouvons conserver les `GetCategories()` en l’occurrence et créer une nouvelle méthode qui retourne ces informations. Essayons d’utiliser cette dernière approche, en créant une nouvelle méthode nommée `GetCategoriesAndNumberOfProducts()`.

Pour ajouter cette nouvelle méthode de `GetCategoriesAndNumberOfProducts()`, cliquez avec le bouton droit sur le `CategoriesTableAdapter` et choisissez nouvelle requête. L’Assistant Configuration de requêtes TableAdapter, que nous avons utilisé plusieurs fois dans les didacticiels précédents, s’affiche. Pour cette méthode, démarrez l’Assistant en indiquant que la requête utilise une instruction SQL ad hoc qui retourne des lignes.

[![créer la méthode à l’aide d’une instruction SQL ad hoc](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image18.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image17.png)

**Figure 7**: créer la méthode à l’aide d’une instruction SQL ad hoc ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image19.png))

[![l’instruction SQL retourne des lignes](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image21.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image20.png)

**Figure 8**: l’instruction SQL retourne des lignes ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image22.png))

L’écran suivant de l’Assistant nous invite à utiliser la requête. Pour renvoyer chaque `CategoryID`de catégorie, `CategoryName`et `Description` champs, ainsi que le nombre de produits associés à la catégorie, utilisez l’instruction `SELECT` suivante :

[!code-sql[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample7.sql)]

[![spécifier la requête à utiliser](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image24.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image23.png)

**Figure 9**: spécifier la requête à utiliser ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image25.png))

Notez que la sous-requête qui calcule le nombre de produits associés à la catégorie est associée à un alias `NumberOfProducts`. Cette correspondance de noms entraîne l’Association de la valeur retournée par cette sous-requête à la colonne `CategoriesDataTable` s `NumberOfProducts`.

Après avoir entré cette requête, la dernière étape consiste à choisir le nom de la nouvelle méthode. Utilisez `FillWithNumberOfProducts` et `GetCategoriesAndNumberOfProducts` pour les modèles remplir un DataTable et retourner un DataTable, respectivement.

[![nommez les nouvelles méthodes TableAdapter s FillWithNumberOfProducts et GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image27.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image26.png)

**Figure 10**: nommer les nouvelles méthodes de TableAdapter s `FillWithNumberOfProducts` et `GetCategoriesAndNumberOfProducts` ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image28.png))

À ce stade, la couche d’accès aux données a été étendue pour inclure le nombre de produits par catégorie. Dans la mesure où la couche de présentation achemine tous les appels à la couche DAL via une couche de logique métier distincte, nous devons ajouter une méthode de `GetCategoriesAndNumberOfProducts` correspondante à la classe `CategoriesBLL` :

[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample8.vb)]

Une fois la couche DAL et la couche BLL terminées, nous sommes prêts à lier ces données au répéteur `Categories` dans `CategoriesAndProducts.aspx`! Si vous avez déjà créé un ObjectDataSource pour le Repeater à partir de la section détermination du nombre de produits dans la `ItemDataBound` gestionnaire d’événements, supprimez cet ObjectDataSource et supprimez le paramètre de propriété Repeater s `DataSourceID` ; Déconnectez également l’événement Repeater s `ItemDataBound` du gestionnaire d’événements en supprimant la syntaxe `Handles Categories.OnItemDataBound` dans la classe code-behind ASP.NET.

Une fois le répétiteur rétabli dans son état d’origine, ajoutez un nouvel ObjectDataSource nommé `CategoriesDataSource` via la balise active de Repeater s. Configurez l’ObjectDataSource pour utiliser la classe `CategoriesBLL`, mais au lieu d’utiliser la méthode `GetCategories()`, utilisez `GetCategoriesAndNumberOfProducts()` à la place (voir figure 11).

[![configurer ObjectDataSource pour utiliser la méthode GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image30.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image29.png)

**Figure 11**: configurer ObjectDataSource pour utiliser la méthode `GetCategoriesAndNumberOfProducts` ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image31.png))

Ensuite, mettez à jour le `ItemTemplate` afin que la propriété LinkButton s `Text` soit assignée de manière déclarative à l’aide de la syntaxe DataBinding et inclue à la fois les champs de données `CategoryName` et `NumberOfProducts`. Le balisage déclaratif complet pour le Repeater et le `CategoriesDataSource` ObjectDataSource sont les suivants :

[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample9.aspx)]

La sortie rendue par la mise à jour de la couche DAL pour inclure une colonne `NumberOfProducts` est identique à l’utilisation de la `ItemDataBound` approche du gestionnaire d’événements (reportez-vous à la figure 4 pour voir une capture d’écran du répéteur montrant les noms de catégorie et le nombre de produits).

## <a name="step-3-displaying-the-selected-category-s-products"></a>Étape 3 : affichage des produits de la catégorie sélectionnée

À ce stade, nous avons le `Categories` répéteur affichant la liste des catégories, ainsi que le nombre de produits dans chaque catégorie. Le Repeater utilise un LinkButton pour chaque catégorie qui, lorsque vous cliquez dessus, provoque une publication (postback). à ce moment-là, nous devons afficher ces produits pour la catégorie sélectionnée dans le contrôle DataList `CategoryProducts`.

Nous nous contenterons de faire en sorte que la DataList affiche uniquement ces produits pour la catégorie sélectionnée. Dans le [maître/détail utilisant un GridView maître sélectionnable avec un didacticiel Details DetailsView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) , nous avons vu comment créer un contrôle GridView dont les lignes pouvaient être sélectionnées, avec les détails de la ligne sélectionnée affichés dans un DetailsView sur la même page. L’ObjectDataSource s de GridView a renvoyé des informations sur tous les produits à l’aide de la méthode `ProductsBLL` s `GetProducts()` alors que le contrôle DetailsView s a récupéré des informations sur le produit sélectionné à l’aide de la méthode `GetProductsByProductID(productID)`. La valeur du paramètre *`productID`* a été fournie de façon déclarative en l’associant à la valeur de la propriété GridView s `SelectedValue`. Malheureusement, le Repeater n’a pas de propriété `SelectedValue` et ne peut pas servir de source de paramètres.

> [!NOTE]
> C’est l’un des défis qui s’affichent lors de l’utilisation du LinkButton dans un Repeater. Nous avons utilisé un lien hypertexte pour passer le `CategoryID` via la chaîne de chaîne à la place, nous pourrions utiliser ce champ QueryString comme source pour la valeur du paramètre s.

Avant de vous soucier de l’absence d’une propriété de `SelectedValue` pour le Repeater, vous devez d’abord lier le contrôle DataList à un ObjectDataSource et spécifier son `ItemTemplate`.

À partir de la balise active de DataList s, choisissez d’ajouter un nouvel ObjectDataSource nommé `CategoryProductsDataSource` et configurez-le pour qu’il utilise la méthode `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)`. Étant donné que le contrôle DataList de ce didacticiel offre une interface en lecture seule, n’hésitez pas à définir les listes déroulantes dans les onglets insertion, mise à jour et suppression sur (aucune).

[![configurer ObjectDataSource pour utiliser la méthode ProductsBLL Class s GetProductsByCategoryID (categoryID)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image33.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image32.png)

**Figure 12**: configurer ObjectDataSource pour utiliser `ProductsBLL` classe s `GetProductsByCategoryID(categoryID)` méthode ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image34.png))

Étant donné que la méthode `GetProductsByCategoryID(categoryID)` attend un paramètre d’entrée ( *`categoryID`* ), l’Assistant Configuration de la source de données nous permet de spécifier la source du paramètre. Si les catégories étaient répertoriées dans un GridView ou un contrôle DataList, nous allons définir la liste déroulante source du paramètre sur Control et ControlID sur la `ID` du contrôle Web Data. Toutefois, étant donné que le Repeater n’a pas de propriété `SelectedValue` il ne peut pas être utilisé comme source de paramètre. Si vous activez la case à cocher, vous constaterez que la liste déroulante ControlID ne contient qu’une seule `ID``CategoryProducts`de contrôle, la `ID` du contrôle DataList.

Pour le moment, définissez la liste déroulante source du paramètre sur aucun. Nous allons terminer par programmation la valeur de ce paramètre lorsque l’utilisateur clique sur un LinkButton de catégorie dans le répéteur.

[![ne spécifiez pas de source de paramètre pour le paramètre categoryID](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image36.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image35.png)

**Figure 13**: ne pas spécifier de source de paramètre pour le paramètre *`categoryID`* ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image37.png))

À l’issue de l’exécution de l’Assistant Configuration de la source de données, Visual Studio génère automatiquement les `ItemTemplate`DataList. Remplacez ce `ItemTemplate` par défaut par le modèle que nous avons utilisé dans le didacticiel précédent. en outre, affectez la valeur 2 à la propriété DataList s `RepeatColumns`. Après avoir apporté ces modifications, le balisage déclaratif pour votre contrôle DataList et son ObjectDataSource associé doivent ressembler à ce qui suit :

[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample10.aspx)]

Actuellement, le paramètre `CategoryProductsDataSource` ObjectDataSource s *`categoryID`* n’est jamais défini. aucun produit n’est donc affiché lors de l’affichage de la page. Ce que nous devons faire, c’est que cette valeur de paramètre est définie en fonction de la `CategoryID` de la catégorie sur laquelle vous avez cliqué dans le répéteur. Cela présente deux défis : tout d’abord, comment déterminer quand un LinkButton dans le `ItemTemplate` Repeater a été cliqué ; Deuxièmement, comment pouvons-nous déterminer la `CategoryID` de la catégorie correspondante dont le lien a été cliqué ?

Le LinkButton comme les contrôles Button et ImageButton a un événement `Click` et un [événement`Command`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.command.aspx). L’événement `Click` est conçu pour noter simplement que l’utilisateur a cliqué sur le LinkButton. Toutefois, parfois, en plus de noter que le LinkButton a été cliqué, nous devons également transmettre des informations supplémentaires au gestionnaire d’événements. Si c’est le cas, les propriétés LinkButton s [`CommandName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandname.aspx) et [`CommandArgument`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandargument.aspx) peuvent recevoir ces informations supplémentaires. Ensuite, lorsque l’utilisateur clique sur le LinkButton, son événement `Command` se déclenche (au lieu de son événement `Click`) et le gestionnaire d’événements reçoit les valeurs des propriétés `CommandName` et `CommandArgument`.

Lorsqu’un événement `Command` est déclenché à partir d’un modèle dans le Repeater, l’événement Repeater s [`ItemCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeater.itemcommand.aspx) se déclenche et reçoit les valeurs `CommandName` et `CommandArgument` du LinkButton cliqué (ou Button ou ImageButton). Par conséquent, pour déterminer quand un clic a été effectué sur une catégorie LinkButton dans le répétiteur, nous devons effectuer les opérations suivantes :

1. Définissez la propriété `CommandName` du LinkButton dans le `ItemTemplate` Repeater sur une valeur (j’ai utilisé ListProducts). En définissant cette `CommandName` valeur, l’événement LinkButton s `Command` se déclenche lorsque l’utilisateur clique sur le LinkButton.
2. Affectez à la propriété LinkButton s `CommandArgument` la valeur de l’élément actif s `CategoryID`.
3. Créez un gestionnaire d’événements pour l’événement Repeater s `ItemCommand`. Dans le gestionnaire d’événements, définissez le paramètre `CategoryProductsDataSource` ObjectDataSource s `CategoryID` sur la valeur de la `CommandArgument`passée.

Le balisage de `ItemTemplate` suivant pour les catégories Repeater implémente les étapes 1 et 2. Notez la manière dont la valeur de `CommandArgument` est assignée à l’élément de données `CategoryID` à l’aide de la syntaxe DataBinding :

[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample11.aspx)]

Chaque fois que vous créez un gestionnaire d’événements `ItemCommand`, il est prudent de toujours vérifier la valeur de `CommandName` entrante, car *tout* événement `Command` déclenché par *un* bouton, LinkButton ou ImageButton dans le Repeater entraîne le déclenchement de l’événement `ItemCommand`. Bien que nous n’ayons actuellement qu’un tel LinkButton, à l’avenir, nous (ou un autre développeur de notre équipe) pouvez ajouter des contrôles Web de bouton supplémentaires au répéteur qui, lorsque vous cliquez dessus, déclenchent le même `ItemCommand` gestionnaire d’événements. Par conséquent, il est préférable de toujours vérifier la propriété `CommandName` et de continuer uniquement avec votre logique de programmation si elle correspond à la valeur attendue.

Après vous être assuré que la valeur `CommandName` transmise est égale à ListProducts, le gestionnaire d’événements affecte ensuite le paramètre `CategoryID` `CategoryProductsDataSource` ObjectDataSource s à la valeur de l' `CommandArgument`passé. Cette modification de la `SelectParameters` ObjectDataSource s entraîne automatiquement la liaison de la DataList à la source de données, en présentant les produits de la catégorie nouvellement sélectionnée.

[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample12.vb)]

Grâce à ces ajouts, notre didacticiel est terminé. Prenez un moment pour le tester dans un navigateur. La figure 14 affiche l’écran lors de la première visite de la page. Comme une catégorie n’a pas encore été sélectionnée, aucun produit n’est affiché. En cliquant sur une catégorie, par exemple produire, vous affichez les produits de la catégorie Product dans une vue à deux colonnes (voir figure 15).

[![aucun produit n’est affiché lors de la première visite de la page](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image39.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image38.png)

**Figure 14**: aucun produit n’est affiché lors de la première visite de la page ([cliquez pour afficher l’image en taille réelle](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image40.png))

[![cliquant sur la catégorie production répertorie les produits correspondants à droite](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image42.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image41.png)

**Figure 15**: cliquer sur la catégorie production répertorie les produits correspondants à droite ([cliquez pour afficher l’image en plein écran](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image43.png))

## <a name="summary"></a>Récapitulatif

Comme nous l’avons vu dans ce didacticiel et dans le précédent, les rapports maître/détail peuvent être répartis sur deux pages ou être consolidés sur un. Toutefois, l’affichage d’un rapport maître/détails sur une seule page présente des difficultés à mettre en page les enregistrements maître et détails sur la page. Dans le *maître/détail utilisant un GridView maître sélectionnable avec un didacticiel Details DetailsView* , les enregistrements de détails s’affichent au-dessus des enregistrements principaux. dans ce didacticiel, nous avons utilisé des techniques CSS pour faire flotter les enregistrements maîtres à gauche des détails.

En plus de l’affichage des rapports maître/détails, nous avons également eu l’opportunité d’explorer comment récupérer le nombre de produits associés à chaque catégorie, ainsi que la façon d’effectuer une logique côté serveur quand un bouton LinkButton (ou un bouton ou un ImageButton) est cliqué dans un répéteur.

Ce didacticiel termine notre examen des rapports maître/détail avec les contrôles DataList et Repeater. Notre prochain ensemble de didacticiels illustre comment ajouter des fonctionnalités de modification et de suppression au contrôle DataList.

Bonne programmation !

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Floatutorial](http://css.maxdesign.com.au/floatutorial/) un didacticiel sur les éléments CSS flottants avec CSS
- [CSS positionnant](http://www.brainjar.com/css/positioning/) plus d’informations sur le positionnement des éléments avec CSS
- [Disposition de contenu avec du code html](http://www.w3schools.com/html/html_layout.asp) à l’aide de `<table>` s et d’autres éléments HTML pour le positionnement

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel était Zack Jones. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](master-detail-filtering-acess-two-pages-datalist-vb.md)
