---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
title: Ajout de contrôles de validation aux interfaces de modification et d’insertion (VB) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons voir combien il est facile d’ajouter des contrôles de validation aux EditItemTemplate et InsertItemTemplate d’un contrôle Web de données, afin de fournir un plus grand nombre de contrôles...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: e3d7028a-7a22-4a4f-babe-d53afc41c0e2
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
msc.type: authoredcontent
ms.openlocfilehash: 5c5ad110ee0836f0a464b02a2b29254e2e06381e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74571343"
---
# <a name="adding-validation-controls-to-the-editing-and-inserting-interfaces-vb"></a>Ajout de contrôles Validation aux interfaces de modification et d’insertion (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_19_VB.exe) ou [Télécharger le PDF](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/datatutorial19vb1.pdf)

> Dans ce didacticiel, nous allons voir combien il est facile d’ajouter des contrôles de validation aux EditItemTemplate et InsertItemTemplate d’un contrôle Web de données, afin de fournir une interface utilisateur plus infaillible.

## <a name="introduction"></a>Introduction

Les contrôles GridView et DetailsView des exemples que nous avons explorés au cours des trois derniers didacticiels ont tous été composés de BoundFields et CheckBoxFields (les types de champs ajoutés automatiquement par Visual Studio lors de la liaison d’un GridView ou d’un DetailsView à une source de données) contrôle via la balise active). Lors de la modification d’une ligne dans un contrôle GridView ou DetailsView, les BoundFields qui ne sont pas en lecture seule sont convertis en zones de texte, à partir desquelles l’utilisateur final peut modifier les données existantes. De même, lors de l’insertion d’un nouvel enregistrement dans un contrôle DetailsView, les BoundFields dont la propriété `InsertVisible` a la valeur `True` (valeur par défaut) sont rendus sous forme de zones de texte vides, dans lesquelles l’utilisateur peut fournir les valeurs de champ de l’enregistrement. De même, les CheckBoxFields, qui sont désactivés dans l’interface standard en lecture seule, sont convertis en cases à cocher activées dans les interfaces de modification et d’insertion.

Alors que les interfaces de modification et d’insertion par défaut pour BoundField et CheckBoxField peuvent être utiles, l’interface ne dispose d’aucune sorte de validation. Si un utilisateur fait une erreur d’entrée de données, par exemple en omettant le champ `ProductName` ou en entrant une valeur non valide pour `UnitsInStock` (telle que-50), une exception est levée à partir des profondeurs de l’architecture de l’application. Bien que cette exception puisse être gérée de façon appropriée comme illustré dans le [didacticiel précédent](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md), idéalement, la modification ou l’insertion d’une interface utilisateur inclurait des contrôles de validation pour empêcher un utilisateur d’entrer de telles données non valides en premier lieu.

Pour fournir une interface de modification ou d’insertion personnalisée, nous devons remplacer BoundField ou CheckBoxField par un TemplateField. TemplateFields, qui a été le sujet de la discussion dans l' [utilisation de TemplateFields dans le contrôle GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) et [l’utilisation de TemplateFields dans les](../custom-formatting/using-templatefields-in-the-detailsview-control-vb.md) didacticiels de contrôle DetailsView, peut être constitué de plusieurs modèles définissant des interfaces distinctes pour différents États de ligne. Le `ItemTemplate` TemplateField est utilisé lors du rendu de champs ou de lignes en lecture seule dans les contrôles DetailsView ou GridView, tandis que les `EditItemTemplate` et `InsertItemTemplate` indiquent les interfaces à utiliser respectivement pour les modes édition et insertion.

Dans ce didacticiel, nous allons voir combien il est facile d’ajouter des contrôles de validation à la `EditItemTemplate` du TemplateField et `InsertItemTemplate` de fournir une interface utilisateur plus infaillible. Plus précisément, ce didacticiel prend l’exemple créé dans le didacticiel [examen des événements associés à l’insertion, la mise à jour et la suppression](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) et augmente les interfaces de modification et d’insertion pour inclure la validation appropriée.

