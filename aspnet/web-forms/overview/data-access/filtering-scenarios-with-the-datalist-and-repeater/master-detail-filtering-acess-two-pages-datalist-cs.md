---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-cs
title: Filtrage maître/détail sur deux pages (C#) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons examiner comment séparer un rapport maître/détail sur deux pages. Dans la page principale, nous utilisons un contrôle Repeater pour afficher une liste de categ...
ms.author: riande
ms.date: 10/30/2010
ms.assetid: 68b8c023-92fa-4df6-9563-1764e16e4b04
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: 545b24a66476c55aff88ac62d3a6528105fea6c0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78607078"
---
# <a name="masterdetail-filtering-across-two-pages-c"></a>Filtrage maître/détail sur deux pages (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_34_CS.exe) ou [Télécharger le PDF](master-detail-filtering-acess-two-pages-datalist-cs/_static/datatutorial34cs1.pdf)

> Dans ce didacticiel, nous allons examiner comment séparer un rapport maître/détail sur deux pages. Dans la page « maître », nous utilisons un contrôle Repeater pour afficher une liste de catégories qui, lorsque vous cliquez dessus, remettra l’utilisateur à la page « Détails » où un contrôle DataList à deux colonnes affiche les produits appartenant à la catégorie sélectionnée.

## <a name="introduction"></a>Introduction

Dans le [didacticiel précédent](master-detail-filtering-with-a-dropdownlist-datalist-cs.md) , nous avons vu comment afficher des rapports maître/détail dans une page Web unique à l’aide de DropDownList pour afficher les enregistrements « maîtres » et un contrôle DataList pour afficher les détails. Un autre modèle courant utilisé pour les rapports maître/détail consiste à avoir les enregistrements maîtres sur une page Web et les détails sur un autre. Dans le didacticiel précédent [/filtrage des détails sur deux pages](../masterdetail/master-detail-filtering-across-two-pages-cs.md) , nous avons examiné ce modèle à l’aide d’un GridView pour afficher tous les fournisseurs dans le système. Ce GridView comprenait un HyperLinkField, qui s’affichait sous la forme d’un lien vers une deuxième page, transmettant le `SupplierID` dans la chaîne de chaîne. La deuxième page utilisait un GridView pour répertorier les produits fournis par le fournisseur sélectionné.

Ces rapports maître/détail à deux pages peuvent également être obtenus à l’aide des contrôles DataList et Repeater. La seule différence est que ni le contrôle DataList ni le Repeater n’assurent la prise en charge du contrôle HyperLinkField. Au lieu de cela, nous devons ajouter un contrôle HyperLink Web ou un élément HTML d’ancrage (`<a>`) dans le `ItemTemplate`du contrôle. La propriété `NavigateUrl` du lien hypertexte ou l’attribut `href` de l’ancre peut ensuite être personnalisé à l’aide des approches déclaratives ou par programmation.

Dans ce didacticiel, nous allons explorer un exemple qui répertorie les catégories d’une liste à puces sur une page à l’aide d’un contrôle Repeater. Chaque élément de liste inclut le nom et la description de la catégorie, le nom de catégorie étant affiché sous la forme d’un lien vers une deuxième page. Si vous cliquez sur ce lien, l’utilisateur passera à la deuxième page, où un contrôle DataList affichera les produits appartenant à la catégorie sélectionnée.

## <a name="step-1-displaying-the-categories-in-a-bulleted-list"></a>Étape 1 : affichage des catégories dans une liste à puces

La première étape de la création d’un rapport maître/détail consiste à commencer par afficher les enregistrements « maîtres ». Par conséquent, la première tâche consiste à afficher les catégories dans la page principale. Ouvrez la page `CategoryListMaster.aspx` dans le dossier `DataListRepeaterFiltering`, ajoutez un contrôle Repeater et, à partir de la balise active, choisissez d’ajouter un nouvel ObjectDataSource. Configurez le nouvel ObjectDataSource afin qu’il accède à ses données à partir de la méthode de `GetCategories` de la classe `CategoriesBLL` (voir figure 1).

[![configurer ObjectDataSource pour utiliser la méthode GetCategories de la classe CategoriesBLL](master-detail-filtering-acess-two-pages-datalist-cs/_static/image2.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image1.png)

**Figure 1**: configurer ObjectDataSource pour utiliser la méthode de `GetCategories` de la classe `CategoriesBLL` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-acess-two-pages-datalist-cs/_static/image3.png))

