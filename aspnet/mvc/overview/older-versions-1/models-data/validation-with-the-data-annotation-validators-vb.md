---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
title: Validation avec les validateurs d’annotation de données (VB) | Microsoft Docs
author: microsoft
description: Tirez parti du Binder de modèle d’annotation de données pour effectuer la validation dans une application MVC ASP.NET. Découvrez comment utiliser les différents types de validateurs...
ms.author: riande
ms.date: 05/29/2009
ms.assetid: 0d23ff2b-f2ec-434a-be3b-1180beeccba3
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
msc.type: authoredcontent
ms.openlocfilehash: 00150575baabc659f7dd0c07349cde52105f6c8b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78542083"
---
# <a name="validation-with-the-data-annotation-validators-vb"></a>Validation avec les validateurs d’annotation de données (VB)

par [Microsoft](https://github.com/microsoft)

> Tirez parti du Binder de modèle d’annotation de données pour effectuer la validation dans une application MVC ASP.NET. Découvrez comment utiliser les différents types d’attributs de validateur et les utiliser dans le Entity Framework Microsoft.

Dans ce didacticiel, vous allez apprendre à utiliser les validateurs d’annotation de données pour effectuer la validation dans une application MVC ASP.NET. L’avantage de l’utilisation des validateurs d’annotation de données est qu’ils vous permettent d’effectuer une validation simplement en ajoutant un ou plusieurs attributs, tels que l’attribut Required ou StringLength, à une propriété de classe.

Avant de pouvoir utiliser les validateurs d’annotation de données, vous devez télécharger le Binder de modèle d’annotations de données. Vous pouvez télécharger l’exemple de Binder de modèle d’annotations de données à partir du site Web CodePlex en cliquant [ici](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).

Il est important de comprendre que le Binder de modèle d’annotations de données n’est pas une partie officielle de l’infrastructure Microsoft ASP.NET MVC. Bien que le Binder de modèle d’annotations de données ait été créé par l’équipe Microsoft ASP.NET MVC, Microsoft n’offre pas de support produit officiel pour le Binder de modèle d’annotations de données décrit et utilisé dans ce didacticiel.

## <a name="using-the-data-annotation-model-binder"></a>Utilisation du Binder de modèle d’annotation de données

Pour pouvoir utiliser le Binder de modèle d’annotations de données dans une application ASP.NET MVC, vous devez d’abord ajouter une référence à l’assembly Microsoft. Web. Mvc. DataAnnotations. dll et à l’assembly System. ComponentModel. DataAnnotations. dll. Sélectionnez l’option de menu **projet, ajouter une référence**. Ensuite, cliquez sur l’onglet **Parcourir** et accédez à l’emplacement où vous avez téléchargé (et décompressé) l’exemple de Binder de modèle d’annotations de données (voir la **figure 1**).

[![](validation-with-the-data-annotation-validators-vb/_static/image2.png)](validation-with-the-data-annotation-validators-vb/_static/image1.png)

**Figure 1**: ajout d’une référence au classeur de modèles d’annotations de données ([cliquez pour afficher l’image en taille réelle](validation-with-the-data-annotation-validators-vb/_static/image3.png))

Sélectionnez l’assembly Microsoft. Web. Mvc. DataAnnotations. dll et l’assembly System. ComponentModel. DataAnnotations. dll, puis cliquez sur le bouton **OK** .

Vous ne pouvez pas utiliser l’assembly System. ComponentModel. DataAnnotations. dll inclus avec .NET Framework Service Pack 1 avec le Binder de modèle d’annotations de données. Vous devez utiliser la version de l’assembly System. ComponentModel. DataAnnotations. dll inclus avec le téléchargement de l’exemple de Binder de modèle d’annotations de données.

Enfin, vous devez inscrire le Binder de modèle DataAnnotations dans le fichier global. asax. Ajoutez la ligne de code suivante au gestionnaire d’événements de l’application\_Start () afin que la méthode d’application\_Start () ressemble à ceci :

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample1.vb)]

