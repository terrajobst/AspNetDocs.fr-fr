---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-cs
title: Ajout d’une colonne GridView de cases d'C#option () | Microsoft Docs
author: rick-anderson
description: Ce didacticiel explique comment ajouter une colonne de cases d’option à un contrôle GridView pour fournir à l’utilisateur un moyen plus intuitif de sélectionner une seule ligne...
ms.author: riande
ms.date: 03/06/2007
ms.assetid: 32377145-ec25-4715-8370-a1c590a331d5
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-cs
msc.type: authoredcontent
ms.openlocfilehash: b59cc64b14c6414e6558fdb8a281644db8386701
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78591097"
---
# <a name="adding-a-gridview-column-of-radio-buttons-c"></a>Ajout d’une colonne GridView de cases d’option (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_51_CS.exe) ou [Télécharger le PDF](adding-a-gridview-column-of-radio-buttons-cs/_static/datatutorial51cs1.pdf)

> Ce didacticiel explique comment ajouter une colonne de cases d’option à un contrôle GridView pour fournir à l’utilisateur un moyen plus intuitif de sélectionner une seule ligne de GridView.

## <a name="introduction"></a>Introduction

Le contrôle GridView offre une grande quantité de fonctionnalités intégrées. Il comprend un certain nombre de champs différents permettant d’afficher du texte, des images, des liens hypertexte et des boutons. Il prend en charge les modèles pour une personnalisation plus poussée. En quelques clics de souris, il est possible de créer un contrôle GridView dans lequel chaque ligne peut être sélectionnée à l’aide d’un bouton ou d’activer les fonctionnalités de modification ou de suppression. Malgré la multitude de fonctionnalités fournies, il y aura souvent des situations dans lesquelles des fonctionnalités supplémentaires non prises en charge devront être ajoutées. Dans ce didacticiel et les deux suivants, nous allons examiner comment améliorer la fonctionnalité de GridView s pour inclure des fonctionnalités supplémentaires.

Ce didacticiel et l’élément suivant portent sur l’amélioration du processus de sélection de lignes. Comme examiné dans le [maître/détail à l’aide d’un GridView maître sélectionnable avec un DetailView détails](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md), nous pouvons ajouter une CommandField au GridView qui comprend un bouton Sélectionner. Lorsque vous cliquez dessus, une publication est effectuée et la propriété GridView s `SelectedIndex` est mise à jour vers l’index de la ligne dont le bouton Sélectionner a été activé. Dans le didacticiel [maître/détail utilisant un GridView maître sélectionnable avec un didacticiel détails DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) , nous avons vu comment utiliser cette fonctionnalité pour afficher les détails de la ligne GridView sélectionnée.

Si le bouton Sélectionner fonctionne dans de nombreuses situations, il peut ne pas fonctionner aussi bien pour d’autres. Au lieu d’utiliser un bouton, deux autres éléments de l’interface utilisateur sont généralement utilisés pour la sélection : la case d’option et la case à cocher. Nous pouvons augmenter le GridView afin qu’à la place d’un bouton Sélectionner, chaque ligne contienne une case d’option ou une case à cocher. Dans les scénarios où l’utilisateur ne peut sélectionner qu’un des enregistrements GridView, la case d’option peut être préférée sur le bouton Sélectionner. Dans les situations où l’utilisateur peut sélectionner plusieurs enregistrements, par exemple dans une application de messagerie Web, où un utilisateur peut vouloir sélectionner plusieurs messages pour supprimer la case à cocher propose des fonctionnalités qui ne sont pas disponibles à partir du bouton Sélectionner ou de la case d’option interfaces utilisateur.

Ce didacticiel explique comment ajouter une colonne de cases d’option au contrôle GridView. Le didacticiel de procédure explore l’utilisation de cases à cocher.

## <a name="step-1-creating-the-enhancing-the-gridview-web-pages"></a>Étape 1 : création de l’amélioration des pages Web GridView

Avant de commencer à améliorer le contrôle GridView pour inclure une colonne de cases d’option, commencez par prendre un moment pour créer les pages ASP.NET dans notre projet de site Web dont nous aurons besoin pour ce didacticiel et les deux suivantes. Commencez par ajouter un nouveau dossier nommé `EnhancedGridView`. Ajoutez ensuite les pages ASP.NET suivantes à ce dossier, en veillant à associer chaque page à la page maître `Site.master` :

- `Default.aspx`
- `RadioButtonField.aspx`
- `CheckBoxField.aspx`
- `InsertThroughFooter.aspx`

![Ajouter les pages ASP.NET pour les didacticiels liés à SqlDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image1.gif)

**Figure 1**: ajouter les pages ASP.net pour les didacticiels liés à SqlDataSource

Comme dans les autres dossiers, `Default.aspx` dans le dossier `EnhancedGridView` répertorie les didacticiels de la section. N’oubliez pas que le contrôle utilisateur `SectionLevelTutorialListing.ascx` fournit cette fonctionnalité. Par conséquent, ajoutez ce contrôle utilisateur à `Default.aspx` en le faisant glisser de l’Explorateur de solutions sur la page s Mode Création.

