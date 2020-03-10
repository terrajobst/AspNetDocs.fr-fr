---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-vb
title: Ajout de contrôles de validation à l’interface de modification du contrôle DataList (VB) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons découvrir combien il est facile d’ajouter des contrôles de validation au contrôle EditItemTemplate de DataList afin de fournir un utilisateur de modification plus rapide...
ms.author: riande
ms.date: 10/30/2006
ms.assetid: 6b073fc6-524d-453d-be7c-0c30986de391
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: f952a7bb95e956a2ad935f8bdef5c3efa7437ecb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78594520"
---
# <a name="adding-validation-controls-to-the-datalists-editing-interface-vb"></a>Ajout de contrôles Validation aux interfaces de modification du contrôle DataList (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_39_VB.exe) ou [Télécharger le PDF](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/datatutorial39vb1.pdf)

> Dans ce didacticiel, nous allons découvrir combien il est facile d’ajouter des contrôles de validation au contrôle EditItemTemplate de DataList afin de fournir une interface utilisateur de modification plus simple.

## <a name="introduction"></a>Introduction

Dans les didacticiels de modification de DataList jusqu’à présent, les interfaces de modification DataLists n’ont pas inclus de validation d’entrée utilisateur proactive, même si une entrée utilisateur non valide, telle qu’un nom de produit manquant ou un prix négatif, entraîne une exception. Dans le [didacticiel précédent](handling-bll-and-dal-level-exceptions-vb.md) , nous avons examiné comment ajouter du code de gestion des exceptions au gestionnaire d’événements `UpdateCommand` DataList afin d’intercepter et d’afficher en douceur des informations sur les exceptions qui ont été levées. Dans l’idéal, toutefois, l’interface de modification inclut des contrôles de validation pour empêcher un utilisateur d’entrer de telles données non valides en premier lieu.

Dans ce didacticiel, nous allons voir combien il est facile d’ajouter des contrôles de validation aux `EditItemTemplate` DataList afin de fournir une interface utilisateur de modification plus simple. Plus précisément, ce didacticiel prend l’exemple créé dans le didacticiel précédent et augmente l’interface de modification pour inclure la validation appropriée.

## <a name="step-1-replicating-the-example-fromhandling-bll--and-dal-level-exceptions"></a>Étape 1 : réplication de l’exemple à partir de la[gestion des exceptions de niveau BLL et dal](handling-bll-and-dal-level-exceptions-vb.md)

Dans le didacticiel [gestion des exceptions de niveau BLL et dal](handling-bll-and-dal-level-exceptions-vb.md) , nous avons créé une page qui répertorie les noms et les prix des produits dans une DataList modifiable à deux colonnes. L’objectif de ce didacticiel est d’augmenter l’interface de modification de DataList pour inclure des contrôles de validation. En particulier, notre logique de validation :

- Exiger que le nom du produit soit fourni
- Vérifiez que la valeur entrée pour le prix est un format monétaire valide
- Vérifiez que la valeur entrée pour le prix est supérieure ou égale à zéro, car une valeur de `UnitPrice` négative n’est pas conforme

Avant de pouvoir examiner l’exemple précédent pour inclure la validation, nous devons d’abord répliquer l’exemple de la page `ErrorHandling.aspx` dans le dossier `EditDeleteDataList` vers la page de ce didacticiel, `UIValidation.aspx`. Pour ce faire, nous devons copier à la fois le balisage déclaratif de la page `ErrorHandling.aspx` et son code source. Tout d’abord, copiez le balisage déclaratif en effectuant les étapes suivantes :

1. Ouvrir la page `ErrorHandling.aspx` dans Visual Studio
2. Accédez à la balise déclarative page s (cliquez sur le bouton source en bas de la page)
3. Copiez le texte dans les balises `<asp:Content>` et `</asp:Content>` (lignes 3 à 32), comme illustré à la figure 1.