## <a name="step-1-replicating-the-example-fromexamining-the-events-associated-with-inserting-updating-and-deletingexamining-the-events-associated-with-inserting-updating-and-deleting-vbmd"></a>Étape 1 : réplication de l’exemple à partir de[l’examen des événements associés à l’insertion, la mise à jour et la suppression](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)

Dans le didacticiel [examen des événements associés à l’insertion, à la mise à jour et à la suppression,](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) nous avons créé une page qui répertorie les noms et les prix des produits dans un GridView modifiable. En outre, la page incluait un DetailsView dont la propriété `DefaultMode` était définie sur `Insert`, ce qui rendait donc toujours le rendu en mode insertion. À partir de ce DetailsView, l’utilisateur peut entrer le nom et le prix d’un nouveau produit, cliquer sur Insérer et l’avoir ajouté au système (voir la figure 1).

[![l’exemple précédent permet aux utilisateurs d’ajouter de nouveaux produits et de modifier ceux qui existent déjà.](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image2.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image1.png)

**Figure 1**: l’exemple précédent permet aux utilisateurs d’ajouter de nouveaux produits et de modifier ceux qui existent déjà ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image3.png))

L’objectif de ce didacticiel est d’augmenter le DetailsView et GridView pour fournir des contrôles de validation. En particulier, notre logique de validation :

- Exiger que le nom soit fourni lors de l’insertion ou de la modification d’un produit
- Exiger que le prix soit fourni lors de l’insertion d’un enregistrement ; lors de la modification d’un enregistrement, nous avons toujours besoin d’un prix, mais utilisera la logique de programmation dans le gestionnaire d’événements `RowUpdating` du GridView déjà présent dans le didacticiel précédent
- Vérifiez que la valeur entrée pour le prix est un format monétaire valide

Avant de pouvoir examiner l’exemple précédent pour inclure la validation, nous devons d’abord répliquer l’exemple de la page `DataModificationEvents.aspx` vers la page de ce didacticiel, `UIValidation.aspx`. Pour ce faire, nous devons copier à la fois le balisage déclaratif de la page `DataModificationEvents.aspx` et son code source. Tout d’abord, copiez le balisage déclaratif en effectuant les étapes suivantes :

1. Ouvrir la page `DataModificationEvents.aspx` dans Visual Studio
2. Accédez au balisage déclaratif de la page (cliquez sur le bouton source en bas de la page).
3. Copiez le texte dans les balises `<asp:Content>` et `</asp:Content>` (lignes 3 à 44), comme illustré à la figure 2.

[![copier le texte dans le contrôle &lt;asp : content&gt;](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image5.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image4.png)

**Figure 2**: copier le texte dans le contrôle de `<asp:Content>` ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image6.png))

1. Ouvrir la page `UIValidation.aspx`
2. Atteindre le balisage déclaratif de la page
3. Collez le texte dans le contrôle `<asp:Content>`.

Pour copier le code source, ouvrez la page `DataModificationEvents.aspx.vb` et copiez simplement le texte *dans* la classe `EditInsertDelete_DataModificationEvents`. Copiez les trois gestionnaires d’événements (`Page_Load`, `GridView1_RowUpdating`et `ObjectDataSource1_Inserting`), mais ne copiez **pas** la déclaration de classe. Collez le texte copié *dans* la classe `EditInsertDelete_UIValidation` dans `UIValidation.aspx.vb`.

Après avoir déplacé le contenu et le code de `DataModificationEvents.aspx` vers `UIValidation.aspx`, prenez un moment pour tester votre progression dans un navigateur. Vous devez voir la même sortie et expérimenter les mêmes fonctionnalités dans chacune de ces deux pages (reportez-vous à la figure 1 pour obtenir une capture d’écran de `DataModificationEvents.aspx` en action).

## <a name="step-2-converting-the-boundfields-into-templatefields"></a>Étape 2 : conversion du BoundFields en TemplateFields