Cette ligne de code inscrit le DataAnnotationsModelBinder comme Binder de modèle par défaut pour l’application ASP.NET MVC entière.

## <a name="using-the-data-annotation-validator-attributes"></a>Utilisation des attributs du validateur d’annotation de données

Lorsque vous utilisez le Binder de modèle d’annotations de données, vous utilisez des attributs de validateur pour effectuer la validation. L’espace de noms System. ComponentModel. DataAnnotations comprend les attributs de validateur suivants :

- Plage : vous permet de vérifier si la valeur d’une propriété est comprise entre une plage de valeurs spécifiée.
- RegularExpression : vous permet de vérifier si la valeur d’une propriété correspond à un modèle d’expression régulière spécifié.
- Obligatoire : vous permet de marquer une propriété en fonction des besoins.
- StringLength : vous permet de spécifier une longueur maximale pour une propriété de type chaîne.
- Validation : classe de base pour tous les attributs de validateur.

> [!NOTE] 
> 
> Si vos besoins de validation ne sont pas satisfaisants par l’un des validateurs standard, vous avez toujours la possibilité de créer un attribut de validateur personnalisé en héritant d’un nouvel attribut de validateur de l’attribut de validation de base.

La classe Product de la **Liste 1** illustre comment utiliser ces attributs de validateur. Les propriétés Name, description et UnitPrice sont marquées comme obligatoires. La propriété Name doit avoir une longueur de chaîne inférieure à 10 caractères. Enfin, la propriété UnitPrice doit correspondre à un modèle d’expression régulière qui représente un montant en devise.

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample2.vb)]

**Liste 1**: Models\Product.vb

La classe Product illustre l’utilisation d’un attribut supplémentaire : l’attribut DisplayName. L’attribut DisplayName vous permet de modifier le nom de la propriété lorsque la propriété est affichée dans un message d’erreur. Au lieu d’afficher le message d’erreur « le champ PrixUnitaire est requis », vous pouvez afficher le message d’erreur « le champ Price est requis ».

> [!NOTE] 
> 
> Si vous souhaitez personnaliser complètement le message d’erreur affiché par un validateur, vous pouvez assigner un message d’erreur personnalisé à la propriété ErrorMessage du validateur comme suit : `<Required(ErrorMessage:="This field needs a value!")>`

Vous pouvez utiliser la classe Product de la **Liste 1** avec l’action créer () du contrôleur dans la **liste 2**. Cette action de contrôleur réaffiche la vue Create lorsque l’état du modèle contient des erreurs.

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample3.vb)]

**Liste 2**: Controllers\ProductController.vb

Enfin, vous pouvez créer la vue dans la **liste 3** en cliquant avec le bouton droit sur l’action créer () et en sélectionnant l’option de menu **Ajouter une vue**. Créez une vue fortement typée avec la classe Product comme classe de modèle. Sélectionnez **créer** dans la liste déroulante Afficher le contenu (voir **figure 2**).

[![](validation-with-the-data-annotation-validators-vb/_static/image5.png)](validation-with-the-data-annotation-validators-vb/_static/image4.png)

**Figure 2**: ajout de la vue Create

[!code-aspx[Main](validation-with-the-data-annotation-validators-vb/samples/sample4.aspx)]

**Liste 3**: Views\Product\Create.aspx

> [!NOTE] 
> 
> Supprimez le champ ID de l’option créer un formulaire généré par l’option de menu **Ajouter une vue** . Étant donné que le champ ID correspond à une colonne d’identité, vous ne souhaitez pas autoriser les utilisateurs à entrer une valeur pour ce champ.

Si vous envoyez le formulaire de création d’un produit et que vous n’entrez pas de valeurs pour les champs obligatoires, les messages d’erreur de validation de la **figure 3** s’affichent.

[![](validation-with-the-data-annotation-validators-vb/_static/image7.png)](validation-with-the-data-annotation-validators-vb/_static/image6.png)

**Figure 3**: champs obligatoires manquants

Si vous entrez un montant de devise non valide, le message d’erreur de la **figure 4** s’affiche.

