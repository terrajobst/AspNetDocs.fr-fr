---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs
title: Affichage des données avec les contrôles DataList et Repeater (C#) | Microsoft Docs
author: rick-anderson
description: Dans les didacticiels précédents, nous avons utilisé le contrôle GridView pour afficher les données. À partir de ce didacticiel, nous examinons la création de modèles de rapports communs avec...
ms.author: riande
ms.date: 09/13/2006
ms.assetid: 0591cacc-b34b-4cf6-885e-2c9953bb0946
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 09d3faf811f21a66bb5c234f71d77b2552ae6516
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78611859"
---
# <a name="displaying-data-with-the-datalist-and-repeater-controls-c"></a>Affichage de données avec les contrôles DataList et Repeater (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_29_CS.exe) ou [Télécharger le PDF](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/datatutorial29cs1.pdf)

> Dans les didacticiels précédents, nous avons utilisé le contrôle GridView pour afficher les données. À partir de ce didacticiel, nous examinons la création de modèles de rapports communs avec les contrôles DataList et Repeater, en commençant par les principes de base de l’affichage des données avec ces contrôles.

## <a name="introduction"></a>Introduction

Dans tous les exemples au cours des 28 derniers didacticiels, si nous avions besoin d’afficher plusieurs enregistrements d’une source de données, nous avons converti le contrôle GridView. Le GridView restitue une ligne pour chaque enregistrement dans la source de données, en affichant les champs de données de l’enregistrement dans les colonnes. Tandis que le contrôle GridView permet d’afficher, de parcourir, de trier, de modifier et de supprimer des données, son apparence est un peu boxy. En outre, le balisage responsable de la structure de GridView s est résolu, il comprend un `<table>` HTML avec une ligne de table (`<tr>`) pour chaque enregistrement et une cellule de table (`<td>`) pour chaque champ.

Pour fournir un degré de personnalisation plus élevé dans l’apparence et le balisage rendu lors de l’affichage de plusieurs enregistrements, ASP.NET 2,0 offre les contrôles DataList et Repeater (également disponibles dans ASP.NET version 1. x). Les contrôles DataList et Repeater affichent leur contenu à l’aide de modèles plutôt que BoundFields, CheckBoxFields, ButtonFields, etc. Comme le contrôle GridView, le contrôle DataList est rendu sous forme de `<table>`HTML, mais permet l’affichage de plusieurs enregistrements de source de données par ligne de table. En revanche, le Repeater n’affiche aucun balisage supplémentaire que celui que vous spécifiez explicitement et est un candidat idéal lorsque vous avez besoin d’un contrôle précis sur le balisage émis.

Au cours des douze prochains didacticiels, nous examinerons la création de modèles de rapports courants avec les contrôles DataList et Repeater, en commençant par les principes de base de l’affichage des données avec ces modèles de contrôles. Nous verrons comment mettre en forme ces contrôles, comment modifier la disposition des enregistrements de la source de données dans les scénarios DataList, maître/détails courants, comment modifier et supprimer des données, comment paginer des enregistrements, et ainsi de suite.

## <a name="step-1-adding-the-datalist-and-repeater-tutorial-web-pages"></a>Étape 1 : ajout des pages Web du didacticiel DataList et Repeater

Avant de commencer ce didacticiel, nous allons tout d’abord prendre un moment pour ajouter les pages ASP.NET dont nous aurons besoin pour ce didacticiel et les didacticiels suivants traitant de l’affichage des données à l’aide du contrôle DataList et du Repeater. Commencez par créer un nouveau dossier dans le projet nommé `DataListRepeaterBasics`. Ensuite, ajoutez les cinq pages ASP.NET suivantes à ce dossier, en faisant en sorte qu’elles soient toutes configurées pour utiliser la page maître `Site.master`:

- `Default.aspx`
- `Basics.aspx`
- `Formatting.aspx`
- `RepeatColumnAndDirection.aspx`
- `NestedControls.aspx`

![Créer un dossier DataListRepeaterBasics et ajouter les pages du didacticiel ASP.NET](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image1.png)

**Figure 1**: créer un dossier `DataListRepeaterBasics` et ajouter les pages du didacticiel ASP.net

Ouvrez la page `Default.aspx`, puis faites glisser le contrôle utilisateur `SectionLevelTutorialListing.ascx` à partir du dossier `UserControls` sur l’aire de conception. Ce contrôle utilisateur, que nous avons créé dans le didacticiel sur les [pages maîtres et la navigation](../introduction/master-pages-and-site-navigation-cs.md) dans les sites, énumère le plan du site et affiche les didacticiels de la section actuelle dans une liste à puces.

[![ajouter le contrôle utilisateur SectionLevelTutorialListing. ascx à default. aspx](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image3.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image2.png)

**Figure 2**: ajouter le contrôle utilisateur `SectionLevelTutorialListing.ascx` à `Default.aspx` ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image4.png))