Ensuite, définissez les modèles du répéteur de façon à ce qu’il affiche chaque nom de catégorie et description en tant qu’élément dans une liste à puces. Ne vous inquiétez pas encore d’avoir chaque catégorie liée à la page de détails. L’exemple suivant montre le balisage déclaratif pour le Repeater et l’ObjectDataSource :

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample1.aspx)]

Une fois cette balise terminée, prenez un moment pour voir notre progression dans un navigateur. Comme le montre la figure 2, le Repeater est restitué sous la forme d’une liste à puces indiquant le nom et la description de chaque catégorie.

[![chaque catégorie est affichée sous la forme d’un élément de liste à puces](master-detail-filtering-acess-two-pages-datalist-cs/_static/image5.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image4.png)

**Figure 2**: chaque catégorie est affichée sous la forme d’un élément de liste à puces ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-acess-two-pages-datalist-cs/_static/image6.png))

## <a name="step-2-turning-the-category-name-into-a-link-to-the-details-page"></a>Étape 2 : transformation du nom de catégorie en lien vers la page de détails

Pour permettre à un utilisateur d’afficher les informations de « détails » pour une catégorie donnée, nous devons ajouter un lien vers chaque élément de liste à puces qui, lorsque vous cliquez dessus, remettra l’utilisateur à la deuxième page (`ProductsForCategoryDetails.aspx`). Cette deuxième page affiche ensuite les produits pour la catégorie sélectionnée à l’aide d’un contrôle DataList. Pour déterminer la catégorie dont l’utilisateur a cliqué sur le lien, nous devons passer le `CategoryID` de la catégorie sur laquelle vous avez cliqué sur la deuxième page via un mécanisme. La méthode la plus simple et la plus simple pour transférer des données scalaires d’une page à une autre consiste à utiliser QueryString, qui est l’option que nous allons utiliser dans ce didacticiel. En particulier, la page `ProductsForCategoryDetails.aspx` s’attend à ce que la valeur *`categoryID`* sélectionnée soit passée via un champ querystring nommé `CategoryID`. Par exemple, pour afficher les produits de la catégorie boissons, qui a une `CategoryID` de 1, un utilisateur peut visiter `ProductsForCategoryDetails.aspx?CategoryID=1`.

Pour créer un lien hypertexte pour chaque élément de liste à puces dans le répétiteur, nous devons ajouter un contrôle HyperLink Web ou un élément d’ancrage HTML (`<a>`) au `ItemTemplate`. Dans les scénarios où le lien hypertexte est affiché de la même manière pour chaque ligne, l’une ou l’autre approche suffira. Pour les répéteurs, je préfère utiliser l’élément d’ancrage. Pour utiliser l’élément d’ancrage, mettez à jour le ItemTemplate du Repeater en :

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample2.aspx)]

Notez que la `CategoryID` peut être injectée directement dans l’attribut `href` de l’élément d’ancrage ; Toutefois, pour ce faire, assurez-vous de délimiter la valeur de l’attribut `href` par des apostrophes (et des guillemets), car la méthode `Eval` dans l’attribut `href` délimite sa chaîne (`"CategoryID"`) par des guillemets. Vous pouvez également utiliser un contrôle Web HYPERLINK à la place :

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample3.aspx)]