Pour ajouter des contrôles de validation aux interfaces de modification et d’insertion, les BoundFields utilisés par les contrôles DetailsView et GridView doivent être convertis en TemplateFields. Pour ce faire, cliquez respectivement sur les liens modifier les colonnes et modifier les champs dans les balises actives GridView et DetailsView. À partir de là, sélectionnez chacun des BoundFields et cliquez sur le lien « convertir ce champ en TemplateField ».

[![convertir chaque BoundFields du contrôle DetailsView et GridView en TemplateFields](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image8.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image7.png)

**Figure 3**: convertir chaque BoundFields du contrôle DetailsView et de GridView en TemplateFields ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image9.png))

La conversion d’un BoundField en TemplateField via la boîte de dialogue champs génère un TemplateField qui présente les mêmes interfaces en lecture seule, modification et insertion que le BoundField lui-même. Le balisage suivant montre la syntaxe déclarative pour le champ `ProductName` dans le DetailsView une fois qu’il a été converti en TemplateField :

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample1.aspx)]

Notez que ce TemplateField avait trois modèles créés automatiquement `ItemTemplate`, `EditItemTemplate`et `InsertItemTemplate`. L' `ItemTemplate` affiche une valeur de champ de données unique (`ProductName`) à l’aide d’un contrôle Web Label, tandis que le `EditItemTemplate` et `InsertItemTemplate` présenter la valeur du champ de données dans un contrôle Web TextBox qui associe le champ de données à la propriété `Text` de la zone de texte à l’aide de la liaison de données bidirectionnelle. Étant donné que nous utilisons uniquement le DetailsView dans cette page pour l’insertion, vous pouvez supprimer les `ItemTemplate` et `EditItemTemplate` des deux TemplateFields, bien qu’il n’y ait pas de danger de les laisser.

Étant donné que le contrôle GridView ne prend pas en charge les fonctionnalités d’insertion intégrées du contrôle DetailsView, la conversion du champ `ProductName` du contrôle GridView en TemplateField entraîne uniquement l' `ItemTemplate` et l' `EditItemTemplate`:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample2.aspx)]

En cliquant sur le bouton « convertir ce champ en TemplateField », Visual Studio a créé un TemplateField dont les modèles imitent l’interface utilisateur du BoundField converti. Vous pouvez le vérifier en visitant cette page via un navigateur. Vous constaterez que l’apparence et le comportement du TemplateFields sont identiques à ceux de l’utilisation de BoundFields à la place.

> [!NOTE]
> N’hésitez pas à personnaliser les interfaces de modification dans les modèles en fonction des besoins. Par exemple, nous pouvons souhaiter que la zone de texte dans le `UnitPrice` TemplateFields soit rendue sous la forme d’une zone de texte plus petite que la zone de texte `ProductName`. Pour ce faire, vous pouvez définir la propriété `Columns` de la zone de texte sur une valeur appropriée ou fournir une largeur absolue via la propriété `Width`. Dans le didacticiel suivant, nous verrons comment personnaliser complètement l’interface de modification en remplaçant la zone de texte par un autre contrôle Web d’entrée de données.

## <a name="step-3-adding-the-validation-controls-to-the-gridviewsedititemtemplate-s"></a>Étape 3 : ajout des contrôles de validation aux`EditItemTemplate` de GridView

Lors de la construction de formulaires de saisie de données, il est important que les utilisateurs entrent les champs obligatoires et que toutes les entrées fournies soient des valeurs valides et correctement mises en forme. Pour vous assurer que les entrées d’un utilisateur sont valides, ASP.NET fournit cinq contrôles de validation intégrés conçus pour valider la valeur d’un contrôle d’entrée unique :

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) garantit qu’une valeur a été fournie
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) valide une valeur par rapport à une autre valeur de contrôle Web ou une valeur constante, ou garantit que le format de la valeur est légal pour un type de données spécifié
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) garantit qu’une valeur est comprise dans une plage de valeurs
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) valide une valeur par rapport à une [expression régulière](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) valide une valeur par rapport à une méthode personnalisée définie par l’utilisateur