Pour que la liste à puces affiche les didacticiels DataList et Repeater que nous allons créer, vous devez les ajouter au plan du site. Ouvrez le fichier `Web.sitemap` et ajoutez le balisage suivant après la balise de nœud de plan de site ajout de boutons personnalisés :

[!code-xml[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample1.xml)]

![Mettre à jour le plan du site pour inclure les nouvelles pages ASP.NET](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image5.png)

**Figure 3**: mettre à jour le plan du site pour inclure les nouvelles pages ASP.net

## <a name="step-2-displaying-product-information-with-the-datalist"></a>Étape 2 : affichage des informations sur le produit avec DataList

À l’instar du FormView, le résultat du rendu du contrôle DataList dépend des modèles plutôt que BoundFields, CheckBoxFields, etc. Contrairement au FormView, le contrôle DataList est conçu pour afficher un ensemble d’enregistrements plutôt qu’un solitaires. Commençons ce didacticiel en examinant les informations sur les produits de liaison à un contrôle DataList. Commencez par ouvrir la page `Basics.aspx` dans le dossier `DataListRepeaterBasics`. Ensuite, faites glisser un contrôle DataList de la boîte à outils vers le concepteur. Comme l’illustre la figure 4, avant de spécifier les modèles DataList, le concepteur l’affiche sous forme de zone grise.

[![faire glisser le contrôle DataList de la boîte à outils vers le concepteur](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image7.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image6.png)

**Figure 4**: faites glisser le contrôle DataList de la boîte à outils vers le concepteur ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image8.png))

À partir de la balise active de DataList s, ajoutez un nouvel ObjectDataSource et configurez-le pour qu’il utilise la méthode `ProductsBLL` classe s `GetProducts`. Étant donné que nous avons recréé un contrôle DataList en lecture seule dans ce didacticiel, définissez la liste déroulante sur (aucun) dans les onglets insertion, mise à jour et suppression de l’Assistant.

[![choisir de créer un ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image10.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image9.png)

**Figure 5**: choisir de créer un ObjectDataSource ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image11.png))

[![configurer ObjectDataSource pour utiliser la classe ProductsBLL](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image13.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image12.png)

**Figure 6**: configurer ObjectDataSource pour utiliser la classe `ProductsBLL` ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image14.png))

[![récupérer des informations sur tous les produits à l’aide de la méthode GetProducts](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image16.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image15.png)

**Figure 7**: récupérer des informations sur tous les produits à l’aide de la méthode `GetProducts` ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image17.png))

Après avoir configuré l’ObjectDataSource et l’avoir associé au contrôle DataList par le biais de sa balise active, Visual Studio crée automatiquement un `ItemTemplate` dans le contrôle DataList qui affiche le nom et la valeur de chaque champ de données retourné par la source de données (voir le balisage ci-dessous). Cette apparence `ItemTemplate` s par défaut est identique à celle des modèles créés automatiquement lors de la liaison d’une source de données au FormView par le biais du concepteur.

[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample2.aspx)]

> [!NOTE]
> Rappelez-vous que lors de la liaison d’une source de données à un contrôle FormView via la balise active FormView s, Visual Studio a créé un `ItemTemplate`, `InsertItemTemplate`et `EditItemTemplate`. Avec la DataList, toutefois, seul un `ItemTemplate` est créé. Cela est dû au fait que la DataList n’a pas la même prise en charge de modification et d’insertion intégrée que le FormView. La DataList contient des événements liés à la modification et à la suppression, et la prise en charge de l’édition et de la suppression peut être ajoutée avec un peu de code, mais il n’y a pas de prise en charge prête à l’emploi simple comme avec le FormView. Nous verrons comment inclure la prise en charge de la modification et de la suppression avec la DataList dans un prochain didacticiel.