[![ajouter le contrôle utilisateur SectionLevelTutorialListing. ascx à default. aspx](adding-a-gridview-column-of-radio-buttons-cs/_static/image2.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image1.png)

**Figure 2**: ajouter le contrôle utilisateur `SectionLevelTutorialListing.ascx` à `Default.aspx` ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-radio-buttons-cs/_static/image2.png))

Enfin, ajoutez ces quatre pages en tant qu’entrées au fichier `Web.sitemap`. Plus précisément, ajoutez le balisage suivant après l’utilisation du contrôle SqlDataSource `<siteMapNode>`:

[!code-xml[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample1.xml)]

Après avoir mis à jour `Web.sitemap`, prenez un moment pour afficher le site Web des didacticiels via un navigateur. Le menu à gauche contient maintenant des éléments pour les didacticiels de modification, d’insertion et de suppression.

![Le plan de site comprend maintenant des entrées pour l’amélioration des didacticiels GridView](adding-a-gridview-column-of-radio-buttons-cs/_static/image3.gif)

**Figure 3**: le plan de site contient maintenant des entrées pour l’amélioration des didacticiels GridView

## <a name="step-2-displaying-the-suppliers-in-a-gridview"></a>Étape 2 : affichage des fournisseurs dans un GridView

Pour ce didacticiel, vous allez créer un GridView qui répertorie les fournisseurs des États-Unis, chaque ligne GridView fournissant une case d’option. Après avoir sélectionné un fournisseur à l’aide de la case d’option, l’utilisateur peut afficher les produits du fournisseur en cliquant sur un bouton. Bien que cette tâche puisse paraître insignifiante, il y a un certain nombre de subtilités qui la rendent particulièrement difficile. Avant de nous plonger dans ces subtilités, commençons par obtenir un contrôle GridView énumérant les fournisseurs.

Commencez par ouvrir la page `RadioButtonField.aspx` dans le dossier `EnhancedGridView` en faisant glisser un contrôle GridView de la boîte à outils vers le concepteur. Définissez le `ID` GridView s sur `Suppliers` et, à partir de sa balise active, choisissez de créer une nouvelle source de données. En particulier, créez un ObjectDataSource nommé `SuppliersDataSource` qui extrait ses données de l’objet `SuppliersBLL`.

[![créer un ObjectDataSource nommé SuppliersDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image4.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image3.png)

**Figure 4**: créer un ObjectDataSource nommé `SuppliersDataSource` ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-radio-buttons-cs/_static/image4.png))

[![configurer ObjectDataSource pour utiliser la classe SuppliersBLL](adding-a-gridview-column-of-radio-buttons-cs/_static/image5.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image5.png)

**Figure 5**: configurer ObjectDataSource pour utiliser la classe `SuppliersBLL` ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-radio-buttons-cs/_static/image6.png))

Étant donné que nous voulons uniquement répertorier ces fournisseurs aux États-Unis, choisissez la méthode `GetSuppliersByCountry(country)` dans la liste déroulante de l’onglet sélectionner.

[![configurer ObjectDataSource pour utiliser la classe SuppliersBLL](adding-a-gridview-column-of-radio-buttons-cs/_static/image6.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image7.png)

**Figure 6**: configurer ObjectDataSource pour utiliser la classe `SuppliersBLL` ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-radio-buttons-cs/_static/image8.png))

Dans l’onglet mettre à jour, sélectionnez l’option (aucune), puis cliquez sur suivant.

[![configurer ObjectDataSource pour utiliser la classe SuppliersBLL](adding-a-gridview-column-of-radio-buttons-cs/_static/image7.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image9.png)

**Figure 7**: configurer ObjectDataSource pour utiliser la classe `SuppliersBLL` ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-radio-buttons-cs/_static/image10.png))

Étant donné que la méthode `GetSuppliersByCountry(country)` accepte un paramètre, l’Assistant Configuration de la source de données nous invite à entrer la source de ce paramètre. Pour spécifier une valeur codée en dur (USA, dans cet exemple), laissez la liste déroulante source de paramètres définie sur aucun et entrez la valeur par défaut dans la zone de texte. Cliquez sur Terminer pour terminer l'Assistant.

[![utiliser les États-Unis comme valeur par défaut pour le paramètre Country](adding-a-gridview-column-of-radio-buttons-cs/_static/image8.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image11.png)

**Figure 8**: utiliser les États-Unis comme valeur par défaut pour le paramètre `country` ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-radio-buttons-cs/_static/image12.png))

Une fois l’exécution de l’Assistant terminée, le contrôle GridView inclura un BoundField pour chacun des champs de données du fournisseur. Supprimez tous les `CompanyName`, `City`et `Country` BoundFields, puis renommez la propriété `CompanyName` BoundFields `HeaderText` en Supplier. Après cela, la syntaxe déclarative GridView et ObjectDataSource doit ressembler à ce qui suit.

[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample2.aspx)]

Pour ce didacticiel, autorisez l’utilisateur à afficher les produits du fournisseur sélectionnés sur la même page que la liste des fournisseurs ou sur une autre page. Pour ce faire, ajoutez deux contrôles Web Button à la page. J’ai défini les `ID` de ces deux boutons pour `ListProducts` et `SendToProducts`, avec l’idée que lorsque l’utilisateur clique sur `ListProducts`, une publication (postback) se produira et que les produits du fournisseur sélectionnés seront répertoriés sur la même page, mais lorsque vous cliquerez sur `SendToProducts`, l’utilisateur sera affecté à une autre page qui répertorie les produits.