Pour plus d’informations sur ces cinq contrôles, consultez la [section contrôles de validation](https://quickstarts.asp.net/quickstartv20/aspnet/doc/ctrlref/validation/default.aspx) des [didacticiels de démarrage rapide ASP.net](https://asp.net/QuickStart/aspnet/).

Pour notre didacticiel, nous allons utiliser un contrôle RequiredFieldValidator à la fois dans les `ProductName` DetailsView et GridView, ainsi qu’avec un RequiredFieldValidator dans le `UnitPrice` TemplateField du contrôle DetailsView. En outre, nous devons ajouter un CompareValidator aux deux contrôles `UnitPrice` TemplateFields, qui garantit que le prix entré a une valeur supérieure ou égale à 0 et qu’il est présenté dans un format monétaire valide.

> [!NOTE]
> Alors que ASP.NET 1. x contenait ces cinq mêmes contrôles de validation, ASP.NET 2,0 a ajouté un certain nombre d’améliorations, les deux principaux étant la prise en charge des scripts côté client pour les navigateurs autres qu’Internet Explorer et la possibilité de partitionner les contrôles de validation sur une page dans groupes de validation. Pour plus d’informations sur les nouvelles fonctionnalités de contrôle de validation dans 2,0, consultez [la section discroisant les contrôles de validation dans ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).

Commençons par ajouter les contrôles de validation nécessaires aux `EditItemTemplate` s dans le TemplateFields du GridView. Pour ce faire, cliquez sur le lien modifier les modèles à partir de la balise active du contrôle GridView pour afficher l’interface de modification du modèle. À partir de là, vous pouvez sélectionner le modèle à modifier dans la liste déroulante. Étant donné que nous souhaitons augmenter l’interface de modification, nous devons ajouter des contrôles de validation aux `ProductName` et à la `EditItemTemplate` de `UnitPrice`.

[![nous devons étendre les EditItemTemplates ProductName et UnitPrice](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image11.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image10.png)

**Figure 4**: nous devons étendre la `ProductName` et les `EditItemTemplate` de `UnitPrice`([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image12.png))

Dans la `EditItemTemplate``ProductName`, ajoutez un RequiredFieldValidator en le faisant glisser de la boîte à outils vers l’interface de modification de modèle, en plaçant après la zone de texte.

[![ajouter un RequiredFieldValidator à la valeur ProductName EditItemTemplate](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image14.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image13.png)

**Figure 5**: ajouter un RequiredFieldValidator au `EditItemTemplate` `ProductName` ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image15.png))

Tous les contrôles de validation fonctionnent en validant l’entrée d’un seul contrôle Web ASP.NET. Par conséquent, nous devons indiquer que le contrôle RequiredFieldValidator que nous venons d’ajouter doit effectuer une validation par rapport à la zone de texte de la `EditItemTemplate`; pour ce faire, affectez à la [propriété ControlToValidate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) du contrôle de validation la `ID` du contrôle Web approprié. La zone de texte a actuellement le `ID` nondescript de `TextBox1`, mais nous allons la remplacer par une valeur plus appropriée. Cliquez sur la zone de texte dans le modèle, puis, dans la Fenêtre Propriétés, remplacez la `ID` `TextBox1` par `EditProductName`.

[![modifier l’ID de la zone de texte en EditProductName](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image17.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image16.png)

**Figure 6**: modifier le `ID` de la zone de texte en `EditProductName` ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image18.png))

Ensuite, affectez à la propriété de `ControlToValidate` de RequiredFieldValidator la valeur `EditProductName`. Enfin, définissez la [propriété ErrorMessage](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) sur « vous devez fournir le nom du produit » et la [propriété Text](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) sur «\*». La valeur de la propriété `Text`, si elle est fournie, est le texte affiché par le contrôle de validation si la validation échoue. La valeur de la propriété `ErrorMessage`, qui est obligatoire, est utilisée par le contrôle ValidationSummary ; Si la valeur de la propriété `Text` est omise, la valeur de la propriété `ErrorMessage` est également le texte affiché par le contrôle de validation sur une entrée non valide.

