---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-cs
title: Gestion des exceptions de niveau BLL et DAL (C#) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons voir comment tactfully gérer les exceptions déclenchées pendant un flux de travail de mise à jour de DataList modifiable.
ms.author: riande
ms.date: 10/30/2006
ms.assetid: f8fd58e2-f932-4f08-ab3d-fbf8ff3295d2
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-cs
msc.type: authoredcontent
ms.openlocfilehash: 35ff60be6ed67ea8d1bf226ae70f590100597757
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78610949"
---
# <a name="handling-bll--and-dal-level-exceptions-c"></a>Gestion des exceptions de niveau BLL et DAL (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_38_CS.exe) ou [Télécharger le PDF](handling-bll-and-dal-level-exceptions-cs/_static/datatutorial38cs1.pdf)

> Dans ce didacticiel, nous allons voir comment tactfully gérer les exceptions déclenchées pendant un flux de travail de mise à jour de DataList modifiable.

## <a name="introduction"></a>Introduction

Dans la [vue d’ensemble de la modification et de la suppression des données dans le didacticiel DataList](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md) , nous avons créé un contrôle DataList qui offrait des fonctionnalités simples de modification et de suppression. Bien qu’elle soit entièrement fonctionnelle, elle n’était pas conviviale, car toute erreur qui s’est produite pendant le processus de modification ou de suppression a entraîné une exception non gérée. Par exemple, si vous omettez le nom du produit ou, lors de la modification d’un produit, l’entrée d’une valeur de prix très abordable, lève une exception. Étant donné que cette exception n’est pas interceptée dans le code, elle se propage jusqu’au runtime ASP.NET, qui affiche ensuite les détails de l’exception dans la page Web.

Comme nous l’avons vu dans la [gestion des exceptions de niveau BLL et dal dans un didacticiel de Page ASP.net](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) , si une exception est levée à partir des profondeurs de la logique métier ou des couches d’accès aux données, les détails de l’exception sont retournés à l’ObjectDataSource, puis au GridView. Nous avons vu comment gérer correctement ces exceptions en créant `Updated` ou `RowUpdated` gestionnaires d’événements pour ObjectDataSource ou GridView, en vérifiant une exception, puis en indiquant que l’exception a été gérée.

Nos didacticiels DataList, toutefois, n’utilisent pas l’ObjectDataSource pour mettre à jour et supprimer des données. Au lieu de cela, nous travaillons directement par rapport à la couche BLL. Afin de détecter les exceptions provenant de la couche BLL ou de la couche DAL, nous devons implémenter le code de gestion des exceptions dans le code-behind de notre page ASP.NET. Dans ce didacticiel, nous allons voir comment plus de tactfully gérer les exceptions déclenchées lors de la mise à jour d’un flux de travail modifiable DataList.

> [!NOTE]
> Dans la *vue d’ensemble de la modification et de la suppression de données dans le didacticiel DataList* , nous avons abordé différentes techniques de modification et de suppression de données du contrôle DataList, certaines techniques impliquées dans l’utilisation d’un ObjectDataSource pour la mise à jour et la suppression. Si vous employez ces techniques, vous pouvez gérer les exceptions à partir de la couche BLL ou de la couche DAL par le biais de l' `Updated` ObjectDataSource s ou `Deleted` gestionnaires d’événements.

## <a name="step-1-creating-an-editable-datalist"></a>Étape 1 : création d’un contrôle DataList modifiable

Avant de vous soucier de la gestion des exceptions qui se produisent pendant le flux de travail de mise à jour, créez d’abord un contrôle DataList modifiable. Ouvrez la page `ErrorHandling.aspx` dans le dossier `EditDeleteDataList`, ajoutez un contrôle DataList au concepteur, définissez sa propriété `ID` sur `Products`, puis ajoutez un nouvel ObjectDataSource nommé `ProductsDataSource`. Configurez le ObjectDataSource pour utiliser la méthode de `GetProducts()` de la classe `ProductsBLL` pour sélectionner des enregistrements ; Définissez les listes déroulantes des onglets insertion, mise à jour et suppression sur (aucune).

[![retourner les informations sur le produit à l’aide de la méthode GetProducts ()](handling-bll-and-dal-level-exceptions-cs/_static/image2.png)](handling-bll-and-dal-level-exceptions-cs/_static/image1.png)

**Figure 1**: renvoyer les informations sur le produit à l’aide de la méthode `GetProducts()` ([cliquez pour afficher l’image en taille réelle](handling-bll-and-dal-level-exceptions-cs/_static/image3.png))

Une fois l’exécution de l’Assistant ObjectDataSource terminée, Visual Studio crée automatiquement un `ItemTemplate` pour la DataList. Remplacez ceci par une `ItemTemplate` qui affiche le nom et le prix de chaque produit et comprend un bouton modifier. Ensuite, créez un `EditItemTemplate` avec un contrôle Web TextBox pour les boutons Name et Price et Update et Cancel. Enfin, affectez la valeur 2 à la propriété DataList s `RepeatColumns`.

Une fois ces modifications effectuées, le balisage déclaratif de votre page doit ressembler à ce qui suit. Double-Vérifiez pour vous assurer que les propriétés `CommandName` des propriétés Edit, Cancel et Update sont définies respectivement sur modifier, annuler et mettre à jour.

[!code-aspx[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample1.aspx)]

> [!NOTE]
> Pour ce didacticiel, l’état d’affichage de DataList s doit être activé.

Prenez un moment pour voir notre progression dans un navigateur (voir figure 2).

[![chaque produit comprend un bouton modifier](handling-bll-and-dal-level-exceptions-cs/_static/image5.png)](handling-bll-and-dal-level-exceptions-cs/_static/image4.png)

**Figure 2**: chaque produit comprend un bouton modifier ([cliquez pour afficher l’image en taille réelle](handling-bll-and-dal-level-exceptions-cs/_static/image6.png))

Actuellement, le bouton modifier génère uniquement une publication (postback). il n’est pas encore modifiable. Pour permettre la modification, nous devons créer des gestionnaires d’événements pour les événements `EditCommand`, `CancelCommand`et `UpdateCommand` de DataList. Les événements `EditCommand` et `CancelCommand` mettent simplement à jour la propriété DataList s `EditItemIndex` et relient les données au contrôle DataList :

[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample2.cs)]