Nous allons prendre un moment pour améliorer l’apparence de ce modèle. Au lieu d’afficher tous les champs de données, laissez s afficher uniquement le nom du produit, le fournisseur, la catégorie, la quantité par unité et le prix unitaire. En outre, voyons comment afficher le nom dans un en-tête `<h4>` et disposer les champs restants à l’aide d’un `<table>` sous le titre.

Pour apporter ces modifications, vous pouvez utiliser les fonctionnalités de modification de modèle dans le concepteur à partir de la balise active de DataList s cliquez sur le lien modifier les modèles, ou vous pouvez modifier le modèle manuellement via la syntaxe déclarative de la page. Si vous utilisez l’option modifier les modèles dans le concepteur, le balisage qui en résulte peut ne pas correspondre exactement au balisage suivant, mais lorsqu’il est affiché dans un navigateur, il doit être très similaire à la capture d’écran illustrée à la figure 8.

[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample3.aspx)]

> [!NOTE]
> L’exemple ci-dessus utilise des contrôles Label Web dont la valeur de la syntaxe DataBinding est assignée à la propriété `Text`. Vous pouvez également avoir omis les étiquettes, en tapant simplement la syntaxe DataBinding. Autrement dit, au lieu d’utiliser `<asp:Label ID="CategoryNameLabel" runat="server" Text='<%# Eval("CategoryName") %>' />` nous aurions pu utiliser à la place la syntaxe déclarative `<%# Eval("CategoryName") %>`.

Toutefois, dans les contrôles Web Label, offre deux avantages. Tout d’abord, il fournit une méthode plus simple pour mettre en forme les données en fonction des données, comme nous le verrons dans le prochain didacticiel. Deuxièmement, l’option modifier les modèles dans le concepteur n’affiche pas la syntaxe de liaison de données déclarative qui apparaît en dehors de certains contrôles Web. Au lieu de cela, l’interface Edit Templates est conçue pour faciliter l’utilisation du balisage statique et des contrôles Web et suppose que toute liaison de liaison sera effectuée par le biais de la boîte de dialogue Modifier les DataBindings, accessible à partir des balises actives des contrôles Web.

Par conséquent, lorsque vous travaillez avec la DataList, qui offre la possibilité de modifier les modèles par le biais du concepteur, je préfère utiliser des contrôles Web label afin que le contenu soit accessible via l’interface Edit Templates. Comme nous le verrons bientôt, le Repeater exige que le contenu du modèle soit modifié à partir de la vue source. Par conséquent, lors de la création des modèles Repeater s, je vais souvent omettre les contrôles Web Label, sauf si je sais que je dois mettre en forme l’apparence du texte lié aux données en fonction de la logique de programmation.

[![chaque sortie de produit est rendue à l’aide du ItemTemplate de DataList](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image19.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image18.png)

**Figure 8**: la sortie de chaque produit est rendue à l’aide de la `ItemTemplate` de contrôle DataList ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image20.png))

## <a name="step-3-improving-the-appearance-of-the-datalist"></a>Étape 3 : amélioration de l’apparence du contrôle DataList

À l’instar de GridView, le contrôle DataList offre un certain nombre de propriétés liées au style, telles que `Font`, `ForeColor`, `BackColor`, `CssClass`, `ItemStyle`, `AlternatingItemStyle`, `SelectedItemStyle`, et ainsi de suite. Lorsque vous utilisez les contrôles GridView et DetailsView, nous avons créé des fichiers d’apparence dans le thème `DataWebControls` qui prédéfinissent les propriétés de `CssClass` pour ces deux contrôles et la propriété `CssClass` pour plusieurs de leurs sous-propriétés (`RowStyle`, `HeaderStyle`, etc.). Faites de même pour le contrôle DataList.