Après avoir défini ces trois propriétés de RequiredFieldValidator, votre écran doit ressembler à la figure 7.

[![définir les propriétés ControlToValidate, ErrorMessage et Text de RequiredFieldValidator](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image20.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image19.png)

**Figure 7**: définir les propriétés de `ControlToValidate`, `ErrorMessage`et `Text` du RequiredFieldValidator ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image21.png))

Une fois que le contrôle RequiredFieldValidator a été ajouté au `ProductName` `EditItemTemplate`, il ne reste plus qu’à ajouter la validation nécessaire au `EditItemTemplate``UnitPrice`. Étant donné que nous avons décidé que, pour cette page, l' `UnitPrice` est facultatif lors de la modification d’un enregistrement, nous n’avons pas besoin d’ajouter un RequiredFieldValidator. Toutefois, nous devons ajouter un CompareValidator pour vous assurer que le `UnitPrice`, s’il est fourni, est correctement mis en forme en tant que devise et supérieur ou égal à 0.

Avant d’ajouter CompareValidator à la `UnitPrice` `EditItemTemplate`, nous allons d’abord modifier l’ID du contrôle Web TextBox de `TextBox2` à `EditUnitPrice`. Après avoir apporté cette modification, ajoutez le CompareValidator, en affectant à sa propriété `ControlToValidate` la valeur `EditUnitPrice`, à sa propriété `ErrorMessage` la valeur « le prix doit être supérieur ou égal à zéro et ne peut pas inclure le symbole monétaire », et sa propriété `Text` à «\*».

Pour indiquer que la valeur de `UnitPrice` doit être supérieure ou égale à 0, affectez à la [propriété Operator](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) du CompareValidator la valeur `GreaterThanEqual`, à sa [propriété ValueToCompare](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) la valeur « 0 » et à sa [propriété de type](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) la valeur `Currency`. La syntaxe déclarative suivante montre le `EditItemTemplate` du `UnitPrice` TemplateField après que ces modifications ont été apportées :

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample3.aspx)]

Après avoir apporté ces modifications, ouvrez la page dans un navigateur. Si vous tentez d’omettre le nom ou d’entrer une valeur de prix non valide lors de la modification d’un produit, un astérisque apparaît en regard de la zone de texte. Comme le montre la figure 8, une valeur de prix qui comprend le symbole monétaire, tel que $19,95, est considérée comme non valide. Le `Currency` du CompareValidator `Type` autorise des séparateurs numériques (tels que des virgules ou des points, selon les paramètres de culture) et un signe plus ou moins de début, mais n’autorise *pas* de symbole monétaire. Ce comportement peut déguiser les utilisateurs, car l’interface de modification restitue actuellement le `UnitPrice` à l’aide du format monétaire.

> [!NOTE]
> Rappelez-vous que dans les événements associés au didacticiel d' *insertion, de mise à jour et de suppression* , nous définissons la propriété `DataFormatString` de BoundField sur `{0:c}` afin de la mettre en forme en tant que devise. En outre, nous définissons la propriété `ApplyFormatInEditMode` sur true, provoquant l’interface de modification du contrôle GridView pour mettre en forme la `UnitPrice` en tant que devise. Lors de la conversion de BoundField en TemplateField, Visual Studio a noté ces paramètres et a mis en forme la propriété de `Text` de la zone de texte en tant que devise à l’aide de la syntaxe de liaison de données `<%# Bind("UnitPrice", "{0:c}") %>`.

[![un astérisque apparaît en regard des zones de texte avec une entrée non valide](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image23.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image22.png)

**Figure 8**: un astérisque apparaît en regard des zones de texte avec une entrée non valide ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image24.png))