[![copier le texte dans le contrôle &lt;asp : content&gt;](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image2.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image1.png)

**Figure 2**: copier le texte dans le contrôle de `<asp:Content>` ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image3.png))

1. Ouvrir la page `UIValidation.aspx`
2. Atteindre le balisage déclaratif de la page
3. Collez le texte dans le contrôle `<asp:Content>`.

Pour copier le code source, ouvrez la page `ErrorHandling.aspx.vb` et copiez simplement le texte *dans* la classe `EditDeleteDataList_ErrorHandling`. Copiez les trois gestionnaires d’événements (`Products_EditCommand`, `Products_CancelCommand`et `Products_UpdateCommand`) avec la méthode `DisplayExceptionDetails`, mais ne copiez **pas** la déclaration de classe ou les instructions `using`. Collez le texte copié *dans* la classe `EditDeleteDataList_UIValidation` dans `UIValidation.aspx.vb`.

Après avoir déplacé le contenu et le code de `ErrorHandling.aspx` vers `UIValidation.aspx`, prenez un moment pour tester les pages dans un navigateur. Vous devez voir la même sortie et expérimenter les mêmes fonctionnalités dans chacune de ces deux pages (voir figure 2).

[![la page UIValidation. aspx imite les fonctionnalités de ErrorHandling. aspx](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image5.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image4.png)

**Figure 2**: la Page de `UIValidation.aspx` imite les fonctionnalités de `ErrorHandling.aspx` ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image6.png))

## <a name="step-2-adding-the-validation-controls-to-the-datalist-s-edititemtemplate"></a>Étape 2 : ajout des contrôles de validation à DataList s EditItemTemplate

Lors de la construction de formulaires de saisie de données, il est important que les utilisateurs entrent les champs obligatoires et que toutes les entrées fournies soient des valeurs valides et correctement mises en forme. Pour garantir la validité des entrées utilisateur, ASP.NET fournit cinq contrôles de validation intégrés qui sont conçus pour valider la valeur d’un contrôle Web d’entrée unique :

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) garantit qu’une valeur a été fournie
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) valide une valeur par rapport à une autre valeur de contrôle Web ou une valeur constante, ou garantit que le format de la valeur s est conforme pour un type de données spécifié
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) garantit qu’une valeur est comprise dans une plage de valeurs
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) valide une valeur par rapport à une [expression régulière](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) valide une valeur par rapport à une méthode personnalisée définie par l’utilisateur

Pour plus d’informations sur ces cinq contrôles, reportez-vous au didacticiel [Ajout de contrôles de validation au didacticiel de modification et d’insertion d’interfaces](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) ou consultez la [section contrôles de validation](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/validation/default.aspx) des [didacticiels de démarrage rapide ASP.net](https://quickstarts.asp.net).

Pour notre didacticiel, nous allons utiliser un RequiredFieldValidator pour garantir qu’une valeur a été fournie pour le nom de produit et un CompareValidator pour garantir que le prix entré a une valeur supérieure ou égale à 0 et qu’il est présenté dans un format monétaire valide.

> [!NOTE]
> Alors que ASP.NET 1. x contenait ces cinq mêmes contrôles de validation, ASP.NET 2,0 a ajouté un certain nombre d’améliorations, les deux principaux étant la prise en charge des scripts côté client pour les navigateurs, en plus d’Internet Explorer et la possibilité de partitionner des contrôles de validation sur une page dans groupes de validation. Pour plus d’informations sur les nouvelles fonctionnalités de contrôle de validation dans 2,0, consultez [la section discroisant les contrôles de validation dans ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).

Commençons par ajouter les contrôles de validation nécessaires à la `EditItemTemplate`de la DataList. Cette tâche peut être effectuée par le biais du concepteur en cliquant sur le lien modifier les modèles de la balise active de DataList s ou à l’aide de la syntaxe déclarative. Passons au processus à l’aide de l’option modifier les modèles de la Mode Création. Après avoir choisi de modifier le `EditItemTemplate`DataList, ajoutez un RequiredFieldValidator en le faisant glisser de la boîte à outils vers l’interface de modification de modèle, en le plaçant après la zone de texte `ProductName`.

[![ajouter un RequiredFieldValidator à EditItemTemplate après la zone de texte ProductName](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image8.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image7.png)

**Figure 3**: ajouter un RequiredFieldValidator au `EditItemTemplate After` la zone de texte `ProductName` ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image9.png))