Notez comment la partie statique de l’URL, `ProductsForCategoryDetails.aspx?CategoryID`, est ajoutée au résultat de `Eval("CategoryID")` directement dans la syntaxe DataBinding à l’aide de la concaténation de chaînes.

L’un des avantages de l’utilisation du contrôle HyperLink est qu’il est possible d’y accéder par programmation à partir du gestionnaire d’événements `ItemDataBound` du Repeater, si nécessaire. Par exemple, vous souhaiterez peut-être afficher le nom de la catégorie sous forme de texte plutôt que sous la forme d’un lien pour les catégories sans produits associés. Une telle vérification peut être effectuée par programmation dans le gestionnaire d’événements `ItemDataBound` ; pour les catégories sans produit associé, la propriété `NavigateUrl` du lien hypertexte peut être définie sur une chaîne vide, ce qui aboutit à un nom de catégorie particulier qui est rendu en texte brut (et non en tant que lien). Reportez-vous au didacticiel sur la [mise en forme des contrôles DataList et Repeater en fonction des données](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs.md) pour plus d’informations sur la mise en forme du contenu de la DataList et du répétiteur en fonction de la logique de programmation via le gestionnaire d’événements `ItemDataBound`.

Si vous procédez ainsi, n’hésitez pas à utiliser l’approche d’élément d’ancrage ou de contrôle de lien hypertexte dans votre page. Quelle que soit l’approche, lorsque vous affichez la page via un navigateur, chaque nom de catégorie doit être affiché sous la forme d’un lien vers `ProductsForCategoryDetails.aspx`, en transmettant la valeur de `CategoryID` applicable (voir figure 3).

[![les noms de catégorie sont maintenant liés à ProductsForCategoryDetails. aspx](master-detail-filtering-acess-two-pages-datalist-cs/_static/image8.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image7.png)

**Figure 3**: les noms de catégorie sont désormais liés à `ProductsForCategoryDetails.aspx` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-acess-two-pages-datalist-cs/_static/image9.png))

## <a name="step-3-listing-the-products-that-belong-to-the-selected-category"></a>Étape 3 : affichage des produits appartenant à la catégorie sélectionnée

Une fois la page `CategoryListMaster.aspx` terminée, nous sommes prêts à attirer l’attention sur l’implémentation de la page « détails », `ProductsForCategoryDetails.aspx`. Ouvrez cette page, faites glisser un contrôle DataList de la boîte à outils vers le concepteur et affectez à sa propriété `ID` la valeur `ProductsInCategory`. Ensuite, à partir de la balise active de DataList, choisissez d’ajouter un nouvel ObjectDataSource à la page, en lui attribuant un nom `ProductsInCategoryDataSource`. Configurez-le de sorte qu’il appelle la méthode `GetProductsByCategoryID(categoryID)` de la classe `ProductsBLL` ; Définissez les listes déroulantes des onglets insertion, mise à jour et suppression sur (aucune).

[![configurer ObjectDataSource pour utiliser la méthode GetProductsByCategoryID (categoryID) de la classe ProductsBLL](master-detail-filtering-acess-two-pages-datalist-cs/_static/image11.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image10.png)

**Figure 4**: configurer ObjectDataSource pour utiliser la méthode de `GetProductsByCategoryID(categoryID)` de la classe `ProductsBLL` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-acess-two-pages-datalist-cs/_static/image12.png))

Étant donné que la méthode `GetProductsByCategoryID(categoryID)` accepte un paramètre d’entrée ( *`categoryID`* ), l’assistant choisir la source de données nous offre la possibilité de spécifier la source du paramètre. Définissez la source de paramètre sur QueryString à l’aide de l' `CategoryID`QueryStringField.