Tandis que la validation fonctionne telle quelle, l’utilisateur doit supprimer manuellement le symbole monétaire lors de la modification d’un enregistrement, ce qui n’est pas acceptable. Pour y remédier, nous disposons de trois options :

1. Configurez la `EditItemTemplate` afin que la valeur `UnitPrice` ne soit pas mise en forme en tant que devise.
2. Autorisez l’utilisateur à entrer un symbole monétaire en supprimant CompareValidator et en le remplaçant par un RegularExpressionValidator qui vérifie correctement une valeur monétaire correctement mise en forme. Le problème ici est que l’expression régulière pour valider une valeur monétaire n’est pas assez conviviale et nécessiterait l’écriture de code si nous voulions incorporer des paramètres de culture.
3. Supprimez complètement le contrôle de validation et reposez-vous sur la logique de validation côté serveur dans le gestionnaire d’événements `RowUpdating` du GridView.

Passons à l’option #1 pour cet exercice. Actuellement, le `UnitPrice` est mis en forme en tant que devise en raison de l’expression de liaison de données pour la zone de texte dans le `EditItemTemplate`: `<%# Bind("UnitPrice", "{0:c}") %>`. Modifiez l’instruction de liaison en `Bind("UnitPrice", "{0:n2}")`, qui met en forme le résultat sous la forme d’un nombre avec deux chiffres de précision. Vous pouvez effectuer cette opération directement par le biais de la syntaxe déclarative ou en cliquant sur le lien modifier les DataBindings dans la zone de texte `EditUnitPrice` de la `EditItemTemplate` du `UnitPrice` TemplateField (voir figures 9 et 10).

[![cliquez sur le lien modifier les DataBindings de la zone de texte](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image26.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image25.png)

**Figure 9**: cliquez sur le lien modifier les DataBindings de la zone de texte ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image27.png))

[![spécifier le spécificateur de format dans l’instruction de liaison](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image29.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image28.png)

**Figure 10**: spécifier le spécificateur de format dans l’instruction `Bind` ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image30.png))

Avec cette modification, le prix mis en forme dans l’interface de modification comprend des virgules comme séparateur de groupes et un point comme séparateur décimal, mais ne quitte pas le symbole monétaire.

> [!NOTE]
> Le `EditItemTemplate` `UnitPrice` n’inclut pas un RequiredFieldValidator, ce qui permet la publication et la logique de mise à jour pour commencer. Toutefois, le gestionnaire d’événements `RowUpdating` copié à partir du didacticiel *examen des événements associés à l’insertion, à la mise à jour et à la suppression* comprend une vérification par programme qui garantit que la `UnitPrice` est fournie. N’hésitez pas à supprimer cette logique, à la conserver en l’espace de la fonction, ou à ajouter un RequiredFieldValidator au `EditItemTemplate`de `UnitPrice`.

## <a name="step-4-summarizing-data-entry-problems"></a>Étape 4 : synthèse des problèmes d’entrée de données

Outre les cinq contrôles de validation, ASP.NET comprend le [contrôle ValidationSummary](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), qui affiche le `ErrorMessage` s de ces contrôles de validation qui ont détecté des données non valides. Ces données de synthèse peuvent être affichées sous forme de texte sur la page Web ou par le biais d’une MessageBox modale côté client. Nous allons améliorer ce didacticiel pour inclure une MessageBox côté client résumant tous les problèmes de validation.

Pour ce faire, faites glisser un contrôle ValidationSummary de la boîte à outils vers le concepteur. L’emplacement du contrôle de validation n’a pas vraiment d’importance, car nous allons le configurer pour afficher uniquement le résumé en tant que MessageBox. Après avoir ajouté le contrôle, définissez sa [propriété ShowSummary](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) sur `False` et sa [propriété méthode ShowMessageBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) sur `True`. Dans ce cas, toutes les erreurs de validation sont résumées dans la MessageBox côté client.

[![les erreurs de validation sont résumées dans la MessageBox côté client](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image32.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image31.png)

**Figure 11**: les erreurs de validation sont résumées dans la MessageBox côté client ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image33.png))