[![](validation-with-the-data-annotation-validators-vb/_static/image9.png)](validation-with-the-data-annotation-validators-vb/_static/image8.png)

**Figure 4**: montant de devise non valide

## <a name="using-data-annotation-validators-with-the-entity-framework"></a>Utilisation de validateurs d’annotation de données avec le Entity Framework

Si vous utilisez Microsoft Entity Framework pour générer vos classes de modèle de données, vous ne pouvez pas appliquer les attributs du validateur directement à vos classes. Étant donné que le Entity Framework Designer génère les classes de modèle, toutes les modifications que vous apportez aux classes de modèle seront remplacées la prochaine fois que vous apportez des modifications dans le concepteur.

Si vous souhaitez utiliser les validateurs avec les classes générées par le Entity Framework vous devez créer des classes de métadonnées. Vous appliquez les validateurs à la classe de métadonnées au lieu d’appliquer les validateurs à la classe réelle.

Par exemple, imaginez que vous avez créé une classe Movie à l’aide de l’Entity Framework (voir **figure 5**). Imaginez, en outre, que vous voulez que les propriétés titre du film et directeur soient les propriétés requises. Dans ce cas, vous pouvez créer la classe partielle et la classe de métadonnées dans la **liste 4**.

[![](validation-with-the-data-annotation-validators-vb/_static/image11.png)](validation-with-the-data-annotation-validators-vb/_static/image10.png)

**Figure 5**: classe Movie générée par Entity Framework

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample5.vb)]

**Liste 4**: Models\Movie.vb

Le fichier figurant dans la **liste 4** contient deux classes nommées Movie et MovieMetaData. La classe Movie est une classe partielle. Elle correspond à la classe partielle générée par la Entity Framework contenue dans le fichier DataModel. Designer. vb.

Actuellement, le .NET Framework ne prend pas en charge les propriétés partielles. Par conséquent, il n’existe aucun moyen d’appliquer les attributs du validateur aux propriétés de la classe Movie définie dans le fichier DataModel. Designer. vb en appliquant les attributs du validateur aux propriétés de la classe Movie définie dans le fichier de la **liste 4**.

Notez que la classe partielle Movie est décorée avec un attribut de type de données qui pointe vers la classe MovieMetaData. La classe MovieMetaData contient des propriétés de proxy pour les propriétés de la classe Movie.

Les attributs du validateur sont appliqués aux propriétés de la classe MovieMetaData. Les propriétés titre, directeur et DateReleased sont toutes marquées comme étant des propriétés requises. Une chaîne contenant moins de 5 caractères doit être affectée à la propriété Director. Enfin, l’attribut DisplayName est appliqué à la propriété DateReleased pour afficher un message d’erreur tel que « le champ date de publication est obligatoire. » au lieu de l’erreur « le champ DateReleased est obligatoire. »

> [!NOTE] 
> 
> Notez que les propriétés de proxy dans la classe MovieMetaData n’ont pas besoin de représenter les mêmes types que les propriétés correspondantes dans la classe Movie. Par exemple, la propriété Director est une propriété de type chaîne dans la classe Movie et une propriété Object dans la classe MovieMetaData.

La page de la **figure 6** illustre les messages d’erreur retournés lorsque vous entrez des valeurs non valides pour les propriétés du film.

[![](validation-with-the-data-annotation-validators-vb/_static/image13.png)](validation-with-the-data-annotation-validators-vb/_static/image12.png)

**Figure 6**: utilisation de validateurs avec l’Entity Framework ([cliquez pour afficher l’image en taille réelle](validation-with-the-data-annotation-validators-vb/_static/image14.png))

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous avez appris à tirer parti du Binder de modèle d’annotation de données pour effectuer la validation dans une application MVC ASP.NET. Vous avez appris à utiliser les différents types d’attributs de validateur, tels que les attributs requis et StringLength. Vous avez également appris à utiliser ces attributs lorsque vous travaillez avec le Entity Framework Microsoft.

> [!div class="step-by-step"]
> [Précédent](validating-with-a-service-layer-vb.md)