La figure 9 illustre le `Suppliers` GridView et les deux contrôles Web Button lorsqu’ils sont affichés dans un navigateur.

[![ces fournisseurs des États-Unis ont leurs informations de nom, de ville et de pays.](adding-a-gridview-column-of-radio-buttons-cs/_static/image9.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image13.png)

**Figure 9**: les informations relatives au nom, à la ville et au pays des fournisseurs des États-Unis sont indiquées ([cliquez ici pour afficher l’image en plein écran](adding-a-gridview-column-of-radio-buttons-cs/_static/image14.png))

## <a name="step-3-adding-a-column-of-radio-buttons"></a>Étape 3 : ajout d’une colonne de cases d’option

À ce stade, le `Suppliers` GridView a trois BoundFields qui affichent le nom de la société, la ville et le pays de chaque fournisseur aux États-Unis. Toutefois, il n’y a pas encore de colonne de cases d’option. Malheureusement, le GridView n’inclut pas de RadioButtonField intégré. dans le cas contraire, nous pourrions simplement l’ajouter à la grille et le faire. Au lieu de cela, nous pouvons ajouter un TemplateField et configurer son `ItemTemplate` pour afficher une case d’option, entraînant une case d’option pour chaque ligne GridView.

Au départ, nous pourrions supposer que l’interface utilisateur souhaitée peut être implémentée en ajoutant un contrôle Web RadioButton au `ItemTemplate` d’un TemplateField. Bien que cette opération ajoute une seule case d’option à chaque ligne du GridView, les cases d’option ne peuvent pas être regroupées et ne sont donc pas mutuellement exclusives. Autrement dit, un utilisateur final peut sélectionner plusieurs cases d’option simultanément à partir du contrôle GridView.

Même si l’utilisation d’un TemplateField de contrôles Web RadioButton n’offre pas les fonctionnalités dont nous avons besoin, nous allons implémenter cette approche, car il est utile d’examiner la raison pour laquelle les cases d’option résultantes ne sont pas regroupées. Commencez par ajouter un TemplateField au GridView Suppliers, en le rendant le champ le plus à gauche. Ensuite, dans la balise active GridView s, cliquez sur le lien modifier les modèles, puis faites glisser un contrôle Web RadioButton de la boîte à outils vers le TemplateField s `ItemTemplate` (voir figure 10). Affectez à la propriété RadioButton s `ID` la valeur `RowSelector` et à la propriété `GroupName` la valeur `SuppliersGroup`.

[![ajouter un contrôle Web RadioButton au ItemTemplate](adding-a-gridview-column-of-radio-buttons-cs/_static/image10.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image15.png)

**Figure 10**: ajouter un contrôle Web RadioButton au `ItemTemplate` ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-radio-buttons-cs/_static/image16.png))

Après avoir effectué ces ajouts par le biais du concepteur, le balisage de GridView s doit ressembler à ce qui suit :

[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample3.aspx)]