## <a name="step-5-adding-the-validation-controls-to-the-detailsviewsinsertitemtemplate"></a>Étape 5 : ajout des contrôles de validation à la`InsertItemTemplate` du contrôle DetailsView

Tout ce qui reste pour ce didacticiel consiste à ajouter les contrôles de validation à l’interface d’insertion du contrôle DetailsView. Le processus d’ajout de contrôles de validation aux modèles du contrôle DetailsView est identique à celui examiné à l’étape 3. par conséquent, nous allons effectuer une tâche dans cette étape. Comme nous l’avons fait avec le `EditItemTemplate` de GridView, je vous encourage à renommer les `ID` s des zones de texte nondescript `TextBox1` et `TextBox2` à `InsertProductName` et `InsertUnitPrice`.

Ajoutez un RequiredFieldValidator à la `InsertItemTemplate``ProductName`. Définissez le `ControlToValidate` sur le `ID` de la zone de texte dans le modèle, sa propriété `Text` sur «\*» et sa propriété `ErrorMessage` sur « vous devez fournir le nom du produit ».

Étant donné que la `UnitPrice` est requise pour cette page lors de l’ajout d’un nouvel enregistrement, ajoutez un contrôle RequiredFieldValidator au `InsertItemTemplate``UnitPrice`, en définissant ses propriétés `ControlToValidate`, `Text`et `ErrorMessage` de façon appropriée. Enfin, ajoutez un CompareValidator au `UnitPrice` `InsertItemTemplate` également, en configurant ses propriétés `ControlToValidate`, `Text`, `ErrorMessage`, `Type`, `Operator`et `ValueToCompare` comme nous l’avons fait avec le CompareValidator du `UnitPrice`dans le `EditItemTemplate`du contrôle GridView.

Après l’ajout de ces contrôles de validation, un nouveau produit ne peut pas être ajouté au système si son nom n’est pas fourni ou si son prix est un nombre négatif ou formaté illégalement.

[![logique de validation a été ajoutée à l’interface d’insertion du contrôle DetailsView](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image35.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image34.png)

**Figure 12**: la logique de validation a été ajoutée à l’interface d’insertion du contrôle DetailsView ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image36.png))

## <a name="step-6-partitioning-the-validation-controls-into-validation-groups"></a>Étape 6 : partitionnement des contrôles de validation en groupes de validation

Notre page se compose de deux ensembles logiquement disparates de contrôles de validation : ceux qui correspondent à l’interface de modification du contrôle GridView et ceux qui correspondent à l’interface d’insertion du contrôle DetailsView. Par défaut, lorsqu’une publication se produit, *tous les* contrôles de validation de la page sont vérifiés. Toutefois, lors de la modification d’un enregistrement, nous ne souhaitons pas valider les contrôles de validation de l’interface d’insertion du contrôle DetailsView. La figure 13 illustre notre dilemme en cours lorsqu’un utilisateur modifie un produit avec des valeurs parfaitement valides, en cliquant sur mettre à jour provoque une erreur de validation, car les valeurs nom et prix de l’interface d’insertion sont vides.

[![la mise à jour d’un produit provoque le déclenchement des contrôles de validation de l’interface d’insertion](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image38.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image37.png)

**Figure 13**: la mise à jour d’un produit entraîne le déclenchement des contrôles de validation de l’interface d’insertion ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image39.png))

Les contrôles de validation dans ASP.NET 2,0 peuvent être partitionnés en groupes de validation par le biais de leur propriété `ValidationGroup`. Pour associer un ensemble de contrôles de validation dans un groupe, affectez simplement à leur propriété `ValidationGroup` la même valeur. Pour notre didacticiel, définissez les propriétés `ValidationGroup` des contrôles de validation dans le TemplateFields de GridView sur `EditValidationControls` et les propriétés `ValidationGroup` du TemplateFields du contrôle DetailsView à `InsertValidationControls`. Ces modifications peuvent être effectuées directement dans le balisage déclaratif ou via le Fenêtre Propriétés lors de l’utilisation de l’interface de modification du modèle du concepteur.