Tous les contrôles de validation fonctionnent en validant l’entrée d’un seul contrôle Web ASP.NET. Par conséquent, nous devons indiquer que le contrôle RequiredFieldValidator que nous venons d’ajouter doit effectuer une validation par rapport à la zone de texte `ProductName`. pour ce faire, affectez à la propriété contrôle de validation s [`ControlToValidate`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) la `ID` du contrôle Web approprié (`ProductName`, dans cette instance). Ensuite, affectez à la [propriété`ErrorMessage`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) la valeur vous devez fournir le nom du produit et la [propriété`Text`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) pour \*. La valeur de la propriété `Text`, si elle est fournie, est le texte affiché par le contrôle de validation si la validation échoue. La valeur de la propriété `ErrorMessage`, qui est obligatoire, est utilisée par le contrôle ValidationSummary ; Si la valeur de la propriété `Text` est omise, la valeur de la propriété `ErrorMessage` est affichée par le contrôle de validation sur une entrée non valide.

Après avoir défini ces trois propriétés de RequiredFieldValidator, votre écran doit ressembler à la figure 4.

[![définir les propriétés RequiredFieldValidator, ErrorMessage et Text de RequiredFieldValidator](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image11.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image10.png)

**Figure 4**: définir les propriétés de l' `ControlToValidate`, `ErrorMessage`et `Text` de RequiredFieldValidator ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image12.png))

Une fois le contrôle RequiredFieldValidator ajouté au `EditItemTemplate`, il vous reste à ajouter la validation nécessaire pour la zone de texte Price s. Étant donné que le `UnitPrice` est facultatif lors de la modification d’un enregistrement, nous n’avons pas besoin d’ajouter un RequiredFieldValidator. Toutefois, nous devons ajouter un CompareValidator pour vous assurer que le `UnitPrice`, s’il est fourni, est correctement mis en forme en tant que devise et supérieur ou égal à 0.

Ajoutez CompareValidator à la `EditItemTemplate` et affectez à sa propriété `ControlToValidate` la valeur `UnitPrice`, sa propriété `ErrorMessage` au prix doit être supérieure ou égale à zéro et ne peut pas inclure le symbole monétaire, et sa propriété `Text` à \*. Pour indiquer que la valeur de `UnitPrice` doit être supérieure ou égale à 0, affectez à la [propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) CompareValidator s`Operator` la valeur `GreaterThanEqual`, à sa [propriété`ValueToCompare`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) la valeur 0 et à la [propriété`Type`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) la valeur `Currency`.

Après avoir ajouté ces deux contrôles de validation, la syntaxe déclarative des `EditItemTemplate` s de DataList doit ressembler à ce qui suit :

[!code-aspx[Main](adding-validation-controls-to-the-datalist-s-editing-interface-vb/samples/sample1.aspx)]

Après avoir apporté ces modifications, ouvrez la page dans un navigateur. Si vous tentez d’omettre le nom ou d’entrer une valeur de prix non valide lors de la modification d’un produit, un astérisque apparaît en regard de la zone de texte. Comme le montre la figure 5, une valeur de prix qui comprend le symbole monétaire, tel que $19,95, est considérée comme non valide. L' `Currency` CompareValidator s `Type` autorise des séparateurs numériques (tels que des virgules ou des points, selon les paramètres de culture) et un signe plus ou moins de début, mais n’autorise *pas* de symbole monétaire. Ce comportement peut déguiser les utilisateurs, car l’interface de modification restitue actuellement le `UnitPrice` à l’aide du format monétaire.

[![un astérisque apparaît en regard des zones de texte avec une entrée non valide](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image14.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image13.png)

**Figure 5**: un astérisque apparaît en regard des zones de texte avec une entrée non valide ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image15.png))