La propriété RadioButton s [`GroupName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.radiobutton.groupname(VS.80).aspx) est utilisée pour regrouper une série de cases d’option. Tous les contrôles RadioButton avec la même valeur `GroupName` sont considérés comme regroupés ; une seule case d’option peut être sélectionnée à la fois dans un groupe. La propriété `GroupName` spécifie la valeur de l’attribut rendu des cases d’option `name`. Le navigateur examine les cases d’option `name` attributs pour déterminer les regroupements de cases d’option.

Une fois le contrôle Web RadioButton ajouté au `ItemTemplate`, accédez à cette page via un navigateur et cliquez sur les cases d’option dans la grille s lignes. Notez que les cases d’option ne sont pas regroupées, ce qui permet de sélectionner toutes les lignes, comme le montre la figure 11.

[![les cases d’option GridView s ne sont pas regroupées](adding-a-gridview-column-of-radio-buttons-cs/_static/image11.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image17.png)

**Figure 11**: les cases d’option de GridView s ne sont pas regroupées ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-radio-buttons-cs/_static/image18.png))

La raison pour laquelle les cases d’option ne sont pas regroupées est la raison pour laquelle les attributs de `name` restitués sont différents, malgré le même `GroupName` paramètre de propriété. Pour voir ces différences, effectuez une vue ou une source à partir du navigateur, puis examinez le balisage de la case d’option :

[!code-html[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample4.html)]

Notez que les attributs `name` et `id` ne sont pas les valeurs exactes spécifiées dans le Fenêtre Propriétés, mais sont précédées d’un certain nombre d’autres valeurs de `ID`. Les valeurs de `ID` supplémentaires ajoutées à l’avant des attributs `id` et `name` rendus sont les `ID` s des cases d’option parent contrôlent les `GridViewRow` s `ID` s, les `ID`de GridView, les `ID`du contrôle de contenu et le formulaire Web s `ID`. Ces `ID` s sont ajoutées afin que chaque contrôle Web rendu dans le GridView ait des valeurs de `id` et de `name` uniques.

Chaque contrôle rendu a besoin d’un `name` différent et `id`, car c’est ainsi que le navigateur identifie de manière unique chaque contrôle côté client et comment il identifie le serveur Web en quoi l’action ou la modification s’est produite lors de la publication (postback). Par exemple, imaginons que nous voulions exécuter du code côté serveur chaque fois qu’un état de case à cocher RadioButton a été modifié. Pour ce faire, nous avons défini la propriété RadioButton s `AutoPostBack` sur `true` et à créer un gestionnaire d’événements pour l’événement `CheckChanged`. Toutefois, si les valeurs `name` et `id` affichées pour toutes les cases d’option étaient identiques, lors de la publication (postback), nous n’avons pas pu déterminer le bouton RadioButton spécifique sur lequel l’utilisateur a cliqué.

Le plus bref, c’est que nous ne pouvons pas créer une colonne de cases d’option dans un GridView à l’aide du contrôle Web RadioButton. Au lieu de cela, nous devons utiliser des techniques plutôt archaïques pour garantir que le balisage approprié est injecté dans chaque ligne GridView.

> [!NOTE]
> À l’instar du contrôle Web RadioButton, le contrôle HTML de case d’option, lorsqu’il est ajouté à un modèle, inclut l’attribut `name` unique, ce qui rend les cases d’option de la grille non groupées. Si vous n’êtes pas familiarisé avec les contrôles HTML, n’hésitez pas à ignorer cette remarque, car les contrôles HTML sont rarement utilisés, en particulier dans ASP.NET 2,0. Toutefois, si vous souhaitez en savoir plus, consultez la rubrique sur [les contrôles Web et les contrôles HTML de](http://www.odetocode.com/Articles/348.aspx) [K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx) s.

## <a name="using-a-literal-control-to-inject-radio-button-markup"></a>Utilisation d’un contrôle Literal pour injecter le balisage de case d’option

Afin de regrouper correctement toutes les cases d’option dans le GridView, nous devons injecter manuellement le balisage des cases d’option dans le `ItemTemplate`. Chaque case d’option doit avoir le même `name` attribut, mais doit avoir un attribut `id` unique (au cas où nous voulons accéder à une case d’option via un script côté client). Une fois qu’un utilisateur a sélectionné une case d’option et publié la page, le navigateur renvoie la valeur de l’attribut `value` cases d’option sélectionnées. Par conséquent, chaque case d’option doit avoir un attribut `value` unique. Enfin, lors de la publication (postback), nous devons veiller à ajouter l’attribut `checked` à la case d’option qui est sélectionnée. dans le cas contraire, une fois que l’utilisateur a effectué une sélection et publie, les cases d’option reviennent à leur état par défaut (toutes non sélectionnées).

Il existe deux approches qui peuvent être prises afin d’injecter un balisage de bas niveau dans un modèle. L’une consiste à effectuer une combinaison de balises et d’appels aux méthodes de mise en forme définies dans la classe code-behind. Cette technique a été abordée dans le didacticiel [utilisation de TemplateFields dans le contrôle GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) . Dans notre cas, il peut ressembler à ceci :

[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample5.aspx)]

Ici, `GetUniqueRadioButton` et `GetRadioButtonValue` seraient des méthodes définies dans la classe code-behind qui renvoyaient les valeurs d’attribut `id` et `value` appropriées pour chaque case d’option. Cette approche fonctionne bien pour l’affectation des attributs `id` et `value`, mais elle est très brève quand vous devez spécifier la valeur de l’attribut `checked`, car la syntaxe DataBinding n’est exécutée que lorsque les données sont liées pour la première fois au GridView. Par conséquent, si l’état d’affichage de GridView est activé, les méthodes de mise en forme se déclenchent uniquement quand la page est chargée pour la première fois (ou lorsque le GridView est explicitement relié à la source de données). par conséquent, la fonction qui définit l’attribut `checked` n’est pas appelée lors de la publication (postback). Il s’agit d’un problème plutôt subtil et d’un peu au-delà de la portée de cet article, je vais donc le faire. En revanche, je vous encourage à essayer d’utiliser l’approche ci-dessus et à l’utiliser jusqu’au point où vous serez bloqué. Bien qu’un tel exercice ne vous permette pas de vous rapprocher d’une version de travail, il vous aidera à mieux comprendre le GridView et le cycle de vie des liaisons de liaison.

L’autre approche pour injecter des balises personnalisées, de bas niveau dans un modèle et l’approche que nous allons utiliser pour ce didacticiel consiste à ajouter un [contrôle Literal](https://msdn.microsoft.com/library/sz4949ks(VS.80).aspx) au modèle. Ensuite, dans le `RowCreated` GridView s ou `RowDataBound` gestionnaire d’événements, le contrôle Literal peut être accédé par programmation et sa propriété `Text` définie sur le balisage à émettre.

Commencez par supprimer le RadioButton de l' `ItemTemplate`TemplateField s, en le remplaçant par un contrôle Literal. Définissez les `ID` du contrôle Literal sur `RadioButtonMarkup`.

[![ajouter un contrôle Literal au ItemTemplate](adding-a-gridview-column-of-radio-buttons-cs/_static/image12.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image19.png)

**Figure 12**: ajouter un contrôle Literal à la `ItemTemplate` ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-radio-buttons-cs/_static/image20.png))

Ensuite, créez un gestionnaire d’événements pour l’événement `RowCreated` GridView s. L’événement `RowCreated` se déclenche une fois pour chaque ligne ajoutée, que les données soient ou non reliées au GridView. Cela signifie que même sur une publication (postback) lorsque les données sont rechargées à partir de l’état d’affichage, l’événement `RowCreated` se déclenche toujours et c’est la raison pour laquelle nous l’utilisons à la place de `RowDataBound` (qui se déclenche uniquement lorsque les données sont explicitement liées au contrôle Web de données).

Dans ce gestionnaire d’événements, nous voulons uniquement procéder à la réutilisation d’une ligne de données. Pour chaque ligne de données, nous voulons référencer par programmation le contrôle `RadioButtonMarkup` Literal et définir sa propriété `Text` sur le balisage à émettre. Comme le montre le code suivant, la balise émise crée une case d’option dont l’attribut `name` a la valeur `SuppliersGroup`, dont l’attribut `id` a la valeur `RowSelectorX`, où *X* est l’index de la ligne GridView et dont l’attribut `value` a la valeur de l’index de la ligne GridView.

[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample6.cs)]

Quand une ligne GridView est sélectionnée et qu’une publication (postback) se produit, nous sommes intéressés par le `SupplierID` du fournisseur sélectionné. Par conséquent, il est possible que la valeur de chaque case d’option soit le `SupplierID` réel (plutôt que l’index de la ligne GridView). Bien que cela puisse fonctionner dans certaines circonstances, il s’agit d’un risque pour la sécurité d’accepter et de traiter un `SupplierID`. Par exemple, notre GridView affiche uniquement les fournisseurs des États-Unis. Toutefois, si le `SupplierID` est passé directement à partir de la case d’option, qu’est-ce qui empêche un utilisateur espiègle de manipuler la valeur `SupplierID` renvoyée lors de la publication ? En utilisant l’index de ligne comme `value`, puis en obtenant le `SupplierID` à la publication (postback) à partir de la collection de `DataKeys`, nous pouvons garantir que l’utilisateur n’utilise qu’une des valeurs `SupplierID` associées à l’une des lignes de GridView.

Après avoir ajouté ce code de gestionnaire d’événements, prenez une minute pour tester la page dans un navigateur. Tout d’abord, Notez qu’une seule case d’option dans la grille peut être sélectionnée à la fois. Toutefois, lorsque vous sélectionnez une case d’option et que vous cliquez sur l’un des boutons, une publication (postback) se produit et les cases d’option reviennent à leur état initial (c’est-à-dire lors de la publication (postback), la case d’option sélectionnée n’est plus sélectionnée). Pour résoudre ce problème, nous devons augmenter le `RowCreated` gestionnaire d’événements afin qu’il inspecte l’index de case d’option sélectionné envoyé à partir de la publication et ajoute l’attribut `checked="checked"` au balisage émis des correspondances d’index de ligne.

Lorsqu’une publication (postback) se produit, le navigateur renvoie le `name` et `value` de la case d’option sélectionnée. La valeur peut être récupérée par programmation à l’aide de `Request.Form["name"]`. La [propriété`Request.Form`](https://msdn.microsoft.com/library/system.web.httprequest.form.aspx) fournit un [`NameValueCollection`](https://msdn.microsoft.com/library/system.collections.specialized.namevaluecollection.aspx) qui représente les variables de formulaire. Les variables de formulaire sont les noms et les valeurs des champs de formulaire dans la page Web, et sont renvoyés par le navigateur Web chaque fois qu’une publication (postback) est effectuée. Étant donné que l’attribut `name` rendu des cases d’option dans le GridView est `SuppliersGroup`, lorsque la page Web est publiée, le navigateur renvoie `SuppliersGroup=valueOfSelectedRadioButton` au serveur Web (avec les autres champs de formulaire). Vous pouvez ensuite accéder à ces informations à partir de la propriété `Request.Form` à l’aide de : `Request.Form["SuppliersGroup"]`.

Étant donné que nous devrons déterminer l’index de la case d’option sélectionné non seulement dans le gestionnaire d’événements `RowCreated`, mais dans les gestionnaires d’événements `Click` pour les contrôles Web Button, ajoutez une propriété `SuppliersSelectedIndex` à la classe code-behind qui retourne `-1` si aucune case d’option n’a été sélectionnée et l’index sélectionné si l’une des cases d’option est sélectionnée.

[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample7.cs)]

Une fois cette propriété ajoutée, nous savons ajouter le balisage `checked="checked"` dans le gestionnaire d’événements `RowCreated` lorsque `SuppliersSelectedIndex` est égal à `e.Row.RowIndex`. Mettez à jour le gestionnaire d’événements pour inclure cette logique :

[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample8.cs)]

Avec cette modification, la case d’option sélectionnée reste sélectionnée après une publication (postback). Maintenant que nous avons la possibilité de spécifier la case d’option sélectionnée, nous pourrions modifier le comportement de manière à ce que, lors de la première visite de la page, la première case d’option de la ligne de GridView soit sélectionnée (au lieu de ne pas avoir de cases d’option sélectionnées par défaut, qui est la valeur actuelle comportement). Pour que la première case d’option soit sélectionnée par défaut, il vous suffit de remplacer l’instruction `if (SuppliersSelectedIndex == e.Row.RowIndex)` par la suivante : `if (SuppliersSelectedIndex == e.Row.RowIndex || (!Page.IsPostBack && e.Row.RowIndex == 0))`.

À ce stade, nous avons ajouté une colonne de cases d’option groupées au contrôle GridView qui permet la sélection et la mémorisation d’une seule ligne GridView sur les publications (postbacks). Les étapes suivantes permettent d’afficher les produits fournis par le fournisseur sélectionné. À l’étape 4, nous verrons comment rediriger l’utilisateur vers une autre page, en l’envoyant le long du `SupplierID`sélectionné. À l’étape 5, nous verrons comment afficher les produits du fournisseur sélectionnés dans un GridView sur la même page.

> [!NOTE]
> Au lieu d’utiliser un TemplateField (l’objectif de cette longue étape 3), nous pourrions créer une classe de `DataControlField` personnalisée qui restitue l’interface utilisateur et les fonctionnalités appropriées. La [classe`DataControlField`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.aspx) est la classe de base à partir de laquelle les champs BoundField, CheckBoxField, TemplateField et d’autres champs GridView et DetailsView intégrés dérivent. La création d’une classe de `DataControlField` personnalisée signifie que la colonne de cases d’option peut être ajoutée simplement à l’aide de la syntaxe déclarative, et que la réplication de la fonctionnalité sur d’autres pages Web et d’autres applications Web est beaucoup plus facile.

Toutefois, si vous avez déjà créé des contrôles compilés personnalisés dans ASP.NET, vous savez que cela nécessite un peu problèmes et y fait un grand nombre de subtilités et de cas limites qui doivent être gérés avec soin. Par conséquent, nous renoncerons à implémenter une colonne de cases d’option comme classe de `DataControlField` personnalisée pour l’instant et à respecter l’option TemplateField. Nous aurons peut-être la possibilité d’explorer la création, l’utilisation et le déploiement des classes de `DataControlField` personnalisées dans un prochain didacticiel.

## <a name="step-4-displaying-the-selected-supplier-s-products-in-a-separate-page"></a>Étape 4 : affichage des produits du fournisseur sélectionnés dans une page distincte

Une fois que l’utilisateur a sélectionné une ligne GridView, vous devez afficher les produits du fournisseur sélectionnés. Dans certains cas, nous pouvons souhaiter afficher ces produits dans une page distincte, dans d’autres, il peut être préférable de le faire dans la même page. Voyons d’abord comment afficher les produits dans une page distincte. à l’étape 5, nous allons examiner comment ajouter un GridView à `RadioButtonField.aspx` pour afficher les produits du fournisseur sélectionnés.

Il existe actuellement deux contrôles Web Button sur la page `ListProducts` et `SendToProducts`. Lorsque vous cliquez sur le bouton `SendToProducts`, nous voulons envoyer l’utilisateur à `~/Filtering/ProductsForSupplierDetails.aspx`. Cette page a été créée dans le didacticiel de [filtrage maître/détail sur deux pages](../masterdetail/master-detail-filtering-across-two-pages-cs.md) et affiche les produits pour le fournisseur dont le `SupplierID` est transmis via le champ querystring nommé `SupplierID`.

Pour fournir cette fonctionnalité, créez un gestionnaire d’événements pour l’événement `SendToProducts` Button s `Click`. À l’étape 3, nous avons ajouté la propriété `SuppliersSelectedIndex`, qui retourne l’index de la ligne dont la case d’option est sélectionnée. Les `SupplierID` correspondants peuvent être récupérés à partir de la collection de `DataKeys` GridView s et l’utilisateur peut ensuite être envoyé à `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` à l’aide de `Response.Redirect("url")`.

[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample9.cs)]

Ce code fonctionne très bien tant que l’une des cases d’option est sélectionnée dans le contrôle GridView. Si, initialement, le contrôle GridView n’a pas de cases d’option sélectionnées, et que l’utilisateur clique sur le bouton `SendToProducts`, `SuppliersSelectedIndex` est `-1`, ce qui entraîne la levée d’une exception dans la mesure où `-1` est hors de la plage d’index de la collection `DataKeys`. Toutefois, ce n’est pas un problème si vous avez décidé de mettre à jour le gestionnaire d’événements `RowCreated` comme indiqué à l’étape 3 pour que la première case d’option du GridView soit initialement sélectionnée.

Pour prendre en charge une valeur `SuppliersSelectedIndex` de `-1`, ajoutez un contrôle Web Label à la page au-dessus de GridView. Définissez sa propriété `ID` sur `ChooseSupplierMsg`, sa propriété `CssClass` sur `Warning`, ses propriétés `EnableViewState` et `Visible` sur `false`, et sa propriété `Text` pour choisir un fournisseur dans la grille. La classe CSS `Warning` affiche le texte dans une police rouge, italique, en gras, de grande taille et est définie dans `Styles.css`. En définissant les propriétés `EnableViewState` et `Visible` sur `false`, l’étiquette n’est pas restituée, à l’exception uniquement des publications dans lesquelles la propriété Control s `Visible` est définie par programmation sur `true`.

[![ajouter un contrôle Web étiquette au-dessus du contrôle GridView](adding-a-gridview-column-of-radio-buttons-cs/_static/image13.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image21.png)

**Figure 13**: ajouter un contrôle Web étiquette au-dessus du contrôle GridView ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-radio-buttons-cs/_static/image22.png))

Ensuite, augmentez le gestionnaire d’événements `Click` pour afficher l’étiquette `ChooseSupplierMsg` si `SuppliersSelectedIndex` est inférieur à zéro, et redirigez l’utilisateur vers `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` dans le cas contraire.

[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample10.cs)]

Accédez à la page dans un navigateur, puis cliquez sur le bouton `SendToProducts` avant de sélectionner un fournisseur à partir du contrôle GridView. Comme illustré dans la figure 14, le `ChooseSupplierMsg` étiquette s’affiche. Sélectionnez ensuite un fournisseur, puis cliquez sur le bouton `SendToProducts`. Vous obtiendrez ainsi une page qui répertorie les produits fournis par le fournisseur sélectionné. La figure 15 illustre la page de `ProductsForSupplierDetails.aspx` lorsque le fournisseur de Breweries Bigfoot a été sélectionné.

[![l’étiquette ChooseSupplierMsg est affichée si aucun fournisseur n’est sélectionné](adding-a-gridview-column-of-radio-buttons-cs/_static/image14.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image23.png)

**Figure 14**: l’étiquette `ChooseSupplierMsg` s’affiche si aucun fournisseur n’est sélectionné ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-radio-buttons-cs/_static/image24.png))

[![les produits des fournisseurs sélectionnés s’affichent dans ProductsForSupplierDetails. aspx](adding-a-gridview-column-of-radio-buttons-cs/_static/image15.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image25.png)

**Figure 15**: les produits du fournisseur sélectionnés s’affichent dans `ProductsForSupplierDetails.aspx` ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-radio-buttons-cs/_static/image26.png))

## <a name="step-5-displaying-the-selected-supplier-s-products-on-the-same-page"></a>Étape 5 : affichage des produits du fournisseur sélectionnés sur la même page

À l’étape 4, nous avons vu comment envoyer à une autre page Web l’utilisateur pour afficher les produits du fournisseur sélectionnés. Vous pouvez également afficher les produits du fournisseur sélectionnés sur la même page. Pour illustrer cela, nous allons ajouter un autre GridView à `RadioButtonField.aspx` pour afficher les produits du fournisseur sélectionnés.

Étant donné que nous souhaitons que ce contrôle GridView des produits s’affiche une fois qu’un fournisseur a été sélectionné, ajoutez un contrôle Web Panel sous le `Suppliers` GridView, en affectant à son `ID` la valeur `ProductsBySupplierPanel` et à sa propriété `Visible` la valeur `false`. Dans le volet, ajoutez les produits texte pour le fournisseur sélectionné, suivis d’un GridView nommé `ProductsBySupplier`. À partir de la balise active GridView s, choisissez de le lier à un nouvel ObjectDataSource nommé `ProductsBySupplierDataSource`.

[![lier le contrôle GridView ProductsBySupplier à un nouvel ObjectDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image16.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image27.png)

**Figure 16**: lier le `ProductsBySupplier` GridView à un nouvel ObjectDataSource ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-radio-buttons-cs/_static/image28.png))

Ensuite, configurez ObjectDataSource pour utiliser la classe `ProductsBLL`. Étant donné que nous souhaitons uniquement récupérer les produits fournis par le fournisseur sélectionné, spécifiez que l’ObjectDataSource doit appeler la méthode `GetProductsBySupplierID(supplierID)` pour récupérer ses données. Sélectionnez (aucun) dans les listes déroulantes des onglets mettre à jour, insérer et supprimer.

[![configurer ObjectDataSource pour utiliser la méthode GetProductsBySupplierID (RéfFournisseur)](adding-a-gridview-column-of-radio-buttons-cs/_static/image17.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image29.png)

**Figure 17**: configurer ObjectDataSource pour utiliser la méthode `GetProductsBySupplierID(supplierID)` ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-radio-buttons-cs/_static/image30.png))

[![définir les listes déroulantes sur (aucune) dans les onglets mettre à jour, insérer et supprimer](adding-a-gridview-column-of-radio-buttons-cs/_static/image18.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image31.png)

**Figure 18**: définir les listes déroulantes sur (aucune) dans les onglets mettre à jour, insérer et supprimer ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-radio-buttons-cs/_static/image32.png))

Après avoir configuré les onglets Sélectionner, mettre à jour, insérer et supprimer, cliquez sur suivant. Étant donné que la méthode `GetProductsBySupplierID(supplierID)` attend un paramètre d’entrée, l’Assistant Création d’une source de données vous invite à spécifier la source pour la valeur du paramètre s.

Nous disposons de deux options pour spécifier la source de la valeur du paramètre s. Nous pourrions utiliser l’objet de paramètre par défaut et affecter par programmation la valeur de la propriété `SuppliersSelectedIndex` à la propriété Parameter s `DefaultValue` dans le gestionnaire d’événements ObjectDataSource s `Selecting`. Reportez-vous au didacticiel [définition par programmation des valeurs de paramètre de ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md) pour obtenir un actualisateur sur l’assignation par programmation des valeurs aux paramètres de l’ObjectDataSource s.

Vous pouvez également utiliser un ControlParameter et faire référence à la propriété `Suppliers` GridView s [`SelectedValue`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue.aspx) (voir figure 19). La propriété GridView s `SelectedValue` retourne la valeur `DataKey` correspondant à la [propriété`SelectedIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex.aspx). Pour que cette option fonctionne, vous devez définir par programmation la propriété GridView s `SelectedIndex` sur la ligne sélectionnée lorsque l’utilisateur clique sur le bouton `ListProducts`. À titre d’avantage supplémentaire, en définissant la `SelectedIndex`, l’enregistrement sélectionné prend la `SelectedRowStyle` définie dans le thème `DataWebControls` (arrière-plan jaune).