Outre les contrôles de validation, les contrôles liés aux boutons et aux boutons dans ASP.NET 2,0 incluent également une propriété `ValidationGroup`. La validité des validateurs d’un groupe de validation est vérifiée uniquement lorsqu’une publication (postback) est induite par un bouton qui a le même paramètre de propriété `ValidationGroup`. Par exemple, pour que le bouton d’insertion du contrôle DetailsView déclenche la `InsertValidationControls` groupe de validation, nous devons affecter à la propriété `ValidationGroup` de CommandField la valeur `InsertValidationControls` (voir figure 14). En outre, définissez la propriété CommandField’s `ValidationGroup` de GridView sur `EditValidationControls`.

[![définir la propriété ValidationGroup CommandField’s du contrôle DetailsView sur InsertValidationControls](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image41.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image40.png)

**Figure 14**: définir la propriété CommandField’s `ValidationGroup` du contrôle DetailsView sur `InsertValidationControls` ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image42.png))

Une fois ces modifications effectuées, les TemplateFields et CommandFields du contrôle DetailsView et GridView doivent ressembler à ce qui suit :

TemplateFields et CommandField du contrôle DetailsView

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample4.aspx)]

CommandField et TemplateFields de GridView

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample5.aspx)]

À ce stade, les contrôles de validation spécifiques à la modification se déclenchent uniquement quand l’utilisateur clique sur le bouton mettre à jour de GridView et que les contrôles de validation spécifiques à l’insertion se déclenchent uniquement quand l’utilisateur clique sur le bouton d’insertion du contrôle DetailsView, ce qui résout le problème mis en surbrillance à la figure 13. Toutefois, avec cette modification, notre contrôle ValidationSummary ne s’affiche plus lors de l’entrée de données non valides. Le contrôle ValidationSummary contient également une propriété `ValidationGroup` et affiche uniquement des informations récapitulatives pour ces contrôles de validation dans son groupe de validation. Par conséquent, nous devons disposer de deux contrôles de validation dans cette page, un pour le groupe de validation `InsertValidationControls` et un pour `EditValidationControls`.

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample6.aspx)]

Avec cet ajout, notre didacticiel est terminé.

## <a name="summary"></a>Récapitulatif

Bien que BoundFields puisse fournir à la fois une interface d’insertion et de modification, l’interface n’est pas personnalisable. En règle générale, nous souhaitons ajouter des contrôles de validation à l’interface de modification et d’insertion pour garantir que l’utilisateur entre les entrées requises dans un format légal. Pour ce faire, nous devons convertir le BoundFields en TemplateFields et ajouter les contrôles de validation au (x) modèle (s) approprié (s). Dans ce didacticiel, nous avons étendu l’exemple du didacticiel *examen des événements associés à l’insertion, à la mise à jour et à la suppression* , en ajoutant des contrôles de validation à l’interface d’insertion du contrôle DetailsView et à l’interface de modification du contrôle GridView. En outre, nous avons vu comment afficher des informations de validation récapitulatives à l’aide du contrôle ValidationSummary et comment partitionner les contrôles de validation sur la page en groupes de validation distincts.

Comme nous l’avons vu dans ce didacticiel, les TemplateFields permettent de compléter les interfaces de modification et d’insertion pour inclure des contrôles de validation. TemplateFields peut également être étendu pour inclure des contrôles Web d’entrée supplémentaires, ce qui permet de remplacer la zone de texte par un contrôle Web plus approprié. Dans notre prochain didacticiel, nous verrons comment remplacer le contrôle TextBox par un contrôle DropDownList lié aux données, ce qui est idéal lors de la modification d’une clé étrangère (par exemple `CategoryID` ou `SupplierID` dans la table `Products`).

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel étaient Liz Shulok et Zack Jones. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)
> [Suivant](customizing-the-data-modification-interface-vb.md)