Tandis que la validation fonctionne telle quelle, l’utilisateur doit supprimer manuellement le symbole monétaire lors de la modification d’un enregistrement, ce qui n’est pas acceptable. De plus, s’il y a des entrées non valides dans l’interface de modification, ni les boutons Update ni Cancel, lorsque vous cliquez dessus, appellent une publication (postback). Dans l’idéal, le bouton Annuler retourne le contrôle DataList à son état antérieur à la modification, quelle que soit la validité des entrées de l’utilisateur. En outre, nous devons nous assurer que les données de la page sont valides avant de mettre à jour les informations sur le produit dans le gestionnaire d’événements de la DataList `UpdateCommand`, car la validation de la logique côté client peut être ignorée par les utilisateurs dont les navigateurs ne prennent pas en charge JavaScript ou dont la prise en charge est désactivée.

## <a name="removing-the-currency-symbol-from-the-edititemtemplate-s-unitprice-textbox"></a>Suppression du symbole monétaire de la zone de texte PrixUnitaire s UnitPrice

Lorsque vous utilisez CompareValidator s `Currency``Type`, l’entrée en cours de validation ne doit pas inclure de symboles monétaires. La présence de tels symboles fait que CompareValidator marque l’entrée comme non valide. Toutefois, notre interface de modification comprend actuellement un symbole monétaire dans la zone de texte `UnitPrice`, ce qui signifie que l’utilisateur doit supprimer explicitement le symbole monétaire avant d’enregistrer les modifications. Pour y remédier, trois options s’offrent à vous :

1. Configurez la `EditItemTemplate` afin que la valeur de la zone de texte `UnitPrice` ne soit pas mise en forme en tant que devise.
2. Autorisez l’utilisateur à entrer un symbole monétaire en supprimant CompareValidator et en le remplaçant par un RegularExpressionValidator qui vérifie si une valeur monétaire est correctement mise en forme. Le défi ici est que l’expression régulière pour valider une valeur monétaire n’est pas aussi simple que CompareValidator et nécessiterait l’écriture de code si nous voulions incorporer des paramètres de culture.
3. Supprimez complètement le contrôle de validation et reposez-vous sur la logique de validation côté serveur personnalisée dans le gestionnaire d’événements `RowUpdating` GridView s.

Passons à l’option 1 pour ce didacticiel. Actuellement, le `UnitPrice` est mis en forme en tant que valeur monétaire en raison de l’expression de liaison de données pour la zone de texte dans le `EditItemTemplate`: `<%# Eval("UnitPrice", "{0:c}") %>`. Remplacez l’instruction `Eval` par `Eval("UnitPrice", "{0:n2}")`, qui met en forme le résultat sous la forme d’un nombre avec deux chiffres de précision. Vous pouvez effectuer cette opération directement par le biais de la syntaxe déclarative ou en cliquant sur le lien modifier les DataBindings dans la zone de texte `UnitPrice` de la `EditItemTemplate`DataList.

Avec cette modification, le prix mis en forme dans l’interface de modification comprend des virgules comme séparateur de groupes et un point comme séparateur décimal, mais ne quitte pas le symbole monétaire.

> [!NOTE]
> Lors de la suppression du format monétaire de l’interface modifiable, je trouve qu’il est utile de placer le symbole monétaire sous forme de texte en dehors de la zone de texte. Cela permet d’indiquer à l’utilisateur qu’il n’est pas nécessaire de fournir le symbole monétaire.

## <a name="fixing-the-cancel-button"></a>Correction du bouton Annuler

