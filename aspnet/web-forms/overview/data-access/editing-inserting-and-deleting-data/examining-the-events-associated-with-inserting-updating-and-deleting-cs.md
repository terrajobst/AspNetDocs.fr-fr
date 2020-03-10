---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs
title: Examen des événements associés à l’insertion, à la mise à jourC#et à la suppression () | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons examiner l’utilisation des événements qui se produisent avant, pendant et après une opération d’insertion, de mise à jour ou de suppression d’un contrôle Web de données ASP.NET. W...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: dab291a0-a8b5-46fa-9dd8-3d35b249201f
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: a8c1388b73524a8bb918b67aa265db894c07636f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78609150"
---
# <a name="examining-the-events-associated-with-inserting-updating-and-deleting-c"></a>Examen des événements associés à l’insertion, à la mise à jour et à la suppression (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_17_CS.exe) ou [Télécharger le PDF](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/datatutorial17cs1.pdf)

> Dans ce didacticiel, nous allons examiner l’utilisation des événements qui se produisent avant, pendant et après une opération d’insertion, de mise à jour ou de suppression d’un contrôle Web de données ASP.NET. Nous verrons également comment personnaliser l’interface de modification pour mettre à jour uniquement un sous-ensemble des champs de produit.

## <a name="introduction"></a>Introduction

Lors de l’utilisation des fonctionnalités intégrées d’insertion, de modification ou de suppression des contrôles GridView, DetailsView ou FormView, une série d’étapes s’écoulent lorsque l’utilisateur final termine le processus d’ajout d’un nouvel enregistrement ou de mise à jour ou de suppression d’un enregistrement existant. Comme nous l’avons vu dans le [didacticiel précédent](an-overview-of-inserting-updating-and-deleting-data-cs.md), quand une ligne est modifiée dans le contrôle GridView, le bouton modifier est remplacé par les boutons mettre à jour et annuler et les BoundFields s’activent dans les zones de texte. Une fois que l’utilisateur final a mis à jour les données et cliqué sur mettre à jour, les étapes suivantes sont effectuées lors de la publication :

1. Le GridView remplit le `UpdateParameters` de l’ObjectDataSource avec le ou les champs d’identification uniques de l’enregistrement modifié (via la propriété `DataKeyNames`), ainsi que les valeurs entrées par l’utilisateur.
2. Le contrôle GridView appelle la méthode de `Update()` ObjectDataSource, qui à son tour appelle la méthode appropriée dans l’objet sous-jacent (`ProductsDAL.UpdateProduct`, dans le didacticiel précédent)
3. Les données sous-jacentes, qui incluent désormais les modifications mises à jour, sont reliées au GridView

Au cours de cette séquence d’étapes, un certain nombre d’événements se déclenchent, ce qui nous permet de créer des gestionnaires d’événements pour ajouter une logique personnalisée, le cas échéant. Par exemple, avant l’étape 1, l’événement `RowUpdating` de GridView se déclenche. À ce stade, vous pouvez annuler la demande de mise à jour en cas d’erreur de validation. Lorsque la méthode `Update()` est appelée, l’événement `Updating` de l’ObjectDataSource se déclenche, ce qui vous donne la possibilité d’ajouter ou de personnaliser les valeurs d’un `UpdateParameters`. Une fois l’exécution de la méthode de l’objet sous-jacent de ObjectDataSource terminée, l’événement de `Updated` de l’ObjectDataSource est déclenché. Un gestionnaire d’événements pour l’événement `Updated` peut inspecter les détails sur l’opération de mise à jour, comme le nombre de lignes affectées et si une exception s’est produite ou non. Enfin, après l’étape 2, l’événement `RowUpdated` de GridView se déclenche ; un gestionnaire d’événements pour cet événement peut examiner des informations supplémentaires sur l’opération de mise à jour qui vient d’être effectuée.

La figure 1 illustre cette série d’événements et d’étapes lors de la mise à jour d’un GridView. Le modèle d’événement de la figure 1 n’est pas unique pour la mise à jour avec un GridView. L’insertion, la mise à jour ou la suppression de données à partir de GridView, DetailsView ou FormView précipite la même séquence d’événements de pré-et de dernier niveau pour le contrôle Web de données et l’ObjectDataSource.

