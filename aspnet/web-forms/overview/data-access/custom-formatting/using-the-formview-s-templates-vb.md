---
uid: web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-vb
title: Utilisation des modèles du FormView (VB) | Microsoft Docs
author: rick-anderson
description: Contrairement au DetailsView, le FormView n’est pas composé de champs. Au lieu de cela, le FormView est rendu à l’aide de modèles. Dans ce didacticiel, nous allons examiner l’utilisation de la touche F...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 67b25f4c-2823-42b6-b07d-1d650b3fd711
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-vb
msc.type: authoredcontent
ms.openlocfilehash: cafe47cf5766bb14503852ec6e9f305d1e6d426f
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74618365"
---
# <a name="using-the-formviews-templates-vb"></a>Utilisation des modèles du FormView (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_14_VB.exe) ou [Télécharger le PDF](using-the-formview-s-templates-vb/_static/datatutorial14vb1.pdf)

> Contrairement au DetailsView, le FormView n’est pas composé de champs. Au lieu de cela, le FormView est rendu à l’aide de modèles. Dans ce didacticiel, nous allons examiner l’utilisation du contrôle FormView pour présenter un affichage des données moins rigide.

## <a name="introduction"></a>Introduction

Dans les deux derniers didacticiels, nous avons vu comment personnaliser les sorties des contrôles GridView et DetailsView à l’aide de TemplateFields. Les TemplateFields permettent au contenu d’un champ spécifique d’être hautement personnalisé, mais à la fin, GridView et DetailsView ont une apparence plutôt boxy, similaire à la grille. Dans de nombreux scénarios, une mise en page de type grille est idéale, mais à des moments un plus fluide, un affichage moins rigide est nécessaire. Lors de l’affichage d’un seul enregistrement, une telle disposition fluide est possible à l’aide du contrôle FormView.

Contrairement au DetailsView, le FormView n’est pas composé de champs. Vous ne pouvez pas ajouter un BoundField ou un TemplateField à un FormView. Au lieu de cela, le FormView est rendu à l’aide de modèles. Imaginez le FormView comme un contrôle DetailsView qui contient un seul TemplateField. Le FormView prend en charge les modèles suivants :

- `ItemTemplate` utilisé pour restituer l’enregistrement particulier affiché dans le FormView
- `HeaderTemplate` utilisé pour spécifier une ligne d’en-tête facultative
- `FooterTemplate` utilisé pour spécifier une ligne de pied de page facultative
- `EmptyDataTemplate` lorsque le `DataSource` du FormView ne dispose d’aucun enregistrement, le `EmptyDataTemplate` est utilisé à la place de l' `ItemTemplate` pour le rendu du balisage du contrôle
- `PagerTemplate` peut être utilisé pour personnaliser l’interface de pagination pour les FormViews dont la pagination est activée
- `EditItemTemplate` / `InsertItemTemplate` utilisé pour personnaliser l’interface de modification ou insérer une interface pour FormViews qui prend en charge ces fonctionnalités

Dans ce didacticiel, nous allons examiner l’utilisation du contrôle FormView pour présenter un affichage moins rigide des produits. Au lieu d’avoir des champs pour le nom, la catégorie, le fournisseur, etc., le `ItemTemplate` du FormView affiche ces valeurs à l’aide d’une combinaison d’un élément d’en-tête et d’un `<table>` (voir figure 1).

[![le FormView s’arrête hors de la disposition de type grille visible dans le DetailsView](using-the-formview-s-templates-vb/_static/image2.png)](using-the-formview-s-templates-vb/_static/image1.png)

**Figure 1**: le FormView se décompose de la disposition de type grille visible dans le DetailsView ([cliquez pour afficher l’image en plein écran](using-the-formview-s-templates-vb/_static/image3.png))

## <a name="step-1-binding-the-data-to-the-formview"></a>Étape 1 : liaison des données au FormView

Ouvrez la page `FormView.aspx` et faites glisser un contrôle FormView de la boîte à outils vers le concepteur. Lorsque vous ajoutez d’abord le FormView, il apparaît sous la forme d’une zone grise, indiquant qu’une `ItemTemplate` est nécessaire.

[![le FormView ne peut pas être rendu dans le concepteur tant qu’un ItemTemplate n’est pas fourni](using-the-formview-s-templates-vb/_static/image5.png)](using-the-formview-s-templates-vb/_static/image4.png)

**Figure 2**: le FormView ne peut pas être rendu dans le concepteur tant qu’un `ItemTemplate` n’est pas fourni ([cliquez pour afficher l’image en taille réelle](using-the-formview-s-templates-vb/_static/image6.png))

La `ItemTemplate` peut être créée manuellement (par le biais de la syntaxe déclarative) ou être créée automatiquement en liant le contrôle FormView à un contrôle de source de données par le biais du concepteur. Ce `ItemTemplate` créé automatiquement contient du code HTML qui répertorie le nom de chaque champ et un contrôle Label dont la propriété `Text` est liée à la valeur du champ. Cette approche crée également automatiquement un `InsertItemTemplate` et `EditItemTemplate`, qui sont tous deux remplis avec des contrôles d’entrée pour chacun des champs de données retournés par le contrôle de source de données.

Si vous souhaitez créer automatiquement le modèle, à partir de la balise active FormView, ajoutez un nouveau contrôle ObjectDataSource qui appelle la méthode `GetProducts()` de la classe `ProductsBLL`. Cette opération crée un FormView avec un `ItemTemplate`, `InsertItemTemplate`et `EditItemTemplate`. À partir de la vue source, supprimez le `InsertItemTemplate` et `EditItemTemplate` puisque nous ne nous intéressons pas à la création d’un FormView qui prend encore en charge la modification ou l’insertion. Ensuite, effacez le balisage dans le `ItemTemplate` pour que nous ayons une ardoise propre à utiliser.