Par défaut, les contrôles Web de validation émettent du code JavaScript pour effectuer la validation côté client. Lorsqu’un utilisateur clique sur un bouton, LinkButton ou ImageButton, les contrôles de validation sur la page sont vérifiés côté client avant que la publication (postback) ne se produise. Si des données ne sont pas valides, la publication est annulée. Pour certains boutons, toutefois, la validité des données peut être imprécise ; dans ce cas, l’annulation de la publication en raison de données non valides est une gêne.

Le bouton Annuler est un exemple de ce type. Imaginez qu’un utilisateur entre des données non valides, par exemple en omettant le nom du produit, puis décide qu’il ne souhaite pas enregistrer le produit après tout et clique sur le bouton Annuler. Actuellement, le bouton Annuler déclenche les contrôles de validation sur la page, ce qui indique que le nom du produit est manquant et empêche la publication (postback). Notre utilisateur doit taper du texte dans la zone de texte `ProductName` pour annuler le processus de modification.

Heureusement, le bouton, LinkButton et ImageButton ont une [propriété`CausesValidation`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.causesvalidation.aspx) qui peut indiquer si un clic sur le bouton doit initialiser la logique de validation (la valeur par défaut est `True`). Définissez le bouton Annuler `CausesValidation` propriété sur `False`.

## <a name="ensuring-the-inputs-are-valid-in-the-updatecommand-event-handler"></a>S’assurer que les entrées sont valides dans le gestionnaire d’événements UpdateCommand

En raison du script côté client émis par les contrôles de validation, si un utilisateur entre une entrée non valide, les contrôles de validation annulent toutes les publications initiées par les contrôles Button, LinkButton ou ImageButton dont les propriétés `CausesValidation` sont `True` (valeur par défaut). Toutefois, si un utilisateur visite avec un navigateur archaïque ou un navigateur dont la prise en charge de JavaScript a été désactivée, les contrôles de validation côté client ne s’exécutent pas.