[![une série d’événements et de pré-événements se déclenche lors de la mise à jour de données dans un GridView](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image2.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image1.png)

**Figure 1**: une série d’événements et de pré-événements se déclenche lors de la mise à jour de données dans un GridView ([cliquez pour afficher l’image en taille réelle](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image3.png))

Dans ce didacticiel, nous allons examiner l’utilisation de ces événements pour étendre les fonctionnalités intégrées d’insertion, de mise à jour et de suppression des contrôles Web de données ASP.NET. Nous verrons également comment personnaliser l’interface de modification pour mettre à jour uniquement un sous-ensemble des champs de produit.

## <a name="step-1-updating-a-productsproductnameandunitpricefields"></a>Étape 1 : mise à jour des champs`ProductName`et`UnitPrice`d’un produit

Dans les interfaces de modification du didacticiel précédent, *tous les* champs de produit qui n’étaient pas en lecture seule devaient être inclus. Si nous devions supprimer un champ de l' `QuantityPerUnit` GridView-disons-lors de la mise à jour des données, le contrôle Web de données ne définissait pas la valeur de la `UpdateParameters` de l’ObjectDataSource `QuantityPerUnit`. L’ObjectDataSource transmettra ensuite une valeur `null` dans la méthode BLL (Business Logic Layer) `UpdateProduct`, ce qui modifierait la `QuantityPerUnit` colonne de l’enregistrement de base de données modifié en valeur `NULL`. De même, si un champ obligatoire, tel que `ProductName`, est supprimé de l’interface de modification, la mise à jour échoue avec l’exception « la*colonne «ProductName » n’autorise pas les valeurs NULL*». La raison de ce comportement est que l’ObjectDataSource a été configuré pour appeler la méthode `UpdateProduct` de la classe `ProductsBLL`, qui attendait un paramètre d’entrée pour chacun des champs de produit. Par conséquent, la collection d' `UpdateParameters` de l’ObjectDataSource contenait un paramètre pour chacun des paramètres d’entrée de la méthode.

Si nous souhaitons fournir un contrôle Web de données qui permet à l’utilisateur final de mettre à jour uniquement un sous-ensemble de champs, nous devons soit définir par programmation les valeurs manquantes de `UpdateParameters` dans le gestionnaire d’événements `Updating` de l’ObjectDataSource, soit créer et appeler une méthode BLL qui attend uniquement un sous-ensemble des champs. Étudions cette dernière approche.

En particulier, nous allons créer une page qui affiche uniquement les champs `ProductName` et `UnitPrice` dans un GridView modifiable. L’interface de modification de ce GridView permettra uniquement à l’utilisateur de mettre à jour les deux champs affichés, `ProductName` et `UnitPrice`. Étant donné que cette interface de modification fournit uniquement un sous-ensemble des champs d’un produit, nous devons soit créer un ObjectDataSource qui utilise la méthode de `UpdateProduct` de la couche BLL existante et avoir les valeurs de champ de produit manquantes définies par programme dans son gestionnaire d’événements `Updating`, soit créer une nouvelle méthode BLL qui attend uniquement le sous-ensemble de champs définis dans le GridView. Pour ce didacticiel, nous allons utiliser la dernière option et créer une surcharge de la méthode `UpdateProduct`, qui accepte uniquement trois paramètres d’entrée : `productName`, `unitPrice`et `productID`:

[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample1.cs)]

À l’instar de la méthode `UpdateProduct` d’origine, cette surcharge commence par vérifier s’il existe un produit dans la base de données avec le `ProductID`spécifié. Si ce n’est pas le cas, elle retourne `false`, indiquant que la demande de mise à jour des informations sur le produit a échoué. Dans le cas contraire, il met à jour les champs `ProductName` et `UnitPrice` de l’enregistrement de produit existant en conséquence et valide la mise à jour en appelant la méthode `Update()` du TableAdapter, en passant l’instance `ProductsRow`.

Grâce à cet ajout à notre classe `ProductsBLL`, nous sommes prêts à créer l’interface GridView simplifiée. Ouvrez le `DataModificationEvents.aspx` dans le dossier `EditInsertDelete` et ajoutez un contrôle GridView à la page. Créez un ObjectDataSource et configurez-le de façon à ce qu’il utilise la classe `ProductsBLL` avec son mappage de méthode `Select()` à `GetProducts` et son `Update()` méthode de mappage à la surcharge `UpdateProduct` qui accepte uniquement les paramètres d’entrée `productName`, `unitPrice`et `productID`. La figure 2 illustre l’Assistant Création d’une source de données lors du mappage de la méthode d' `Update()` ObjectDataSource à la nouvelle surcharge de méthode `UpdateProduct` de la classe `ProductsBLL`.