[![utiliser le champ QueryString CategoryID comme source du paramètre](master-detail-filtering-acess-two-pages-datalist-cs/_static/image14.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image13.png)

**Figure 5**: utiliser le champ QueryString `CategoryID` comme source du paramètre ([Cliquer pour afficher l’image en taille réelle](master-detail-filtering-acess-two-pages-datalist-cs/_static/image15.png))

Comme nous l’avons vu dans les didacticiels précédents, à l’issue de l’exécution de l’assistant choisir la source de données, Visual Studio crée automatiquement un `ItemTemplate` pour le contrôle DataList qui répertorie chaque nom et valeur de champ de données. Remplacez ce modèle par un modèle qui répertorie uniquement le nom, le fournisseur et le prix du produit. En outre, affectez la valeur 2 à la propriété `RepeatColumns` de DataList. Une fois ces modifications effectuées, le balisage déclaratif de DataList et de ObjectDataSource doit ressembler à ce qui suit :

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample4.aspx)]

Pour afficher cette page en action, démarrez à partir de la page `CategoryListMaster.aspx` ; Ensuite, cliquez sur un lien dans la liste à puces catégories. Cela vous permettra de `ProductsForCategoryDetails.aspx`, en transmettant le `CategoryID` via QueryString. Le `ProductsInCategoryDataSource` ObjectDataSource dans `ProductsForCategoryDetails.aspx` obtiendra uniquement ces produits pour la catégorie spécifiée et les affichera dans le contrôle DataList, qui restitue deux produits par ligne. La figure 6 illustre une capture d’écran de `ProductsForCategoryDetails.aspx` lors de l’affichage des boissons.

[![les boissons sont affichées, deux par ligne](master-detail-filtering-acess-two-pages-datalist-cs/_static/image17.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image16.png)

**Figure 6**: les boissons sont affichées, deux par ligne ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-acess-two-pages-datalist-cs/_static/image18.png))

## <a name="step-4-displaying-category-information-on-productsforcategorydetailsaspx"></a>Étape 4 : affichage des informations de catégorie sur ProductsForCategoryDetails. aspx

Lorsqu’un utilisateur clique sur une catégorie dans `CategoryListMaster.aspx`, il est tenu d' `ProductsForCategoryDetails.aspx` et d’afficher les produits appartenant à la catégorie sélectionnée. Toutefois, dans `ProductsForCategoryDetails.aspx` il n’existe aucune indication visuelle quant à la catégorie sélectionnée. Un utilisateur qui voulait cliquer sur les boissons, mais sur les condiments accidentellement cliqués, n’a aucun moyen de réaliser son erreur une fois qu’il a atteint `ProductsForCategoryDetails.aspx`. Pour atténuer ce problème potentiel, nous pouvons afficher des informations sur la catégorie sélectionnée, son nom et sa description, en haut de la page `ProductsForCategoryDetails.aspx`.

Pour ce faire, ajoutez un FormView au-dessus du contrôle Repeater dans `ProductsForCategoryDetails.aspx`. Ensuite, ajoutez un nouvel ObjectDataSource à la page à partir de la balise active FormView nommée `CategoryDataSource` et configurez-le pour utiliser la méthode `GetCategoryByCategoryID(categoryID)` de la classe `CategoriesBLL`.

[![accéder aux informations sur la catégorie par le biais de la méthode GetCategoryByCategoryID (categoryID) de la classe CategoriesBLL](master-detail-filtering-acess-two-pages-datalist-cs/_static/image20.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image19.png)

**Figure 7**: accéder aux informations sur la catégorie par le biais de la méthode `GetCategoryByCategoryID(categoryID)` de la classe `CategoriesBLL` ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-acess-two-pages-datalist-cs/_static/image21.png))

Comme avec la `ProductsInCategoryDataSource` ObjectDataSource ajoutée à l’étape 3, l’Assistant Configuration de la source de données de `CategoryDataSource`vous invite à entrer une source pour le paramètre d’entrée de la méthode `GetCategoryByCategoryID(categoryID)`. Utilisez exactement les mêmes paramètres que précédemment, en définissant la source du paramètre sur QueryString et la valeur QueryStringField sur `CategoryID` (reportez-vous à la figure 5).