Tous les contrôles de validation ASP.NET répètent immédiatement leur logique de validation lors de la publication (postback) et signalent la validité globale des entrées de la page via la [propriété`Page.IsValid`](https://msdn.microsoft.com/library/system.web.ui.page.isvalid.aspx). Toutefois, le workflow de page n’est pas interrompu ou arrêté en fonction de la valeur de `Page.IsValid`. En tant que développeurs, il est de notre responsabilité de s’assurer que la propriété `Page.IsValid` a une valeur de `True` avant de procéder au code qui suppose des données d’entrée valides.

Si JavaScript est désactivé pour un utilisateur, consulte notre page, modifie un produit, entre une valeur de prix trop coûteuse, puis clique sur le bouton mettre à jour, la validation côté client est ignorée et une publication est effectuée. Lors de la publication (postback), le gestionnaire d’événements ASP.NET pages s `UpdateCommand` s’exécute et une exception est levée lors d’une tentative d’analyse trop coûteuse pour un `Decimal`. Étant donné que nous disposons d’une gestion des exceptions, une telle exception sera gérée correctement, mais nous pourrions empêcher le glissement des données non valides en premier lieu en poursuivant uniquement avec le gestionnaire d’événements `UpdateCommand` si `Page.IsValid` a la valeur `True`.

Ajoutez le code suivant au début du gestionnaire d’événements `UpdateCommand`, juste avant le bloc `Try` :

[!code-vb[Main](adding-validation-controls-to-the-datalist-s-editing-interface-vb/samples/sample2.vb)]

Avec cet ajout, le produit tentera d’être mis à jour uniquement si les données envoyées sont valides. La plupart des utilisateurs ne sont pas en mesure de publier des données non valides en raison des contrôles de validation des scripts côté client, mais les utilisateurs dont les navigateurs ne prennent pas en charge JavaScript ou qui ont la prise en charge de JavaScript sont désactivés, peuvent contourner les vérifications côté client et envoyer des données non valides.

> [!NOTE]
> Le lecteur astucieux rappelle que lors de la mise à jour des données avec le GridView, nous n’avons pas besoin de vérifier explicitement la propriété `Page.IsValid` dans notre classe code-behind de la page. Cela est dû au fait que le contrôle GridView consulte la propriété `Page.IsValid` pour nous et ne procède à la mise à jour que si elle retourne une valeur de `True`.

## <a name="step-3-summarizing-data-entry-problems"></a>Étape 3 : synthèse des problèmes d’entrée de données

Outre les cinq contrôles de validation, ASP.NET comprend le [contrôle ValidationSummary](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), qui affiche le `ErrorMessage` s de ces contrôles de validation qui ont détecté des données non valides. Ces données de synthèse peuvent être affichées sous forme de texte sur la page Web ou par le biais d’une MessageBox modale côté client. Nous allons améliorer ce didacticiel pour inclure une MessageBox côté client résumant tous les problèmes de validation.

Pour ce faire, faites glisser un contrôle ValidationSummary de la boîte à outils vers le concepteur. L’emplacement du contrôle ValidationSummary n’a pas vraiment d’importance, car nous allons le configurer pour afficher uniquement le résumé en tant que MessageBox. Après avoir ajouté le contrôle, définissez sa [propriété`ShowSummary`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) sur `False` et sa [propriété`ShowMessageBox`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) sur `True`. Dans ce cas, toutes les erreurs de validation sont résumées dans une MessageBox côté client (voir figure 6).

[![les erreurs de validation sont résumées dans la MessageBox côté client](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image17.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image16.png)

**Figure 6**: les erreurs de validation sont résumées dans la MessageBox côté client ([cliquez pour afficher l’image en taille réelle](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image18.png))

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, nous avons vu comment réduire la probabilité d’exceptions en utilisant des contrôles de validation pour garantir de manière proactive que les entrées de nos utilisateurs sont valides avant de tenter de les utiliser dans le flux de travail de mise à jour. ASP.NET fournit cinq contrôles Web de validation qui sont conçus pour inspecter une entrée de contrôle Web particulière et un rapport sur la validité des e/s. Dans ce didacticiel, nous avons utilisé deux de ces cinq contrôles RequiredFieldValidator et CompareValidator pour garantir que le nom du produit a été fourni et que le prix avait un format monétaire avec une valeur supérieure ou égale à zéro.

Pour ajouter des contrôles de validation à l’interface de modification de DataList, il suffit de les faire glisser sur le `EditItemTemplate` à partir de la boîte à outils et de définir une poignée de propriétés. Par défaut, les contrôles de validation émettent automatiquement un script de validation côté client. ils fournissent également la validation côté serveur lors de la publication (postback), en stockant le résultat cumulé dans la propriété `Page.IsValid`. Pour contourner la validation côté client en cas de clic sur un bouton, LinkButton ou ImageButton, définissez la propriété Button s `CausesValidation` sur `False`. En outre, avant d’effectuer des tâches avec les données soumises lors de la publication (postback), assurez-vous que la propriété `Page.IsValid` retourne `True`.

Tous les didacticiels de modification de DataList que nous avons examinés jusqu’à présent ont eu des interfaces de modification très simples, une zone de texte pour le nom du produit et une autre pour le prix. Toutefois, l’interface de modification peut contenir un mélange de différents contrôles Web, tels que DropDownList, des calendriers, des cases d’option, des cases à cocher, etc. Dans notre prochain didacticiel, nous examinerons la création d’une interface qui utilise une variété de contrôles Web.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Les réviseurs de leads pour ce didacticiel étaient Denis Patterson, Ken Pespisa et Liz Shulok. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](handling-bll-and-dal-level-exceptions-vb.md)
> [Suivant](customizing-the-datalist-s-editing-interface-vb.md)
