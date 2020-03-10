---
uid: web-forms/overview/data-access/basic-reporting/declarative-parameters-cs
title: Paramètres déclaratifs (C#) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons expliquer comment utiliser un paramètre défini sur une valeur codée en dur pour sélectionner les données à afficher dans un contrôle DetailsView.
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 603c9bd3-b895-4ec6-853b-0c81ff36d580
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/declarative-parameters-cs
msc.type: authoredcontent
ms.openlocfilehash: 87c8cfe064abc536e6015b0e553618981da9fefe
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78597271"
---
# <a name="declarative-parameters-c"></a>Paramètres déclaratifs (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_5_CS.exe) ou [Télécharger le PDF](declarative-parameters-cs/_static/datatutorial05cs1.pdf)

> Dans ce didacticiel, nous allons expliquer comment utiliser un paramètre défini sur une valeur codée en dur pour sélectionner les données à afficher dans un contrôle DetailsView.

## <a name="introduction"></a>Introduction

Dans le [dernier didacticiel](displaying-data-with-the-objectdatasource-cs.md) , nous avons examiné l’affichage des données avec les contrôles GridView, DetailsView et FormView liés à un contrôle ObjectDataSource qui a appelé la méthode `GetProducts()` à partir de la classe `ProductsBLL`. La méthode `GetProducts()` retourne un DataTable fortement typé rempli avec tous les enregistrements de la table `Products` de la base de données Northwind. La classe `ProductsBLL` contient des méthodes supplémentaires pour retourner uniquement des sous-ensembles des produits-`GetProductByProductID(productID)`, `GetProductsByCategoryID(categoryID)`et `GetProductsBySupplierID(supplierID)`. Ces trois méthodes attendent un paramètre d’entrée indiquant comment filtrer les informations sur les produits retournés.

ObjectDataSource peut être utilisé pour appeler des méthodes qui attendent des paramètres d’entrée, mais pour ce faire, nous devons spécifier l’origine des valeurs de ces paramètres. Les valeurs de paramètre peuvent être codées en dur ou provenir de diverses sources dynamiques, notamment les valeurs QueryString, les variables de session, la valeur de propriété d’un contrôle Web sur la page, ou d’autres.

Pour ce didacticiel, commençons par illustrer l’utilisation d’un paramètre défini sur une valeur codée en dur. Plus précisément, nous examinerons l’ajout d’un DetailsView à la page qui affiche des informations sur un produit spécifique, à savoir chef Gumbo Mix, qui a une `ProductID` de 5. Ensuite, nous verrons comment définir la valeur de paramètre en fonction d’un contrôle Web. En particulier, nous allons utiliser une zone de texte pour permettre à l’utilisateur de taper un pays, après quoi il pourra cliquer sur un bouton pour afficher la liste des fournisseurs qui résident dans ce pays.

## <a name="using-a-hard-coded-parameter-value"></a>Utilisation d’une valeur de paramètre codée en dur

Pour le premier exemple, commencez par ajouter un contrôle DetailsView à la page `DeclarativeParams.aspx` dans le dossier `BasicReporting`. À partir de la balise active du contrôle DetailsView, sélectionnez &lt;nouvelle source de données&gt; dans la liste déroulante et choisissez d’ajouter un ObjectDataSource.

[![ajouter un ObjectDataSource à la page](declarative-parameters-cs/_static/image2.png)](declarative-parameters-cs/_static/image1.png)

**Figure 1**: ajouter un ObjectDataSource à la page ([cliquez pour afficher l’image en taille réelle](declarative-parameters-cs/_static/image3.png))

L’assistant choisir la source de données s’affiche automatiquement. Sélectionnez la classe `ProductsBLL` dans le premier écran de l’Assistant.

[![sélectionnez la classe ProductsBLL](declarative-parameters-cs/_static/image5.png)](declarative-parameters-cs/_static/image4.png)

**Figure 2**: sélectionner la classe de `ProductsBLL` ([cliquez pour afficher l’image en taille réelle](declarative-parameters-cs/_static/image6.png))

Étant donné que nous souhaitons afficher des informations sur un produit particulier, nous souhaitons utiliser la méthode `GetProductByProductID(productID)`.

[![choisir la méthode GetProductByProductID (productID)](declarative-parameters-cs/_static/image8.png)](declarative-parameters-cs/_static/image7.png)

**Figure 3**: choisir la méthode `GetProductByProductID(productID)` ([cliquez pour afficher l’image en taille réelle](declarative-parameters-cs/_static/image9.png))

Étant donné que la méthode que nous avons sélectionnée inclut un paramètre, il y a un autre écran pour l’Assistant, où nous sommes invités à définir la valeur à utiliser pour le paramètre. La liste à gauche montre tous les paramètres de la méthode sélectionnée. Pour `GetProductByProductID(productID)` il n’existe qu’une seule `productID`. À droite, nous pouvons spécifier la valeur du paramètre sélectionné. La liste déroulante source de paramètres énumère les différentes sources possibles pour la valeur de paramètre. Étant donné que nous voulons spécifier une valeur codée en dur de 5 pour le paramètre `productID`, laissez la source de paramètre la valeur None et entrez 5 dans la zone de texte DefaultValue.

[![une valeur de paramètre codée en dur de 5 sera utilisée pour le paramètre productID](declarative-parameters-cs/_static/image11.png)](declarative-parameters-cs/_static/image10.png)

**Figure 4**: une valeur de paramètre codée en dur de 5 sera utilisée pour le paramètre `productID` ([cliquez pour afficher l’image en taille réelle](declarative-parameters-cs/_static/image12.png))

À l’issue de l’exécution de l’Assistant Configuration de la source de données, le balisage déclaratif du contrôle ObjectDataSource comprend un objet `Parameter` dans la collection `SelectParameters` pour chacun des paramètres d’entrée attendus par la méthode définie dans la propriété `SelectMethod`. Étant donné que la méthode que nous utilisons dans cet exemple s’attend à un seul paramètre d’entrée, `parameterID`, il n’y a qu’une seule entrée ici. La collection `SelectParameters` peut contenir toute classe qui dérive de la classe `Parameter` de l’espace de noms `System.Web.UI.WebControls`. Pour les valeurs de paramètre codées en dur, la classe de `Parameter` de base est utilisée, mais pour les autres options de source de paramètres, une classe de `Parameter` dérivée est utilisée ; vous pouvez également créer vos propres [types de paramètres personnalisés](http://www.leftslipper.com/ShowFaq.aspx?FaqId=11), si nécessaire.

[!code-aspx[Main](declarative-parameters-cs/samples/sample1.aspx)]

> [!NOTE]
> Si vous effectuez la procédure suivante sur votre ordinateur, le balisage déclaratif que vous voyez à ce stade peut inclure des valeurs pour les propriétés `InsertMethod`, `UpdateMethod`et `DeleteMethod`, ainsi que des `DeleteParameters`. L’assistant choisir la source de données de ObjectDataSource spécifie automatiquement les méthodes de l' `ProductBLL` à utiliser pour l’insertion, la mise à jour et la suppression. par conséquent, à moins que vous n’ayez explicitement effacé ces méthodes, elles sont incluses dans le balisage ci-dessus.

Quand vous visitez cette page, le contrôle Web de données appelle la méthode d’ObjectDataSource `Select`, qui appelle la méthode `GetProductByProductID(productID)` de la classe `ProductsBLL` à l’aide de la valeur codée en dur de 5 pour le paramètre d’entrée `productID`. La méthode retourne un objet `ProductDataTable` fortement typé qui contient une seule ligne avec des informations sur la combinaison Gumbo de chef Anton (le produit avec `ProductID` 5).

[![plus d’informations sur la combinaison Gumbo de chef Anton sont affichées.](declarative-parameters-cs/_static/image14.png)](declarative-parameters-cs/_static/image13.png)

**Figure 5**: affichage des informations sur la combinaison Gumbo de chef Anton ([cliquez pour afficher l’image en taille réelle](declarative-parameters-cs/_static/image15.png))

## <a name="setting-the-parameter-value-to-the-property-value-of-a-web-control"></a>Définition de la valeur de paramètre sur la valeur de propriété d’un contrôle Web

Les valeurs de paramètre de ObjectDataSource peuvent également être définies en fonction de la valeur d’un contrôle Web sur la page. Pour illustrer cela, nous allons avoir un GridView qui répertorie tous les fournisseurs situés dans un pays spécifié par l’utilisateur. Pour ce faire, commencez par ajouter une zone de texte à la page dans laquelle l’utilisateur peut entrer un nom de pays. Affectez à la propriété `ID` de ce contrôle TextBox la valeur `CountryName`. Ajoutez également un contrôle Web Button.

[![ajouter une zone de texte à la page avec l’ID CountryName](declarative-parameters-cs/_static/image17.png)](declarative-parameters-cs/_static/image16.png)

**Figure 6**: ajouter une zone de texte à la Page avec `ID` `CountryName` ([cliquez pour afficher l’image en taille réelle](declarative-parameters-cs/_static/image18.png))

Ensuite, ajoutez un contrôle GridView à la page et, à partir de la balise active, choisissez d’ajouter un nouvel ObjectDataSource. Dans la mesure où nous souhaitons afficher des informations sur les fournisseurs, sélectionnez la classe `SuppliersBLL` dans le premier écran de l’Assistant. Dans le deuxième écran, choisissez la méthode `GetSuppliersByCountry(country)`.

[![choisir la méthode GetSuppliersByCountry (Country)](declarative-parameters-cs/_static/image20.png)](declarative-parameters-cs/_static/image19.png)

**Figure 7**: choisir la méthode de `GetSuppliersByCountry(country)` ([cliquez pour afficher l’image en taille réelle](declarative-parameters-cs/_static/image21.png))

Étant donné que la méthode `GetSuppliersByCountry(country)` possède un paramètre d’entrée, l’Assistant inclut une nouvelle fois un écran final pour le choix de la valeur de paramètre. Cette fois-ci, définissez la source du paramètre sur Control. La liste déroulante ControlID est renseignée avec les noms des contrôles sur la page ; Sélectionnez le contrôle `CountryName` dans la liste. Lorsque la page est visitée pour la première fois, la zone de texte `CountryName` est vide, de sorte qu’aucun résultat n’est retourné et rien ne s’affiche. Si vous souhaitez afficher des résultats par défaut, définissez la zone de texte DefaultValue en conséquence.

[![définir la valeur du paramètre sur la valeur du contrôle CountryName](declarative-parameters-cs/_static/image23.png)](declarative-parameters-cs/_static/image22.png)

**Figure 8**: définir la valeur du paramètre sur la valeur de contrôle `CountryName` ([cliquez pour afficher l’image en taille réelle](declarative-parameters-cs/_static/image24.png))

Le balisage déclaratif de ObjectDataSource diffère légèrement de notre premier exemple, à l’aide d’un [ControlParameter](https://msdn.microsoft.com/library/system.web.ui.webcontrols.controlparameter.aspx) au lieu de l’objet `Parameter` standard. Un `ControlParameter` possède des propriétés supplémentaires pour spécifier la `ID` du contrôle Web et la valeur de propriété à utiliser pour le paramètre (`PropertyName`). L’Assistant Configuration de la source de données était suffisamment intelligent pour déterminer que, pour une zone de texte, nous souhaitons probablement utiliser la propriété `Text` pour la valeur de paramètre. Toutefois, si vous souhaitez utiliser une valeur de propriété différente du contrôle Web, vous pouvez modifier la valeur de `PropertyName` ici ou en cliquant sur le lien « Afficher les propriétés avancées » dans l’Assistant.

[!code-aspx[Main](declarative-parameters-cs/samples/sample2.aspx)]

Lorsque vous visitez la page pour la première fois, la zone de texte `CountryName` est vide. La méthode de `Select` de l’ObjectDataSource est toujours appelée par le GridView, mais une valeur de `null` est passée à la méthode `GetSuppliersByCountry(country)`. Le TableAdapter convertit le `null` en une valeur de `NULL` de base de données (`DBNull.Value`), mais la requête utilisée par la méthode `GetSuppliersByCountry(country)` est écrite de telle sorte qu’elle ne retourne aucune valeur quand une valeur `NULL` est spécifiée pour le paramètre `@CategoryID`. En bref, aucun fournisseur n’est retourné.

Toutefois, une fois que le visiteur entre dans un pays, et clique sur le bouton afficher les fournisseurs pour déclencher une publication, la méthode de `Select` ObjectDataSource est réinterrogée, passant la valeur `Text` du contrôle TextBox comme paramètre `country`.

[![ces fournisseurs à partir du Canada sont affichés](declarative-parameters-cs/_static/image26.png)](declarative-parameters-cs/_static/image25.png)

**Figure 9**: les fournisseurs du Canada sont affichés ([cliquez pour afficher l’image en taille réelle](declarative-parameters-cs/_static/image27.png))

## <a name="showing-all-suppliers-by-default"></a>Montrer tous les fournisseurs par défaut

Au lieu d’afficher aucun des fournisseurs lorsque vous affichez la page pour la première fois, nous pouvons afficher *tous les* fournisseurs dans un premier temps, ce qui permet à l’utilisateur de faire défiler la liste en entrant un nom de pays dans la zone de texte. Lorsque la zone de texte est vide, la méthode `GetSuppliersByCountry(country)` de la classe `SuppliersBLL` est passée dans une valeur `null` pour son paramètre d’entrée *`country`* . Cette valeur `null` est ensuite passée dans la méthode `GetSupplierByCountry(country)` de la couche DAL, où elle est traduite en valeur de `NULL` de base de données pour le paramètre `@Country` dans la requête suivante :

[!code-sql[Main](declarative-parameters-cs/samples/sample3.sql)]

L’expression `Country = NULL` retourne toujours false, même pour les enregistrements dont la colonne `Country` a une valeur `NULL` ; par conséquent, aucun enregistrement n’est retourné.

Pour retourner *tous les* fournisseurs lorsque la zone de texte Country est vide, nous pouvons augmenter la méthode `GetSuppliersByCountry(country)` dans la couche BLL pour appeler la méthode `GetSuppliers()` quand son paramètre country est `null` et appeler la méthode `GetSuppliersByCountry(country)` de la couche DAL dans le cas contraire. Cela aura pour effet de retourner tous les fournisseurs quand aucun pays n’est spécifié et le sous-ensemble approprié de fournisseurs lorsque le paramètre Country est inclus.

Remplacez la méthode `GetSuppliersByCountry(country)` de la classe `SuppliersBLL` par la méthode suivante :

[!code-csharp[Main](declarative-parameters-cs/samples/sample4.cs)]

Avec cette modification, la page `DeclarativeParams.aspx` affiche tous les fournisseurs lors de la première visite (ou chaque fois que la zone de texte `CountryName` est vide).

[![tous les fournisseurs sont maintenant affichés par défaut](declarative-parameters-cs/_static/image29.png)](declarative-parameters-cs/_static/image28.png)

**Figure 10**: tous les fournisseurs sont maintenant affichés par défaut ([cliquez pour afficher l’image en taille réelle](declarative-parameters-cs/_static/image30.png))

## <a name="summary"></a>Récapitulatif

Pour pouvoir utiliser des méthodes avec des paramètres d’entrée, nous devons spécifier les valeurs des paramètres dans la collection de `SelectParameters` ObjectDataSource. Les différents types de paramètres permettent d’obtenir la valeur de paramètre à partir de différentes sources. Le type de paramètre par défaut utilise une valeur codée en dur, mais tout aussi facilement (et sans ligne de code), des valeurs de paramètre peuvent être obtenues à partir de la QueryString, des variables de session, des cookies et même des valeurs entrées par l’utilisateur dans les contrôles Web de la page.

Les exemples que nous avons examinés dans ce didacticiel illustrent l’utilisation de valeurs de paramètre déclaratives. Toutefois, il peut arriver que vous deviez utiliser une source de paramètre qui n’est pas disponible, telle que la date et l’heure actuelles ou, si notre site utilisait l’appartenance, l’ID d’utilisateur du visiteur. Dans de tels scénarios, nous pouvons définir les valeurs de paramètre par programmation avant l’ObjectDataSource qui appelle la méthode de l’objet sous-jacent. Nous verrons comment effectuer cette procédure dans le [prochain didacticiel](programmatically-setting-the-objectdatasource-s-parameter-values-cs.md).

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel était Hilton Giesenow. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](displaying-data-with-the-objectdatasource-cs.md)
> [Suivant](programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)
