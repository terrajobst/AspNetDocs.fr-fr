---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
title: Exécution d’une validation simple (VB) | Microsoft Docs
author: StephenWalther
description: Découvrez comment effectuer la validation dans une application ASP.NET MVC. Dans ce didacticiel, Stephen Walther vous présente l’état du modèle et la validation du programme d’assistance HTML...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: df6cf4b7-0bb3-4c4e-b17a-bd78a759a6bc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: 46925f22b7dfc23f2bb89b8d2fff0cbd8ae49062
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78542930"
---
# <a name="performing-simple-validation-vb"></a>Réalisation d’une validation simple (VB)

par [Stephen Walther](https://github.com/StephenWalther)

> Découvrez comment effectuer la validation dans une application ASP.NET MVC. Dans ce didacticiel, Stephen Walther vous présente l’état du modèle et les applications auxiliaires HTML de validation.

L’objectif de ce didacticiel est d’expliquer comment vous pouvez effectuer la validation dans une application ASP.NET MVC. Par exemple, vous apprenez comment empêcher une personne de soumettre un formulaire qui ne contient pas de valeur pour un champ obligatoire. Vous allez apprendre à utiliser l’état du modèle et les applications auxiliaires HTML de validation.

## <a name="understanding-model-state"></a>Fonctionnement de l’état du modèle

Vous utilisez l’état du modèle (ou plus précisément, le dictionnaire d’États de modèles) pour représenter les erreurs de validation. Par exemple, l’action Create () dans la liste 1 valide les propriétés d’une classe Product avant d’ajouter la classe Product à une base de données.

Je ne recommande pas d’ajouter votre logique de validation ou de base de données à un contrôleur. Un contrôleur doit contenir uniquement une logique liée au contrôle de workflow de l’application. Nous prenons un raccourci pour simplifier les choses.

**Liste 1-Controllers\ProductController.vb**

[!code-vb[Main](performing-simple-validation-vb/samples/sample1.vb)]

Dans la liste 1, les propriétés Name, description et UnitsInStock de la classe Product sont validées. Si l’une de ces propriétés échoue à un test de validation, une erreur est ajoutée au dictionnaire d’États de modèles (représenté par la propriété ModelState de la classe Controller).

En cas d’erreur dans l’état du modèle, la propriété ModelState. IsValid retourne la valeur false. Dans ce cas, le formulaire HTML pour la création d’un nouveau produit est réaffiché. Sinon, s’il n’y a aucune erreur de validation, le nouveau produit est ajouté à la base de données.

## <a name="using-the-validation-helpers"></a>Utilisation des applications d’assistance de validation

L’infrastructure MVC ASP.NET comprend deux applications auxiliaires de validation : le programme d’assistance HTML. ValidationMessage () et le programme d’assistance HTML. ValidationSummary (). Vous utilisez ces deux applications auxiliaires dans une vue pour afficher les messages d’erreur de validation.

Les applications auxiliaires html. ValidationMessage () et html. ValidationSummary () sont utilisées dans les vues Create et Edit qui sont générées automatiquement par la génération de modèles automatique ASP.NET MVC. Pour générer la vue Create, procédez comme suit :

1. Cliquez avec le bouton droit sur l’action créer () dans le contrôleur de produit, puis sélectionnez l’option de menu **Ajouter une vue** (voir la figure 1).
2. Dans la boîte de dialogue **Ajouter une vue** , activez la case à cocher **créer une vue fortement typée** (voir figure 2).
3. Dans la liste déroulante **afficher la classe de données** , sélectionnez la classe Product.
4. Dans la liste déroulante **afficher le contenu** , sélectionnez créer.
5. Cliquez sur le bouton **Ajouter**.

Veillez à créer votre application avant d’ajouter une vue. Dans le cas contraire, la liste des classes n’apparaîtra pas dans la liste déroulante **afficher la classe de données** .

[![la boîte de dialogue Nouveau projet](performing-simple-validation-vb/_static/image1.jpg)](performing-simple-validation-vb/_static/image1.png)

**Figure 01**: ajout d’une vue ([cliquez pour afficher l’image en taille réelle](performing-simple-validation-vb/_static/image2.png))

[![la boîte de dialogue Nouveau projet](performing-simple-validation-vb/_static/image2.jpg)](performing-simple-validation-vb/_static/image3.png)

**Figure 02**: création d’une vue fortement typée ([cliquez pour afficher l’image en taille réelle](performing-simple-validation-vb/_static/image4.png))

Une fois ces étapes terminées, vous pouvez obtenir la vue Create (créer) dans Listing 2.

**Liste 2-Views\Product\Create.aspx**

[!code-aspx[Main](performing-simple-validation-vb/samples/sample2.aspx)]

Dans la liste 2, l’assistance HTML. ValidationSummary () est appelée juste au-dessus du formulaire HTML. Cette application d’assistance est utilisée pour afficher une liste des messages d’erreur de validation. L’assistance HTML. ValidationSummary () restitue les erreurs dans une liste à puces.

L’assistance HTML. ValidationMessage () est appelée en regard de chacun des champs de formulaire HTML. Cette application d’assistance est utilisée pour afficher un message d’erreur juste en regard d’un champ de formulaire. Dans le cas de Listing 2, le programme d’assistance HTML. ValidationMessage () affiche un astérisque lorsqu’il y a une erreur.

La page de la figure 3 illustre les messages d’erreur rendus par les applications d’assistance de validation lorsque le formulaire est envoyé avec des champs manquants et des valeurs non valides.

[![la boîte de dialogue Nouveau projet](performing-simple-validation-vb/_static/image3.jpg)](performing-simple-validation-vb/_static/image5.png)

**Figure 03**: création d’une vue soumise avec des problèmes ([cliquez pour afficher l’image en plein écran](performing-simple-validation-vb/_static/image6.png))

Notez que l’apparence des champs d’entrée HTML est également modifiée en cas d’erreur de validation. L’assistance HTML. TextBox () restitue un attribut *Class = "Input-validation-Error"* quand une erreur de validation est associée à la propriété rendue par l’assistance HTML. TextBox ().

Trois classes de feuilles de style en cascade sont utilisées pour contrôler l’apparence des erreurs de validation :

- entrée-validation-erreur-appliqué à la balise de&gt; d’entrée &lt;restituée par le programme d’assistance HTML. TextBox ().
- Field-validation-Error-appliqué à la balise &lt;span&gt; rendu par le programme d’assistance HTML. ValidationMessage ().
- validation-Summary-Errors-appliqué à la balise &lt;UL&gt; rendue par l’assistance HTML. ValidationSummary ().

Vous pouvez modifier ces classes de feuille de style en cascade et, par conséquent, modifier l’apparence des erreurs de validation, en modifiant le fichier site. CSS situé dans le dossier Content.

> [!NOTE] 
> 
> La classe HtmlHelper comprend des propriétés statiques en lecture seule pour récupérer les noms des classes CSS associées à la validation. Ces propriétés statiques sont nommées ValidationInputCssClassName, ValidationFieldCssClassName et ValidationSummaryCssClassName.

## <a name="prebinding-validation-and-postbinding-validation"></a>Validation de préliaison et validation de Postbinding

Si vous soumettez le formulaire HTML pour la création d’un produit et que vous entrez une valeur non valide pour le champ Price et aucune valeur pour le champ UnitsInStock, vous obtenez les messages de validation affichés dans la figure 4. D’où proviennent ces messages d’erreur de validation ?

[![la boîte de dialogue Nouveau projet](performing-simple-validation-vb/_static/image4.jpg)](performing-simple-validation-vb/_static/image7.png)

**Figure 04**: erreurs de validation de préliaison ([cliquez pour afficher l’image en taille réelle](performing-simple-validation-vb/_static/image8.png))

Il existe en fait deux types de messages d’erreur de validation : ceux qui sont générés avant que les champs de formulaire HTML soient liés à une classe et ceux générés une fois que les champs de formulaire sont liés à la classe. En d’autres termes, il existe des erreurs de validation de préliaison et des erreurs de validation postbinding.

L’action Create () exposée par le contrôleur de produit dans la liste 1 accepte une instance de la classe Product. La signature de la méthode Create ressemble à ceci :

[!code-vb[Main](performing-simple-validation-vb/samples/sample3.vb)]

Les valeurs des champs de formulaire HTML du formulaire créer sont liées à la classe productToCreate par un classeur de modèles. Le Binder de modèle par défaut ajoute automatiquement un message d’erreur à l’état du modèle lorsqu’il ne peut pas lier un champ de formulaire à une propriété de formulaire.

Le classeur de modèles par défaut ne peut pas lier la chaîne « Apple » à la propriété Price de la classe Product. Vous ne pouvez pas assigner une chaîne à une propriété décimale. Par conséquent, le classeur de modèles ajoute une erreur à l’état du modèle.

Le Binder de modèle par défaut ne peut pas non plus attribuer la valeur Nothing à une propriété qui n’accepte pas la valeur Nothing. En particulier, le Binder de modèle ne peut pas affecter la valeur Nothing à la propriété UnitsInStock. Une fois encore, le Binder de modèle abandonne et ajoute un message d’erreur à l’état du modèle.

Si vous souhaitez personnaliser l’apparence de ces messages d’erreur de préliaison, vous devez créer des chaînes de ressource pour ces messages.

## <a name="summary"></a>Récapitulatif

L’objectif de ce didacticiel était de décrire les mécanismes de validation de base de l’infrastructure MVC ASP.NET. Vous avez appris à utiliser l’état du modèle et les applications auxiliaires HTML de validation. Nous avons également abordé la distinction entre la préliaison et la validation de postbinding. Dans d’autres didacticiels, nous aborderons différentes stratégies pour déplacer votre code de validation hors de vos contrôleurs et dans vos classes de modèle.

> [!div class="step-by-step"]
> [Précédent](displaying-a-table-of-database-data-vb.md)
> [Suivant](validating-with-the-idataerrorinfo-interface-vb.md)