Une fois l’Assistant terminé, Visual Studio crée automatiquement un `ItemTemplate`, `EditItemTemplate`et `InsertItemTemplate` pour le FormView. Étant donné que nous fournissons une interface en lecture seule, n’hésitez pas à supprimer les `EditItemTemplate` et les `InsertItemTemplate`. En outre, n’hésitez pas à personnaliser le `ItemTemplate`du FormView. Après la suppression des modèles superflus et la personnalisation du modèle ItemTemplate, les balises déclaratives de FormView et de ObjectDataSource doivent ressembler à ce qui suit :

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample5.aspx)]

La figure 8 illustre une capture d’écran lors de l’affichage de cette page dans un navigateur.

> [!NOTE]
> En plus du FormView, j’ai également ajouté un contrôle de lien hypertexte au-dessus du contrôle FormView qui rétablit l’utilisateur dans la liste des catégories (`CategoryListMaster.aspx`). N’hésitez pas à placer ce lien ailleurs ou à l’omettre entièrement.

[![informations sur les catégories s’affichent désormais en haut de la page](master-detail-filtering-acess-two-pages-datalist-cs/_static/image23.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image22.png)

**Figure 8**: les informations de catégorie s’affichent désormais en haut de la page ([cliquez pour afficher l’image en taille réelle](master-detail-filtering-acess-two-pages-datalist-cs/_static/image24.png))

## <a name="step-5-displaying-a-message-if-no-products-belong-to-the-selected-category"></a>Étape 5 : affichage d’un message si aucun produit n’appartient à la catégorie sélectionnée

La page `CategoryListMaster.aspx` répertorie toutes les catégories dans le système, qu’il y ait ou non des produits associés. Si un utilisateur clique sur une catégorie pour laquelle aucun produit n’est associé, la DataList dans `ProductsForCategoryDetails.aspx` ne s’affiche pas, car sa source de données n’a pas d’élément. Comme nous l’avons vu dans les didacticiels précédents, le contrôle GridView fournit une propriété `EmptyDataText` qui peut être utilisée pour spécifier un message texte à afficher s’il n’y a aucun enregistrement dans sa source de données. Malheureusement, ni le contrôle DataList ni le Repeater n’ont une telle propriété.

Pour afficher un message informant l’utilisateur qu’il n’existe aucun produit correspondant pour la catégorie sélectionnée, nous devons ajouter un contrôle Label à la page dont la propriété `Text` est assignée au message à afficher dans le cas où il n’y a aucun produit correspondant. Nous devons ensuite définir par programmation sa propriété `Visible` selon que la DataList contient ou non des éléments.

Pour ce faire, commencez par ajouter une étiquette sous le contrôle DataList. Définissez sa propriété `ID` sur `NoProductsMessage` et sa propriété `Text` sur « il n’y a aucun produit pour la catégorie sélectionnée... » Ensuite, nous devons définir par programmation la propriété `Visible` de cette étiquette selon que des données ont été ou non liées à la DataList `ProductsInCategory`. Cette assignation doit être effectuée une fois que les données ont été liées au contrôle DataList. Pour les contrôles GridView, DetailsView et FormView, nous pourrions créer un gestionnaire d’événements pour l’événement `DataBound` du contrôle, qui se déclenche après la fin de la liaison de liaison. Toutefois, le contrôle DataList et le Repeater ne disposent pas d’un événement `DataBound`.

Pour cet exemple particulier, nous pouvons affecter la propriété `Visible` de l’étiquette dans le gestionnaire d’événements `Page_Load`, car les données ont été affectées au contrôle DataList avant l’événement `Load` de la page. Toutefois, cette approche ne fonctionne pas dans le cas général, car les données de l’ObjectDataSource peuvent être liées à la DataList plus tard dans le cycle de vie de la page. Par exemple, si les données affichées sont basées sur la valeur d’un autre contrôle, comme c’est le cas lors de l’affichage d’un rapport maître/détail à l’aide d’un contrôle DropDownList pour conserver les enregistrements « maîtres », les données risquent de ne pas être reliées au contrôle Web de données jusqu’à la phase de `PreRender` dans le cycle de vie de la page.

Une solution qui fonctionnera dans tous les cas consiste à affecter la propriété `Visible` à `False` dans le gestionnaire d’événements `ItemDataBound` (ou `ItemCreated`) de DataList lors de la liaison d’un type d’élément `Item` ou `AlternatingItem`. Dans ce cas, nous savons qu’il y a au moins un élément de données dans la source de données et que, par conséquent, vous pouvez masquer le `NoProductsMessage` étiquette. En plus de ce gestionnaire d’événements, nous avons également besoin d’un gestionnaire d’événements pour l’événement `DataBinding` de DataList, où nous initialisant la propriété `Visible` de l’étiquette sur `True`. Étant donné que l’événement `DataBinding` se déclenche avant les événements `ItemDataBound`, la propriété `Visible` de l’étiquette sera initialement définie sur `True`; en revanche, s’il existe des éléments de données, ils sont définis sur `False`. Le code suivant implémente cette logique :

[!code-csharp[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample6.cs)]

Toutes les catégories de la base de données Northwind sont associées à un ou plusieurs produits. Pour tester cette fonctionnalité, j’ai ajusté manuellement la base de données Northwind pour ce didacticiel, en réaffectant tous les produits associés à la catégorie production (`CategoryID` = 7) à la catégorie Food (`CategoryID` = 8). Pour ce faire, vous pouvez utiliser la Explorateur de serveurs en choisissant nouvelle requête et en utilisant l’instruction `UPDATE` suivante :

[!code-sql[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample7.sql)]

Après avoir mis à jour la base de données en conséquence, revenez à la page `CategoryListMaster.aspx`, puis cliquez sur le lien produire. Étant donné qu’il n’y a plus de produits appartenant à la catégorie production, vous devez voir « il n’y a aucun produit pour la catégorie sélectionnée... » message, comme illustré à la figure 9.

[![un message s’affiche s’il n’existe aucun produit appartenant à la catégorie sélectionnée](master-detail-filtering-acess-two-pages-datalist-cs/_static/image26.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image25.png)

**Figure 9**: un message s’affiche s’il n’existe aucun produit appartenant à la catégorie sélectionnée ([cliquez pour afficher l’image en plein écran](master-detail-filtering-acess-two-pages-datalist-cs/_static/image27.png))

## <a name="summary"></a>Récapitulatif

Bien que les rapports maître/détail puissent afficher à la fois les enregistrements maître et détail sur une seule page, dans de nombreux sites Web, ils sont séparés sur deux pages Web. Dans ce didacticiel, nous avons vu comment implémenter ce rapport maître/détail en faisant en sorte que les catégories soient répertoriées dans une liste à puces à l’aide d’un répéteur dans la page Web « master » et des produits associés répertoriés dans la page « Détails ». Chaque élément de liste dans la page Web principale contient un lien vers la page de détails qui a été transmise le long de la valeur `CategoryID` de la ligne.

Dans la page de détails, la récupération de ces produits pour le fournisseur spécifié a été effectuée via la méthode `GetProductsByCategoryID(categoryID)` de la classe `ProductsBLL`. La valeur du paramètre *`categoryID`* a été spécifiée de manière déclarative à l’aide de la valeur QueryString `CategoryID` en tant que source du paramètre. Nous avons également vu comment afficher les détails d’une catégorie dans la page de détails à l’aide d’un FormView et comment afficher un message s’il n’y avait aucun produit appartenant à la catégorie sélectionnée.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à...

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel étaient Zack Jones et Liz Shulok. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](master-detail-filtering-with-a-dropdownlist-datalist-cs.md)
> [Suivant](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)