Le gestionnaire d’événements `UpdateCommand` est un peu plus complexe. Il doit lire le `ProductID` du produit modifié à partir de la collection de `DataKeys`, ainsi que le nom et le prix du produit dans les zones de texte de l' `EditItemTemplate`, puis appeler la méthode `ProductsBLL` classe s `UpdateProduct` avant de rétablir le contrôle DataList à son état antérieur à la modification.

Pour le moment, il suffit d’utiliser exactement le même code du gestionnaire d’événements `UpdateCommand` dans la *vue d’ensemble de la modification et de la suppression des données dans le didacticiel DataList* . Nous allons ajouter le code pour gérer correctement les exceptions à l’étape 2.

[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample3.cs)]

Face à une entrée non valide qui peut se présenter sous la forme d’un prix unitaire mal mis en forme, une valeur de prix unitaire non conforme comme-$5,00, ou l’omission du nom du produit, une exception est levée. Étant donné que le gestionnaire d’événements `UpdateCommand` n’inclut pas de code de gestion des exceptions à ce stade, l’exception se propage au runtime ASP.NET, où elle sera affichée à l’utilisateur final (voir figure 3).

![Lorsqu’une exception non gérée se produit, l’utilisateur final voit une page d’erreur](handling-bll-and-dal-level-exceptions-cs/_static/image7.png)

**Figure 3**: lorsqu’une exception non gérée se produit, l’utilisateur final voit une page d’erreur

## <a name="step-2-gracefully-handling-exceptions-in-the-updatecommand-event-handler"></a>Étape 2 : gestion en douceur des exceptions dans le gestionnaire d’événements UpdateCommand