Comme indiqué dans le didacticiel [affichage des données avec ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) , un fichier d’apparence spécifie les propriétés relatives à l’apparence par défaut pour un contrôle Web. un thème est une collection de fichiers de peau, CSS, image et JavaScript qui définissent une apparence et une convivialité spécifiques pour un site Web. Dans le didacticiel *affichage des données avec ObjectDataSource* , nous avons créé un thème de `DataWebControls` (qui est implémenté en tant que dossier dans le dossier `App_Themes`) qui possède actuellement deux fichiers d’apparence : `GridView.skin` et `DetailsView.skin`. Ajoutez un troisième fichier d’apparence pour spécifier les paramètres de style prédéfinis pour le contrôle DataList.

Pour ajouter un fichier d’apparence, cliquez avec le bouton droit sur le dossier `App_Themes/DataWebControls`, choisissez Ajouter un nouvel élément, puis sélectionnez l’option fichier d’apparence dans la liste. Nommez le fichier `DataList.skin`.

[![créer un nouveau fichier d’apparence nommé DataList. Skin](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image22.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image21.png)

**Figure 9**: créer un nouveau fichier d’apparence nommé `DataList.skin` ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image23.png))

Utilisez le balisage suivant pour le fichier `DataList.skin` :

[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample4.aspx)]

Ces paramètres assignent les mêmes classes CSS aux propriétés DataList appropriées, telles qu’elles étaient utilisées avec les contrôles GridView et DetailsView. Les classes CSS utilisées ici `DataWebControlStyle`, `AlternatingRowStyle`, `RowStyle`, etc. sont définies dans le fichier `Styles.css` et ont été ajoutées dans les didacticiels précédents.

Avec l’ajout de ce fichier d’apparence, l’apparence des contrôles DataList est mise à jour dans le concepteur (vous devrez peut-être actualiser le mode concepteur pour voir les effets du nouveau fichier d’apparence. dans le menu Affichage, choisissez actualiser). Comme le montre la figure 10, chaque produit de remplacement a une couleur d’arrière-plan rose clair.

[![créer un nouveau fichier d’apparence nommé DataList. Skin](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image25.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image24.png)

**Figure 10**: créer un nouveau fichier d’apparence nommé `DataList.skin` ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image26.png))

## <a name="step-4-exploring-the-datalist-s-other-templates"></a>Étape 4 : exploration des autres modèles de DataList

En plus de la `ItemTemplate`, la DataList prend en charge six autres modèles facultatifs :