[![mapper la méthode Update () de ObjectDataSource à la nouvelle surcharge UpdateProduct](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image5.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image4.png)

**Figure 2**: mapper la méthode de `Update()` ObjectDataSource à la nouvelle surcharge de `UpdateProduct` ([cliquez pour afficher l’image en taille réelle](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image6.png))

Dans la mesure où notre exemple a uniquement besoin de pouvoir modifier les données, mais pas d’insérer ou de supprimer des enregistrements, prenez un moment pour indiquer explicitement que les méthodes d' `Insert()` et de `Delete()` de l’ObjectDataSource ne doivent pas être mappées à l’une des méthodes de la classe `ProductsBLL` en accédant aux onglets insérer et supprimer et en choisissant (aucun) dans la liste déroulante.

[![sélectionnez (aucun) dans la liste déroulante des onglets insérer et supprimer](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image8.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image7.png)

**Figure 3**: choisir (aucun) dans la liste déroulante des onglets insérer et supprimer ([cliquez pour afficher l’image en taille réelle](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image9.png))

À la fin de cet Assistant, activez la case à cocher Activer la modification à partir de la balise active de GridView.

Une fois l’exécution de l’Assistant Création de source de données terminée et la liaison à GridView, Visual Studio a créé la syntaxe déclarative pour les deux contrôles. Accédez à la vue source pour inspecter le balisage déclaratif de ObjectDataSource, comme indiqué ci-dessous :

[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample2.aspx)]

Étant donné qu’il n’existe aucun mappage pour les méthodes `Insert()` et `Delete()` de l’ObjectDataSource, il n’y a aucune section `InsertParameters` ou `DeleteParameters`. En outre, étant donné que la méthode `Update()` est mappée à la surcharge de méthode `UpdateProduct` qui accepte uniquement trois paramètres d’entrée, la section `UpdateParameters` contient seulement trois instances `Parameter`.

Notez que la propriété de `OldValuesParameterFormatString` ObjectDataSource est définie sur `original_{0}`. Cette propriété est définie automatiquement par Visual Studio lors de l’utilisation de l’Assistant Configuration de la source de données. Toutefois, étant donné que nos méthodes BLL ne s’attendent pas à ce que la valeur d’origine `ProductID` soit passée, supprimez cette assignation de propriété de la syntaxe déclarative de ObjectDataSource.

> [!NOTE]
> Si vous supprimez simplement la valeur de la propriété `OldValuesParameterFormatString` de la Fenêtre Propriétés dans le Mode Création, la propriété existera toujours dans la syntaxe déclarative, mais sera définie sur une chaîne vide. Supprimez la propriété de la syntaxe déclarative ou, à partir de la Fenêtre Propriétés, affectez la valeur par défaut, `{0}`.

Alors que ObjectDataSource n’a `UpdateParameters` pour le nom, le prix et l’ID du produit, Visual Studio a ajouté un BoundField ou CheckBoxField dans le GridView pour chacun des champs du produit.

[![le GridView contient un BoundField ou CheckBoxField pour chacun des champs du produit](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image11.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image10.png)

**Figure 4**: le GridView contient un BoundField ou un CheckBoxField pour chacun des champs du produit ([cliquez pour afficher l’image en taille réelle](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image12.png))

Lorsque l’utilisateur final modifie un produit et clique sur son bouton mettre à jour, le contrôle GridView énumère les champs qui n’étaient pas en lecture seule. Il définit ensuite la valeur du paramètre correspondant dans la collection de `UpdateParameters` de l’ObjectDataSource sur la valeur entrée par l’utilisateur. S’il n’existe pas de paramètre correspondant, le contrôle GridView en ajoute un à la collection. Par conséquent, si notre GridView contient BoundFields et CheckBoxFields pour tous les champs du produit, l’ObjectDataSource finit par appeler la surcharge `UpdateProduct` qui prend tous ces paramètres, bien que le balisage déclaratif de ObjectDataSource spécifie uniquement trois paramètres d’entrée (voir figure 5). De même, s’il existe une combinaison de champs de produit non en lecture seule dans le GridView qui ne correspond pas aux paramètres d’entrée d’une surcharge `UpdateProduct`, une exception est levée lors de la tentative de mise à jour.

[![le contrôle GridView ajoutera des paramètres à la collection UpdateParameters de l’ObjectDataSource](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image14.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image13.png)

**Figure 5**: le GridView ajoute des paramètres à la collection d’ObjectDataSource `UpdateParameters` ([cliquez pour afficher l’image en taille réelle](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image15.png))

Pour vous assurer que ObjectDataSource appelle la surcharge `UpdateProduct` qui accepte uniquement le nom, le prix et l’ID du produit, nous devons limiter le contrôle GridView aux champs modifiables uniquement pour les `ProductName` et les `UnitPrice`. Pour ce faire, vous pouvez supprimer les autres BoundFields et CheckBoxFields, en définissant la propriété `ReadOnly` de ces autres champs sur `true`ou d’une combinaison des deux. Pour ce didacticiel, nous allons simplement supprimer tous les champs GridView, à l’exception des `ProductName` et `UnitPrice` BoundFields, après quoi le balisage déclaratif de GridView ressemble à ce qui suit :

[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample3.aspx)]

Même si la surcharge `UpdateProduct` attend trois paramètres d’entrée, nous avons seulement deux BoundFields dans notre GridView. Cela est dû au fait que le paramètre d’entrée `productID` est une valeur de clé primaire et est transmis par le biais de la valeur de la propriété `DataKeyNames` de la ligne modifiée.

Notre GridView, avec la surcharge `UpdateProduct`, permet à un utilisateur de modifier uniquement le nom et le prix d’un produit sans perdre aucun des autres champs de produit.

[![l’interface autorise uniquement la modification du nom et du prix du produit](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image17.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image16.png)

**Figure 6**: l’interface permet de modifier uniquement le nom et le prix du produit ([cliquez pour afficher l’image en taille réelle](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image18.png))

> [!NOTE]
> Comme indiqué dans le didacticiel précédent, il est primordial que l’état d’affichage de GridView s soit activé (comportement par défaut). Si vous affectez à la propriété GridView s `EnableViewState` la valeur `false`, vous risquez d’avoir des utilisateurs simultanés qui suppriment ou modifient des enregistrements de manière involontaire. Consultez [Avertissement : problème d’accès concurrentiel avec ASP.NET 2,0 GridViews/DetailsView/FormViews qui prennent en charge l’édition et/ou la suppression et dont l’état d’affichage est désactivé](http://scottonwriting.net/sowblog/archive/2006/10/03/163215.aspx) pour plus d’informations.

## <a name="improving-theunitpriceformatting"></a>Amélioration de la mise en forme`UnitPrice`

Tandis que l’exemple de GridView illustré à la figure 6 fonctionne, le champ `UnitPrice` n’est pas mis en forme du tout, ce qui entraîne l’affichage d’un prix qui n’a pas de symboles monétaires et a quatre décimales. Pour appliquer une mise en forme monétaire pour les lignes non modifiables, il vous suffit de définir la propriété `DataFormatString` de `UnitPrice` BoundField sur `{0:c}` et sa propriété `HtmlEncode` sur `false`.

[![définir en conséquence les propriétés DataFormatString et HtmlEncode du UnitPrice](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image20.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image19.png)

**Figure 7**: définir les propriétés de `DataFormatString` et de `HtmlEncode` du `UnitPrice`[en conséquence (cliquez pour afficher l’image en taille réelle](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image21.png))

Avec cette modification, les lignes non modifiables mettent en forme le prix comme une devise ; Toutefois, la ligne modifiée affiche toujours la valeur sans le symbole monétaire et avec quatre décimales.

[les lignes non modifiables ![sont désormais mises en forme en tant que valeurs monétaires](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image23.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image22.png)

**Figure 8**: les lignes non modifiables sont désormais mises en forme en tant que valeurs monétaires ([cliquez pour afficher l’image en taille réelle](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image24.png))

Les instructions de mise en forme spécifiées dans la propriété `DataFormatString` peuvent être appliquées à l’interface de modification en affectant à la propriété `ApplyFormatInEditMode` de BoundField la valeur `true` (la valeur par défaut est `false`). Prenez un moment pour définir cette propriété sur `true`.

[![affectez la valeur true à la propriété ApplyFormatInEditMode de l’BoundField UnitPrice](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image26.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image25.png)

**Figure 9**: définir la propriété `ApplyFormatInEditMode` de `UnitPrice` BoundField sur `true` ([cliquez pour afficher l’image en taille réelle](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image27.png))

Avec cette modification, la valeur de la `UnitPrice` affichée dans la ligne modifiée est également mise en forme en tant que devise.

[![la valeur UnitPrice de la ligne modifiée est maintenant mise en forme en tant que devise](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image29.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image28.png)

**Figure 10**: la valeur `UnitPrice` de la ligne modifiée est maintenant mise en forme en tant que devise ([cliquez pour afficher l’image en taille réelle](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image30.png))

Toutefois, la mise à jour d’un produit avec le symbole monétaire dans la zone de texte, par exemple $19,00 lève une `FormatException`. Lorsque le GridView tente d’attribuer les valeurs fournies par l’utilisateur à la collection d' `UpdateParameters` ObjectDataSource, il ne peut pas convertir la chaîne `UnitPrice` « $19,00 » en `decimal` requise par le paramètre (voir figure 11). Pour remédier à cela, nous pouvons créer un gestionnaire d’événements pour l’événement `RowUpdating` de GridView et le faire analyser le `UnitPrice` fourni par l’utilisateur comme un `decimal`au format monétaire.

L’événement `RowUpdating` de GridView accepte comme deuxième paramètre un objet de type [GridViewUpdateEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdateeventargs(VS.80).aspx), qui inclut un dictionnaire `NewValues` comme l’une de ses propriétés qui contient les valeurs fournies par l’utilisateur prêtes à être assignées à la collection de `UpdateParameters` de l’ObjectDataSource. Nous pouvons remplacer la valeur `UnitPrice` existante dans la collection `NewValues` par une valeur décimale analysée à l’aide du format monétaire avec les lignes de code suivantes dans le gestionnaire d’événements `RowUpdating` :

[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample4.cs)]

Si l’utilisateur a fourni une valeur `UnitPrice` (telle que « $19,00 »), cette valeur est remplacée par la valeur décimale calculée par [Decimal. Parse](https://msdn.microsoft.com/library/system.decimal.parse(VS.80).aspx), en analysant la valeur en tant que devise. Cette opération analyse correctement la valeur décimale dans l’événement des symboles monétaires, des virgules, des points décimaux, etc., et utilise l' [énumération NumberStyles](https://msdn.microsoft.com/library/system.globalization.numberstyles(VS.80).aspx) dans l’espace de noms [System. Globalization](https://msdn.microsoft.com/library/abeh092z(VS.80).aspx) .

La figure 11 illustre le problème causé par les symboles monétaires dans le `UnitPrice`fourni par l’utilisateur, ainsi que la façon dont le gestionnaire d’événements `RowUpdating` de GridView peut être utilisé pour analyser correctement cette entrée.

[![la valeur UnitPrice de la ligne modifiée est maintenant mise en forme en tant que devise](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image32.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image31.png)

**Figure 11**: la valeur `UnitPrice` de la ligne modifiée est maintenant mise en forme en tant que devise ([cliquez pour afficher l’image en taille réelle](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image33.png))

## <a name="step-2-prohibitingnull-unitprices"></a>Étape 2 : interdiction de`NULL UnitPrices`

Alors que la base de données est configurée pour autoriser les valeurs de `NULL` dans la colonne `UnitPrice` de `Products` table, nous pouvons souhaiter empêcher les utilisateurs visitant cette page spécifique de spécifier une valeur de `UnitPrice` `NULL`. Autrement dit, si un utilisateur ne parvient pas à entrer une valeur de `UnitPrice` lors de la modification d’une ligne de produit, plutôt que d’enregistrer les résultats dans la base de données, nous souhaitons afficher un message indiquant à l’utilisateur que, par le biais de cette page, un tarif doit être spécifié pour tous les produits modifiés.

L’objet `GridViewUpdateEventArgs` passé dans le gestionnaire d’événements `RowUpdating` de GridView contient une propriété `Cancel` qui, si elle est définie sur `true`, termine le processus de mise à jour. Nous allons étendre le gestionnaire d’événements `RowUpdating` pour définir `e.Cancel` à `true` et afficher un message expliquant pourquoi la valeur `UnitPrice` de la collection `NewValues` est `null`.

Commencez par ajouter un contrôle Web Label à la page nommée `MustProvideUnitPriceMessage`. Ce contrôle Label est affiché si l’utilisateur ne parvient pas à spécifier une valeur de `UnitPrice` lors de la mise à jour d’un produit. Affectez à la propriété `Text` de l’étiquette la valeur « vous devez fournir un prix pour le produit ». J’ai également créé une nouvelle classe CSS dans `Styles.css` nommée `Warning` avec la définition suivante :

[!code-css[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample5.css)]

Enfin, définissez la propriété `CssClass` de l’étiquette sur `Warning`. À ce stade, le concepteur doit afficher le message d’avertissement dans une taille de police rouge, gras, italique, très grande au-dessus de GridView, comme illustré à la figure 12.

[![une étiquette a été ajoutée au-dessus du GridView](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image35.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image34.png)

**Figure 12**: une étiquette a été ajoutée au-dessus du contrôle GridView ([cliquez pour afficher l’image en taille réelle](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image36.png))

Par défaut, cette étiquette doit être masquée. par conséquent, définissez sa propriété `Visible` sur `false` dans le gestionnaire d’événements `Page_Load` :

[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample6.cs)]

Si l’utilisateur tente de mettre à jour un produit sans spécifier le `UnitPrice`, nous voulons annuler la mise à jour et afficher l’étiquette d’avertissement. Complétez le gestionnaire d’événements `RowUpdating` du GridView comme suit :

[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample7.cs)]

Si un utilisateur tente d’enregistrer un produit sans spécifier de prix, la mise à jour est annulée et un message utile s’affiche. Bien que la base de données (et la logique métier) autorise `NULL` `UnitPrice` s, cette page ASP.NET particulière ne le fait pas.

[![un utilisateur ne peut pas laisser un PrixUnitaire vide](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image38.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image37.png)

**Figure 13**: un utilisateur ne peut pas laisser `UnitPrice` vide ([Cliquer pour afficher l’image en taille réelle](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image39.png))

Jusqu’à présent, nous avons vu comment utiliser l’événement `RowUpdating` de GridView pour modifier par programmation les valeurs de paramètres assignées à la collection de `UpdateParameters` de l’ObjectDataSource et comment annuler complètement le processus de mise à jour. Ces concepts portent sur les contrôles DetailsView et FormView, et s’appliquent également à l’insertion et à la suppression.

Ces tâches peuvent également être effectuées au niveau ObjectDataSource par le biais de gestionnaires d’événements pour ses événements `Inserting`, `Updating`et `Deleting`. Ces événements se déclenchent avant l’appel de la méthode associée de l’objet sous-jacent et fournissent une opportunité de dernière chance de modifier la collection de paramètres d’entrée ou d’annuler l’opération. Les gestionnaires d’événements de ces trois événements sont passés à un objet de type [ObjectDataSourceMethodEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs(VS.80).aspx) qui a deux propriétés intéressantes :

- [Cancel](https://msdn.microsoft.com/library/system.componentmodel.canceleventargs.cancel(VS.80).aspx), qui, s’il est défini sur `true`, annule l’opération en cours d’exécution
- [InputParameters](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs.inputparameters(VS.80).aspx), qui est la collection de `InsertParameters`, `UpdateParameters`ou `DeleteParameters`, selon que le gestionnaire d’événements est destiné à l’événement `Inserting`, `Updating`ou `Deleting`

Pour illustrer l’utilisation des valeurs de paramètre au niveau de l’ObjectDataSource, nous allons inclure un DetailsView dans notre page qui permet aux utilisateurs d’ajouter un nouveau produit. Ce DetailsView sera utilisé pour fournir une interface permettant d’ajouter rapidement un nouveau produit à la base de données. Pour conserver une interface utilisateur cohérente lors de l’ajout d’un nouveau produit, n’hésitez pas à autoriser l’utilisateur à entrer des valeurs pour les champs `ProductName` et `UnitPrice`. Par défaut, les valeurs qui ne sont pas fournies dans l’interface d’insertion du contrôle DetailsView sont définies sur une valeur de base de données `NULL`. Toutefois, nous pouvons utiliser l’événement `Inserting` de l’ObjectDataSource pour injecter des valeurs par défaut différentes, comme nous le verrons bientôt.

## <a name="step-3-providing-an-interface-to-add-new-products"></a>Étape 3 : fournir une interface pour ajouter de nouveaux produits

Faites glisser un DetailsView de la boîte à outils vers le concepteur au-dessus du GridView, effacez ses propriétés `Height` et `Width`, puis liez-le à l’ObjectDataSource déjà présent sur la page. Cela ajoute un BoundField ou CheckBoxField pour chacun des champs du produit. Étant donné que nous souhaitons utiliser ce DetailsView pour ajouter de nouveaux produits, nous devons vérifier l’option Activer l’insertion à partir de la balise active. Toutefois, il n’existe pas d’option de ce type, car la méthode de `Insert()` de l’ObjectDataSource n’est pas mappée à une méthode dans la classe `ProductsBLL` (Rappelez-vous que nous définissons ce mappage sur (aucun) lors de la configuration de la source de données, voir la figure 3).

Pour configurer l’ObjectDataSource, sélectionnez le lien configurer la source de données à partir de sa balise active, en lançant l’Assistant. Le premier écran vous permet de modifier l’objet sous-jacent auquel ObjectDataSource est lié. Laissez la valeur `ProductsBLL`. L’écran suivant répertorie les mappages des méthodes de l’ObjectDataSource aux objets sous-jacents. Bien que nous indiquions explicitement que les méthodes `Insert()` et `Delete()` ne devaient pas être mappées à des méthodes, si vous accédez aux onglets insertion et suppression, vous verrez qu’un mappage est présent. Cela est dû au fait que les méthodes de `AddProduct` et de `DeleteProduct` du `ProductsBLL`utilisent l’attribut `DataObjectMethodAttribute` pour indiquer qu’il s’agit des méthodes par défaut pour `Insert()` et `Delete()`, respectivement. Par conséquent, l’Assistant ObjectDataSource les sélectionne chaque fois que vous exécutez l’Assistant, sauf si une autre valeur est spécifiée explicitement.

Laissez la méthode `Insert()` pointant vers la méthode `AddProduct`, mais définissez à nouveau la liste déroulante de l’onglet supprimer sur (aucune).

[![définir la méthode AddProduct dans la liste déroulante de l’onglet Insertion](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image41.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image40.png)

**Figure 14**: définir la liste déroulante de l’onglet Insertion sur la méthode `AddProduct` ([cliquez pour afficher l’image en taille réelle](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image42.png))

[![définir la liste déroulante de l’onglet supprimer sur (aucun)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image44.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image43.png)

**Figure 15**: définir la liste déroulante de l’onglet supprimer sur (aucun) ([cliquez pour afficher l’image en taille réelle](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image45.png))

Après avoir apporté ces modifications, la syntaxe déclarative de ObjectDataSource sera étendue pour inclure une collection `InsertParameters`, comme indiqué ci-dessous :

[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample8.aspx)]

La réexécution de l’Assistant a ajouté la propriété `OldValuesParameterFormatString`. Prenez un moment pour effacer cette propriété en la définissant sur la valeur par défaut (`{0}`) ou en la supprimant entièrement de la syntaxe déclarative.

Avec ObjectDataSource fournissant des fonctionnalités d’insertion, la balise active du contrôle DetailsView inclut désormais la case à cocher Activer l’insertion. Revenez au concepteur et cochez cette option. Ensuite, réduisez le DetailsView pour qu’il n’ait que deux BoundFields-`ProductName` et `UnitPrice`-et CommandField. À ce stade, la syntaxe déclarative du contrôle DetailsView doit ressembler à ceci :

[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample9.aspx)]

La figure 16 illustre cette page lorsque vous l’affichez dans un navigateur à ce stade. Comme vous pouvez le voir, le contrôle DetailsView répertorie le nom et le prix du premier produit (Chai). Ce que nous voulons, toutefois, est une interface d’insertion qui permet à l’utilisateur d’ajouter rapidement un nouveau produit à la base de données.

[![le contrôle DetailsView est actuellement affiché en mode lecture seule](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image47.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image46.png)

**Figure 16**: le contrôle DetailsView est actuellement affiché en mode lecture seule ([cliquez pour afficher l’image en taille réelle](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image48.png))

Pour afficher le DetailsView dans son mode d’insertion, nous devons définir la propriété `DefaultMode` sur `Inserting`. Le contrôle DetailsView est alors rendu en mode insertion lors de sa première visite et il est conservé après l’insertion d’un nouvel enregistrement. Comme le montre la figure 17, un contrôle DetailsView fournit une interface rapide pour l’ajout d’un nouvel enregistrement.

[![le contrôle DetailsView fournit une interface pour ajouter rapidement un nouveau produit](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image50.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image49.png)

**Figure 17**: le contrôle DetailsView fournit une interface permettant d’ajouter rapidement un nouveau produit ([cliquez pour afficher l’image en taille réelle](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image51.png))

Lorsque l’utilisateur entre un nom de produit et un prix (par exemple, « Acme Water » et 1,99, comme dans la figure 17) et clique sur Insérer, une publication (postback) se produit et le workflow d’insertion commence, sanctionné par l’ajout d’un nouvel enregistrement de produit à la base de données. Le contrôle DetailsView gère son interface d’insertion et le contrôle GridView est automatiquement lié à sa source de données afin d’inclure le nouveau produit, comme illustré à la figure 18.

![Le produit](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image52.png)

**Figure 18**: le produit « Acme Water » a été ajouté à la base de données

Alors que le GridView de la figure 18 ne l’affiche pas, les champs de produit absents de l’interface DetailsView `CategoryID`, `SupplierID`, `QuantityPerUnit`, etc., sont affectés `NULL` valeurs de base de données. Vous pouvez le constater en effectuant les étapes suivantes :

1. Accéder au Explorateur de serveurs dans Visual Studio
2. Développement du nœud de base de données `NORTHWND.MDF`
3. Cliquez avec le bouton droit sur le nœud `Products` de la table de base de données
4. Sélectionner Afficher les données de la table

Cela permet de répertorier tous les enregistrements dans la table `Products`. Comme le montre la figure 19, toutes les colonnes du nouveau produit autres que `ProductID`, `ProductName`et `UnitPrice` comportent des valeurs de `NULL`.

[![les champs de produit non fournis dans le contrôle DetailsView reçoivent des valeurs NULL](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image54.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image53.png)

**Figure 19**: les champs de produit non fournis dans le contrôle DetailsView sont affectés `NULL` valeurs ([cliquez pour afficher l’image en taille réelle](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image55.png))

Nous pouvons fournir une valeur par défaut autre que `NULL` pour une ou plusieurs de ces valeurs de colonne, soit parce que `NULL` n’est pas la meilleure option par défaut, soit parce que la colonne de base de données elle-même n’autorise pas les `NULL` s. Pour ce faire, nous pouvons définir par programmation les valeurs des paramètres de la collection de `InputParameters` du contrôle DetailsView. Cette assignation peut être effectuée dans le gestionnaire d’événements pour l’événement de `ItemInserting` du contrôle DetailsView ou l’événement de `Inserting` de l’ObjectDataSource. Étant donné que nous avons déjà examiné l’utilisation des événements antérieurs et postérieurs au niveau du contrôle de données Web, nous allons explorer l’utilisation des événements de l’ObjectDataSource.

## <a name="step-4-assigning-values-to-thecategoryidandsupplieridparameters"></a>Étape 4 : affectation de valeurs aux paramètres`CategoryID`et`SupplierID`

Pour ce didacticiel, supposons que pour notre application, lors de l’ajout d’un nouveau produit via cette interface, un `CategoryID` et `SupplierID` valeur de 1 lui soient attribués. Comme mentionné précédemment, l’ObjectDataSource contient une paire d’événements de pré-et de poste qui se déclenchent pendant le processus de modification des données. Lorsque sa méthode `Insert()` est appelée, ObjectDataSource commence par déclencher son événement `Inserting`, puis appelle la méthode à laquelle sa méthode `Insert()` a été mappée, puis déclenche l’événement `Inserted`. Le gestionnaire d’événements `Inserting` vous offre une dernière possibilité de modifier les paramètres d’entrée ou d’annuler l’opération.

> [!NOTE]
> Dans une application réelle, vous souhaiterez peut-être laisser l’utilisateur spécifier la catégorie et le fournisseur ou choisir cette valeur pour ces éléments en fonction de certains critères ou de la logique métier (au lieu de sélectionner aveuglément un ID de 1). Quoi qu’il en soit, l’exemple montre comment définir par programmation la valeur d’un paramètre d’entrée à partir de l’événement de préniveau de l’ObjectDataSource.

Prenez un moment pour créer un gestionnaire d’événements pour l’événement de `Inserting` de l’ObjectDataSource. Notez que le deuxième paramètre d’entrée du gestionnaire d’événements est un objet de type `ObjectDataSourceMethodEventArgs`, qui a une propriété pour accéder à la collection de paramètres (`InputParameters`) et à une propriété pour annuler l’opération (`Cancel`).

[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample10.cs)]

À ce stade, la propriété `InputParameters` contient la collection d’ObjectDataSource `InsertParameters` avec les valeurs assignées à partir du contrôle DetailsView. Pour modifier la valeur de l’un de ces paramètres, utilisez simplement : `e.InputParameters["paramName"] = value`. Par conséquent, pour définir les `CategoryID` et `SupplierID` sur les valeurs 1, ajustez le `Inserting` gestionnaire d’événements de façon à ce qu’il ressemble à ce qui suit :

[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample11.cs)]

Cette fois-ci, lorsque vous ajoutez un nouveau produit (par exemple, Acme soda), les colonnes `CategoryID` et `SupplierID` du nouveau produit sont définies sur 1 (voir figure 20).

[![les nouveaux produits ont maintenant leurs valeurs CategoryID et RéfFournisseur définies sur 1](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image57.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image56.png)

**Figure 20**: les valeurs `CategoryID` et `SupplierID` des nouveaux produits sont désormais définies sur 1 ([cliquez pour afficher l’image en taille réelle](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image58.png))

## <a name="summary"></a>Récapitulatif

Pendant le processus de modification, d’insertion et de suppression, le contrôle Web de données et l’ObjectDataSource passent par un certain nombre d’événements de pré-et de après-niveau. Dans ce didacticiel, nous avons examiné les événements de préversion et vu comment les utiliser pour personnaliser les paramètres d’entrée ou annuler l’opération de modification des données à la fois à partir du contrôle Web des données et des événements de ObjectDataSource. Dans le didacticiel suivant, nous allons examiner comment créer et utiliser des gestionnaires d’événements pour les événements de publication.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel étaient Jackie Goor et Liz Shulok. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](an-overview-of-inserting-updating-and-deleting-data-cs.md)
> [Suivant](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)