Pendant le flux de travail de mise à jour, des exceptions peuvent se produire dans le gestionnaire d’événements `UpdateCommand`, la couche BLL ou la couche DAL. Par exemple, si un utilisateur entre un prix trop onéreux, l’instruction `Decimal.Parse` dans le gestionnaire d’événements `UpdateCommand` lèvera une exception `FormatException`. Si l’utilisateur omet le nom du produit ou si le prix a une valeur négative, la couche DAL lève une exception.

Quand une exception se produit, nous souhaitons afficher un message d’information dans la page elle-même. Ajoutez un contrôle Web Label à la page dont la `ID` a la valeur `ExceptionDetails`. Configurez le texte de l’étiquette pour qu’il s’affiche dans une police rouge, très grande, en gras et en italique en affectant sa propriété `CssClass` à la classe CSS `Warning`, qui est définie dans le fichier `Styles.css`.

Lorsqu’une erreur se produit, nous voulons uniquement que l’étiquette s’affiche une seule fois. Autrement dit, lors des publications (postbacks) suivantes, le message d’avertissement de l’étiquette doit disparaître. Pour ce faire, vous pouvez effacer la propriété label s `Text` ou les paramètres de sa propriété `Visible` à `False` dans le gestionnaire d’événements `Page_Load` (comme nous l’avons fait pour la [gestion des exceptions de niveau BLL et dal dans un didacticiel de Page ASP.net](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) ) ou en désactivant la prise en charge de l’état d’affichage des étiquettes. Essayons d’utiliser cette dernière option.

[!code-aspx[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample4.aspx)]

Quand une exception est levée, nous attribuons les détails de l’exception à la propriété `ExceptionDetails` label Control s `Text`. Étant donné que son état d’affichage est désactivé, lors des publications (postbacks) suivantes, les modifications de programmation de `Text` propriété s seront perdues, rétablissant ainsi le texte par défaut (une chaîne vide), masquant ainsi le message d’avertissement.

Pour déterminer quand une erreur a été déclenchée afin d’afficher un message utile sur la page, nous devons ajouter un bloc `Try ... Catch` au gestionnaire d’événements `UpdateCommand`. La partie `Try` contient du code qui peut mener à une exception, tandis que le bloc `Catch` contient du code qui est exécuté en face d’une exception. Pour plus d’informations sur le bloc `Try ... Catch`, consultez la section notions de base de la [gestion des exceptions](https://msdn.microsoft.com/library/2w8f0bss.aspx) dans la documentation de .NET Framework.

[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample5.cs)]

Lorsqu’une exception de tout type est levée par du code dans le bloc `Try`, le code du bloc de `Catch` commence à s’exécuter. Le type d’exception qui est levé `DbException`, `NoNullAllowedException`, `ArgumentException`, etc. dépend de ce qui, exactement, a déclenché l’erreur en premier lieu. Si un problème survient au niveau de la base de données, une `DbException` est levée. Si une valeur non conforme est entrée pour les champs `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`ou `ReorderLevel`, une `ArgumentException` est levée, puisque nous avons ajouté du code pour valider ces valeurs de champ dans la classe `ProductsDataTable` (consultez le didacticiel [création d’une couche logique métier](../introduction/creating-a-business-logic-layer-cs.md) ).

Nous pouvons fournir une explication plus utile à l’utilisateur final en basant le texte du message sur le type d’exception intercepté. Le code suivant qui a été utilisé dans un formulaire presque identique dans la [gestion des exceptions de niveau BLL et dal dans un didacticiel de Page ASP.net](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) fournit ce niveau de détail :

[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample6.cs)]

Pour suivre ce didacticiel, il vous suffit d’appeler la méthode `DisplayExceptionDetails` à partir du bloc `Catch` en passant l’instance `Exception` interceptée (`ex`).