Si vous préférez créer le `ItemTemplate` manuellement, vous pouvez ajouter et configurer l’ObjectDataSource en le faisant glisser de la boîte à outils vers le concepteur. Toutefois, ne définissez pas la source de données du FormView à partir du concepteur. Au lieu de cela, accédez à la vue source et définissez manuellement la propriété `DataSourceID` du FormView sur la valeur `ID` de l’ObjectDataSource. Ajoutez ensuite manuellement le `ItemTemplate`.

Quelle que soit l’approche que vous avez décidé de prendre, à ce stade, le balisage déclaratif de votre FormView devrait ressembler à ceci :

[!code-aspx[Main](using-the-formview-s-templates-vb/samples/sample1.aspx)]

Prenez un moment pour activer la case à cocher Activer la pagination dans la balise active du FormView. Cette opération ajoute l’attribut `AllowPaging="True"` à la syntaxe déclarative du FormView. En outre, affectez la valeur false à la propriété `EnableViewState`.

## <a name="step-2-defining-theitemtemplates-markup"></a>Étape 2 : définition du balisage de l'`ItemTemplate`

Avec le FormView lié au contrôle ObjectDataSource et configuré pour prendre en charge la pagination, nous sommes prêts à spécifier le contenu pour le `ItemTemplate`. Pour ce didacticiel, nous allons afficher le nom du produit dans un en-tête de `<h3>`. Ensuite, nous allons utiliser un `<table>` HTML pour afficher les propriétés de produit restantes dans une table à quatre colonnes où la première et la troisième colonne répertorient les noms de propriété, et la deuxième et la quatrième listent leurs valeurs.

Ce balisage peut être entré dans via l’interface de modification de modèle du FormView dans le concepteur ou entré manuellement via la syntaxe déclarative. Lorsque je travaille avec des modèles, je trouve généralement plus rapide de travailler directement avec la syntaxe déclarative, mais n’hésitez pas à utiliser la technique qui vous convient le mieux.

Le balisage suivant montre le balisage déclaratif FormView après la fin de la structure du `ItemTemplate`:

[!code-aspx[Main](using-the-formview-s-templates-vb/samples/sample2.aspx)]

Notez que la syntaxe DataBinding-`<%# Eval("ProductName") %>`, par exemple, peut être injectée directement dans la sortie du modèle. Autrement dit, il ne doit pas être assigné à la propriété `Text` d’un contrôle Label. Par exemple, la valeur `ProductName` s’affiche dans un élément `<h3>` à l’aide de `<h3><%# Eval("ProductName") %></h3>`, qui pour le produit que la fonction chai s’affiche comme `<h3>Chai</h3>`.

Les classes CSS `ProductPropertyLabel` et `ProductPropertyValue` sont utilisées pour spécifier le style des noms et valeurs des propriétés de produit dans le `<table>`. Ces classes CSS sont définies dans `Styles.css` et les noms de propriété sont en gras et alignés à droite, et ajoutent une marge droite aux valeurs de propriété.

Dans la mesure où aucun CheckBoxFields n’est disponible avec le contrôle FormView, afin d’afficher la valeur `Discontinued` sous la forme d’une case à cocher, nous devons ajouter notre propre contrôle CheckBox. La propriété `Enabled` a la valeur false, la rend en lecture seule et la propriété `Checked` de la case à cocher est liée à la valeur du champ de données `Discontinued`.

Une fois l' `ItemTemplate` terminée, les informations sur le produit s’affichent de manière plus fluide. Comparez la sortie DetailsView du dernier didacticiel (figure 3) avec la sortie générée par le FormView dans ce didacticiel (figure 4).

[![la sortie de DetailsView rigide](using-the-formview-s-templates-vb/_static/image8.png)](using-the-formview-s-templates-vb/_static/image7.png)

**Figure 3**: sortie rigide DetailsView ([cliquez pour afficher l’image en taille réelle](using-the-formview-s-templates-vb/_static/image9.png))

[![la sortie de la FormView fluide](using-the-formview-s-templates-vb/_static/image11.png)](using-the-formview-s-templates-vb/_static/image10.png)

**Figure 4**: sortie du FormView fluide ([cliquez pour afficher l’image en taille réelle](using-the-formview-s-templates-vb/_static/image12.png))

## <a name="summary"></a>Récapitulatif

Alors que la sortie des contrôles GridView et DetailsView peut être personnalisée à l’aide de TemplateFields, ils présentent tous deux toujours leurs données dans un format boxy de grille. Dans les cas où un seul enregistrement doit être affiché à l’aide d’une disposition moins rigide, le FormView est un choix idéal. À l’instar du contrôle DetailsView, le FormView restitue un enregistrement unique à partir de son `DataSource`, mais contrairement à DetailsView, il est composé uniquement de modèles et ne prend pas en charge les champs.

Comme nous l’avons vu dans ce didacticiel, le FormView offre une mise en page plus souple lors de l’affichage d’un seul enregistrement. Dans les prochains didacticiels, nous examinerons les contrôles DataList et Repeater, qui offrent le même niveau de flexibilité que le FormsView, mais peuvent afficher plusieurs enregistrements (comme le GridView).

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de Lead pour ce didacticiel était E.R. Gilmore. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](using-templatefields-in-the-detailsview-control-vb.md)
> [Suivant](displaying-summary-information-in-the-gridview-s-footer-vb.md)