[![utiliser un ControlParameter pour spécifier le paramètre de définition de la source comme source du paramètre](adding-a-gridview-column-of-radio-buttons-cs/_static/image19.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image33.png)

**Figure 19**: utiliser un ControlParameter pour spécifier la valeur SelectedValue de GridView comme source de paramètre ([Cliquer pour afficher l’image en taille réelle](adding-a-gridview-column-of-radio-buttons-cs/_static/image34.png))

À la fin de l’Assistant, Visual Studio ajoute automatiquement des champs pour les champs de données du produit. Supprimez tout sauf les `ProductName`, `CategoryName`et `UnitPrice` BoundFields, puis modifiez les propriétés de `HeaderText` en produit, catégorie et prix. Configurez le `UnitPrice` BoundField afin que sa valeur soit mise en forme en tant que devise. Une fois ces modifications effectuées, les balises déclaratives Panel, GridView et ObjectDataSource doivent ressembler à ce qui suit :

[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample11.aspx)]

Pour effectuer cet exercice, nous devons affecter à la propriété GridView s `SelectedIndex` la valeur `SelectedSuppliersIndex` et à la propriété `Visible` du panneau `ProductsBySupplierPanel` la valeur `true` lorsque l’utilisateur clique sur le bouton `ListProducts`. Pour ce faire, créez un gestionnaire d’événements pour l’événement `ListProducts` Button Web Control s `Click` et ajoutez le code suivant :