Avec le bloc `Try ... Catch` en place, les utilisateurs voient un message d’erreur plus informatif, comme les figures 4 et 5. Notez qu’en cas d’exception, la DataList reste en mode édition. En effet, une fois que l’exception se produit, le workflow de contrôle est immédiatement redirigé vers le bloc `Catch`, ignorant ainsi le code qui retourne la DataList à son état antérieur à la modification.

[![un message d’erreur s’affiche si un utilisateur omet un champ obligatoire](handling-bll-and-dal-level-exceptions-cs/_static/image9.png)](handling-bll-and-dal-level-exceptions-cs/_static/image8.png)

**Figure 4**: un message d’erreur s’affiche si un utilisateur omet un champ obligatoire ([cliquez pour afficher l’image en taille réelle](handling-bll-and-dal-level-exceptions-cs/_static/image10.png))

[![un message d’erreur s’affiche lors de l’entrée d’un prix négatif](handling-bll-and-dal-level-exceptions-cs/_static/image12.png)](handling-bll-and-dal-level-exceptions-cs/_static/image11.png)

**Figure 5**: un message d’erreur s’affiche lors de l’entrée d’un prix négatif ([cliquez pour afficher l’image en taille réelle](handling-bll-and-dal-level-exceptions-cs/_static/image13.png))

## <a name="summary"></a>Récapitulatif

Les contrôles GridView et ObjectDataSource fournissent des gestionnaires d’événements qui incluent des informations sur les exceptions levées pendant le flux de travail de mise à jour et de suppression, ainsi que des propriétés qui peuvent être définies pour indiquer si l’exception a été ou non Gérai. Toutefois, ces fonctionnalités ne sont pas disponibles lors de l’utilisation du contrôle DataList et de l’utilisation directe de la couche BLL. Au lieu de cela, nous sommes responsables de l’implémentation de la gestion des exceptions.

Dans ce didacticiel, nous avons vu comment ajouter la gestion des exceptions à un flux de travail de mise à jour des contrôles DataList modifiables en ajoutant un bloc `Try ... Catch` au gestionnaire d’événements `UpdateCommand`. Si une exception est levée pendant le flux de travail de mise à jour, le code de bloc s `Catch` s’exécute et affiche des informations utiles dans l’étiquette `ExceptionDetails`.

À ce stade, la DataList ne fait aucun effort pour empêcher les exceptions de se produire en premier lieu. Même si nous savons qu’un prix négatif entraîne une exception, nous n’avons pas encore ajouté de fonctionnalité pour empêcher l’utilisateur d’entrer de telles entrées non valides de manière proactive. Dans notre prochain didacticiel, nous verrons comment réduire les exceptions provoquées par une entrée d’utilisateur non valide en ajoutant des contrôles de validation dans la `EditItemTemplate`.

Bonne programmation !

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Instructions de conception pour les exceptions](https://msdn.microsoft.com/library/ms298399.aspx)
- [Erreurs de modules et de gestionnaires de journalisation (ELMAH)](http://workspaces.gotdotnet.com/elmah) (bibliothèque open source pour la journalisation des erreurs)
- [Enterprise Library pour .NET Framework 2,0](https://www.microsoft.com/downloads/details.aspx?familyid=5A14E870-406B-4F2A-B723-97BA84AE80B5&amp;displaylang=en) (comprend le bloc d’application de gestion des exceptions)

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept livres ASP/ASP. net et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec des technologies Web Microsoft depuis 1998. Scott travaille en tant que consultant, formateur et auteur indépendant. Son dernier livre est [*Sams vous apprend vous-même ASP.NET 2,0 en 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve sur [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements à

Cette série de didacticiels a été examinée par de nombreux réviseurs utiles. Le réviseur de leads pour ce didacticiel était Ken Pespisa. Vous souhaitez revoir mes prochains articles MSDN ? Si c’est le cas, insérez une ligne sur [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](performing-batch-updates-cs.md)
> [Suivant](adding-validation-controls-to-the-datalist-s-editing-interface-cs.md)