- `HeaderTemplate` s’il est fourni, ajoute une ligne d’en-tête à la sortie et est utilisé pour afficher cette ligne
- `AlternatingItemTemplate` utilisé pour le rendu des éléments de remplacement
- `SelectedItemTemplate` utilisé pour afficher l’élément sélectionné ; l’élément sélectionné est l’élément dont l’index correspond à la propriété DataList s [`SelectedIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.selectedindex.aspx)
- `EditItemTemplate` utilisé pour restituer l’élément en cours de modification
- `SeparatorTemplate` s’il est fourni, ajoute un séparateur entre chaque élément et est utilisé pour restituer ce séparateur
- `FooterTemplate`-si elle est fournie, ajoute une ligne de pied de page à la sortie et est utilisée pour afficher cette ligne

Lorsque vous spécifiez la `HeaderTemplate` ou la `FooterTemplate`, DataList ajoute une ligne d’en-tête ou de pied de page supplémentaire à la sortie rendue. Comme avec les lignes d’en-tête et de pied de page GridView s, l’en-tête et le pied de page d’un contrôle DataList ne sont pas liés aux données. Par conséquent, toute syntaxe DataBinding dans le `HeaderTemplate` ou `FooterTemplate` qui tente d’accéder aux données liées retourne une chaîne vide.

> [!NOTE]
> Comme nous l’avons vu dans l' [affichage des informations de synthèse dans le didacticiel sur le pied de page de GridView s](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md) , alors que les lignes d’en-tête et de pied de page ne prennent pas en charge la syntaxe DataBinding, les informations spécifiques aux données peuvent être injectées directement dans ces lignes à partir du gestionnaire d’événements `RowDataBound` GridView s. Cette technique peut être utilisée pour calculer des totaux en cours d’exécution ou d’autres informations à partir des données liées au contrôle, ainsi que pour affecter ces informations au pied de page. Ce même concept peut être appliqué aux contrôles DataList et Repeater. la seule différence est que pour les contrôles DataList et Repeater, créez un gestionnaire d’événements pour l’événement `ItemDataBound` (au lieu de pour l’événement `RowDataBound`).

Pour notre exemple, les informations sur le produit du titre s’affichent en haut des contrôles DataList dans un en-tête de `<h3>`. Pour ce faire, ajoutez un `HeaderTemplate` avec le balisage approprié. Pour ce faire, dans le concepteur, cliquez sur le lien modifier les modèles dans la balise active de DataList s, choisissez le modèle d’en-tête dans la liste déroulante, puis tapez le texte après avoir sélectionné l’option titre 3 dans la liste déroulante style (voir figure 11).

[![ajouter un HeaderTemplate avec le texte informations sur le produit](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image28.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image27.png)

**Figure 11**: ajouter un `HeaderTemplate` avec le texte informations sur le produit ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image29.png))

Cela peut également être ajouté de façon déclarative en entrant la balise suivante dans les balises `<asp:DataList>` :

[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample5.html)]

Pour ajouter un peu d’espace entre chaque liste de produits, ajoutez une `SeparatorTemplate` qui comprend une ligne entre chaque section. La balise de règle horizontale (`<hr>`) ajoute un tel séparateur. Créez le `SeparatorTemplate` pour qu’il ait le balisage suivant :

[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample6.html)]

> [!NOTE]
> Comme les `HeaderTemplate` et `FooterTemplates`, le `SeparatorTemplate` n’est lié à aucun enregistrement de la source de données et, par conséquent, ne peut pas accéder directement aux enregistrements de la source de données liés au contrôle DataList.

Après avoir apporté cet ajout, lorsque vous affichez la page via un navigateur, elle doit ressembler à la figure 12. Notez la ligne d’en-tête et la ligne entre chaque liste de produits.

[![la DataList contient une ligne d’en-tête et une règle horizontale entre chaque liste de produits](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image31.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image30.png)

**Figure 12**: la DataList contient une ligne d’en-tête et une règle horizontale entre chaque liste de produits ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image32.png))

## <a name="step-5-rendering-specific-markup-with-the-repeater-control"></a>Étape 5 : rendu d’un balisage spécifique à l’aide du contrôle Repeater

Si vous effectuez une vue ou une source à partir de votre navigateur lorsque vous visitez l’exemple DataList de la figure 12, vous verrez que la DataList émet un `<table>` HTML qui contient une ligne de table (`<tr>`) avec une cellule de table unique (`<td>`) pour chaque élément lié au contrôle DataList. En fait, cette sortie est identique à ce qui serait émis à partir d’un GridView avec un seul TemplateField. Comme nous le verrons dans un prochain didacticiel, la DataList permet une personnalisation supplémentaire de la sortie, ce qui nous permet d’afficher plusieurs enregistrements de source de données par ligne de table.

Que se passe-t-il si vous ne souhaitez pas émettre de `<table>`HTML ? Pour le contrôle total et total sur le balisage généré par un contrôle Web de données, nous devons utiliser le contrôle Repeater. À l’instar de la DataList, le Repeater est construit en fonction des modèles. Toutefois, le Repeater offre uniquement les cinq modèles suivants :

- `HeaderTemplate` s’il est fourni, ajoute le balisage spécifié avant les éléments
- `ItemTemplate` utilisé pour le rendu des éléments
- `AlternatingItemTemplate` s’il est fourni, utilisé pour afficher des éléments de remplacement
- `SeparatorTemplate` s’il est fourni, ajoute le balisage spécifié entre chaque élément
- `FooterTemplate`-si fourni, ajoute la balise spécifiée après les éléments

Dans ASP.NET 1. x, le contrôle Repeater était généralement utilisé pour afficher une liste à puces dont les données proviennent d’une source de données. Dans ce cas, les `HeaderTemplate` et `FooterTemplates` contenir respectivement les balises d’ouverture et de fermeture `<ul>`, tandis que le `ItemTemplate` contient des éléments `<li>` avec la syntaxe DataBinding. Cette approche peut toujours être utilisée dans ASP.NET 2,0 comme nous l’avons vu dans deux exemples dans le didacticiel sur les [pages maîtres et la navigation](../introduction/master-pages-and-site-navigation-cs.md) dans les sites :

- Dans la page maître `Site.master`, un répéteur a été utilisé pour afficher une liste à puces du contenu du plan de site de niveau supérieur (création de rapports de base, filtrage des rapports, mise en forme personnalisée, etc.); un autre Repeater imbriqué a été utilisé pour afficher les sections enfants des sections de niveau supérieur
- Dans `SectionLevelTutorialListing.ascx`, un Repeater a été utilisé pour afficher une liste à puces des sections enfants de la section du plan de site actuel

> [!NOTE]
> ASP.NET 2,0 introduit le nouveau [contrôle BulletedList](https://msdn.microsoft.com/library/ms228101.aspx), qui peut être lié à un contrôle de source de données afin d’afficher une liste à puces simple. Avec le contrôle BulletedList, nous n’avons pas besoin de spécifier l’un des éléments HTML liés à la liste. au lieu de cela, nous indiquons simplement le champ de données à afficher en tant que texte pour chaque élément de liste.

Le Repeater fait office de contrôle Web d’interception de données. S’il n’existe pas de contrôle existant qui génère le balisage nécessaire, le contrôle Repeater peut être utilisé. Pour illustrer l’utilisation du Repeater, utilisez la liste des catégories affichée au-dessus des informations sur le produit, créée à l’étape 2. En particulier, les catégories sont affichées dans un `<table>` HTML à une seule ligne avec chaque catégorie affichée sous la forme d’une colonne dans la table.

Pour ce faire, commencez par faire glisser un contrôle Repeater de la boîte à outils vers le concepteur, au-dessus des informations sur le produit DataList. Comme avec la DataList, le répéteur s’affiche initialement sous la forme d’une zone grise jusqu’à ce que ses modèles aient été définis.

[![ajouter un répéteur au concepteur](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image34.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image33.png)

**Figure 13**: ajouter un répéteur au concepteur ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image35.png))

Il n’y a qu’une seule option dans la balise active de Repeater s : choisir la source de données. Choisissez de créer un ObjectDataSource et configurez-le pour qu’il utilise la méthode de `GetCategories` de la classe `CategoriesBLL`.

[![créer un ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image37.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image36.png)

**Figure 14**: créer un ObjectDataSource ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image38.png))

[![configurer ObjectDataSource pour utiliser la classe CategoriesBLL](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image40.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image39.png)

**Figure 15**: configurer ObjectDataSource pour utiliser la classe `CategoriesBLL` ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image41.png))

[![récupérer des informations sur toutes les catégories à l’aide de la méthode GetCategories](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image43.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image42.png)

**Figure 16**: récupérer des informations sur toutes les catégories à l’aide de la méthode `GetCategories` ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image44.png))

Contrairement au contrôle DataList, Visual Studio ne crée pas automatiquement un ItemTemplate pour le Repeater après l’avoir lié à une source de données. En outre, les modèles Repeater s ne peuvent pas être configurés par le biais du concepteur et doivent être spécifiés de façon déclarative.

Pour afficher les catégories sous la forme d’une seule ligne `<table>` avec une colonne pour chaque catégorie, il faut que le Repeater émette un balisage similaire à ce qui suit :

[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample7.html)]

Étant donné que la `<td>Category X</td>` texte est la partie qui se répète, elle apparaît dans le ItemTemplate de Repeater s. Le balisage qui apparaît avant-`<table><tr>`-est placé dans le `HeaderTemplate`, tandis que le marqueur de fin `</tr></table>`-est placé dans le `FooterTemplate`. Pour entrer ces paramètres de modèle, accédez à la partie déclarative de la page ASP.NET en cliquant sur le bouton source dans le coin inférieur gauche et tapez la syntaxe suivante :

[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample8.aspx)]

Le Repeater émet le balisage précis tel que spécifié par ses modèles, rien de plus, rien de moins. La figure 17 illustre la sortie de Repeater s lorsqu’elle est affichée dans un navigateur.

[![un tableau de &lt;HTML à une seule ligne&gt; répertorie chaque catégorie dans une colonne distincte](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image46.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image45.png)

**Figure 17**: un `<table>` HTML à une seule ligne répertorie chaque catégorie dans une colonne distincte ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image47.png))

## <a name="step-6-improving-the-appearance-of-the-repeater"></a>Étape 6 : amélioration de l’apparence du répéteur

Étant donné que le Repeater émet précisément le balisage spécifié par ses modèles, il ne doit pas être surprenant qu’il n’y ait pas de propriétés liées au style pour le Repeater. Pour modifier l’apparence du contenu généré par le Repeater, nous devons ajouter manuellement le contenu HTML ou CSS nécessaire directement aux modèles Repeater s.

Pour notre exemple, nous allons faire en sorte que les colonnes de catégorie présentent d’autres couleurs d’arrière-plan, comme avec les lignes alternées dans le contrôle DataList. Pour ce faire, nous devons affecter la classe CSS `RowStyle` à chaque élément Repeater et la classe CSS `AlternatingRowStyle` à chaque élément Repeater de remplacement par le biais des modèles `ItemTemplate` et `AlternatingItemTemplate`, comme suit :

[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample9.aspx)]

Let s ajoute également une ligne d’en-tête à la sortie avec les catégories de produits texte. Étant donné que nous ne savons pas combien de colonnes, le `<table>` qui en résulte sera constitué, le moyen le plus simple de générer une ligne d’en-tête qui est garantie pour couvrir toutes les colonnes consiste à utiliser *deux* `<table>` s. La première `<table>` contient deux lignes : la ligne d’en-tête et une ligne qui contiendra la deuxième `<table>` une seule ligne qui a une colonne pour chaque catégorie du système. Autrement dit, nous souhaitons émettre le balisage suivant :

[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample10.html)]

Les `HeaderTemplate` et `FooterTemplate` suivants entraînent le balisage souhaité :

[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample11.aspx)]

La figure 18 illustre le répéteur une fois que ces modifications ont été apportées.

[![les colonnes de catégorie alterner dans la couleur d’arrière-plan et contient une ligne d’en-tête](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image49.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image48.png)

**Figure 18**: les colonnes de catégorie alternent avec la couleur d’arrière-plan et incluent une ligne d’en-tête ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image50.png))

## <a name="summary"></a>Récapitulatif

Bien que le contrôle GridView facilite l’affichage, la modification, la suppression, le tri et la pagination des données, l’apparence est très boxy et similaire à la grille. Pour un meilleur contrôle de l’apparence, nous devons activer les contrôles DataList ou Repeater. Ces deux contrôles affichent un ensemble d’enregistrements à l’aide de modèles au lieu de BoundFields, CheckBoxFields, et ainsi de suite.

Le contrôle DataList est rendu sous forme de `<table>` HTML qui, par défaut, affiche chaque enregistrement de source de données dans une ligne de table unique, tout comme un GridView avec un TemplateField unique. Comme nous le verrons dans un futur didacticiel, toutefois, le contrôle DataList autorise l’affichage de plusieurs enregistrements par ligne de table. En revanche, le répéteur émet strictement le balisage spécifié dans ses modèles ; elle n’ajoute pas de balisage supplémentaire et est donc couramment utilisée pour afficher des données dans des éléments HTML autres qu’un `<table>` (par exemple, dans une liste à puces).

Bien que le contrôle DataList et le Repeater offrent plus de flexibilité dans leur sortie rendue, ils disposent de nombreuses fonctionnalités intégrées qui se trouvent dans le GridView. Comme nous le verrons dans les didacticiels à venir, certaines de ces fonctionnalités peuvent être reconnectées sans trop d’efforts, mais n’oubliez pas que l’utilisation du contrôle DataList ou Repeater à la place de GridView limite les fonctionnalités que vous pouvez utiliser sans avoir à implémenter ces fonctionnalités test.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel étaient Yaakov Ellis, Liz Shulok, Randy Schmidt et Stacy Park. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](formatting-the-datalist-and-repeater-based-upon-data-cs.md)