[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample12.cs)]

Si un fournisseur n’a pas été sélectionné dans le contrôle GridView, le `ChooseSupplierMsg` étiquette est affiché et le panneau `ProductsBySupplierPanel` masqué. Dans le cas contraire, si un fournisseur a été sélectionné, le `ProductsBySupplierPanel` s’affiche et la propriété GridView s `SelectedIndex` est mise à jour.

La figure 20 montre les résultats une fois que le fournisseur Bigfoot Breweries a été sélectionné et que l’utilisateur a cliqué sur le bouton afficher les produits en page.

[![les produits fournis par Bigfoot Breweries sont répertoriés sur la même page](adding-a-gridview-column-of-radio-buttons-cs/_static/image20.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image35.png)

**Figure 20**: les produits fournis par Bigfoot Breweries sont répertoriés sur la même page ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-radio-buttons-cs/_static/image36.png))

## <a name="summary"></a>Récapitulatif

Comme indiqué dans le didacticiel [maître/détail utilisant un GridView maître sélectionnable avec un DetailView détails](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) , les enregistrements peuvent être sélectionnés à partir d’un GridView à l’aide d’un CommandField dont la propriété `ShowSelectButton` est définie sur `true`. Mais le CommandField affiche ses boutons comme des boutons de commande, des liens ou des images standard. Une autre interface utilisateur de sélection de lignes consiste à fournir une case d’option ou une case à cocher dans chaque ligne GridView. Dans ce didacticiel, nous avons examiné comment ajouter une colonne de cases d’option.

Malheureusement, l’ajout d’une colonne de cases d’option n’est pas aussi simple que possible. Il n’existe aucun RadioButtonField intégré qui peut être ajouté en cliquant sur un bouton, et l’utilisation du contrôle Web RadioButton dans un TemplateField introduit son propre ensemble de problèmes. À la fin, pour fournir une telle interface, nous devons soit créer une classe de `DataControlField` personnalisée, soit recourir à l’injection du code HTML approprié dans un TemplateField pendant l’événement `RowCreated`.

Après avoir exploré l’ajout d’une colonne de cases d’option, nous allons attirer l’attention sur l’ajout d’une colonne de cases à cocher. Avec une colonne de cases à cocher, un utilisateur peut sélectionner une ou plusieurs lignes GridView, puis effectuer une opération sur toutes les lignes sélectionnées (par exemple, en sélectionnant un ensemble d’e-mails à partir d’un client de messagerie Web, puis en choisissant de supprimer tous les e-mails sélectionnés). Dans le didacticiel suivant, nous verrons comment ajouter une telle colonne.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel était David SURU. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](adding-a-gridview-column-of-checkboxes-cs.md)
