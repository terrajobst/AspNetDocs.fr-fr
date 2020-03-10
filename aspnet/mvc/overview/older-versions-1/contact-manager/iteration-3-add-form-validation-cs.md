---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-cs
title: '#3 d’itération – ajouter la validationC#de formulaire () | Microsoft Docs'
author: microsoft
description: Dans la troisième itération, nous ajoutons la validation de base du formulaire. Nous empêchons les utilisateurs de soumettre un formulaire sans remplir les champs de formulaire requis. Nous validons également emai...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 51a0d175-913b-43d8-95e3-840fb96ad1a9
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: af2e86e820f60f0a3d8e3db8f78eba67ef63579a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544470"
---
# <a name="iteration-3--add-form-validation-c"></a>#3 d’itération – ajouter une validationC#de formulaire ()

par [Microsoft](https://github.com/microsoft)

[Télécharger le code](iteration-3-add-form-validation-cs/_static/contactmanager_3_cs1.zip)

> Dans la troisième itération, nous ajoutons la validation de base du formulaire. Nous empêchons les utilisateurs de soumettre un formulaire sans remplir les champs de formulaire requis. Nous validons également les adresses de messagerie et les numéros de téléphone.

## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Création d’une application MVC ASP.NET de gestionC#des contacts ()

Dans cette série de didacticiels, nous créons une application de gestion de contacts entière du début à la fin. L’application Gestionnaire de contacts vous permet de stocker les informations de contact, les noms de téléphone et les adresses de messagerie, pour obtenir la liste des personnes.

Nous générons l’application sur plusieurs itérations. À chaque itération, nous améliorons progressivement l’application. L’objectif de cette approche à plusieurs itérations est de vous permettre de comprendre la raison de chaque modification.

- #1 d’itération : créez l’application. Dans la première itération, nous créons le gestionnaire de contacts de la manière la plus simple possible. Nous ajoutons la prise en charge des opérations de base de données de base : créer, lire, mettre à jour et supprimer (CRUD).

- Itération #2 : rendez l’application agréable. Dans cette itération, nous améliorons l’apparence de l’application en modifiant la page maître par défaut de la vue MVC ASP.NET et la feuille de style en cascade.

- Itération #3 : ajouter une validation de formulaire. Dans la troisième itération, nous ajoutons la validation de base du formulaire. Nous empêchons les utilisateurs de soumettre un formulaire sans remplir les champs de formulaire requis. Nous validons également les adresses de messagerie et les numéros de téléphone.

- Itération #4 : rendez l’application faiblement couplée. Dans cette quatrième itération, nous tirant parti de plusieurs modèles de conception de logiciels pour faciliter la gestion et la modification de l’application de gestionnaire de contacts. Par exemple, nous refactorisons notre application pour utiliser le modèle de référentiel et le modèle d’injection de dépendances.

- #5 d’itération-créer des tests unitaires. Dans la cinquième itération, nous rendons notre application plus facile à gérer et à modifier en ajoutant des tests unitaires. Nous imitons nos classes de modèle de données et créons des tests unitaires pour nos contrôleurs et la logique de validation.

- Itération #6-Utilisez le développement piloté par les tests. Dans cette sixième itération, nous ajoutons de nouvelles fonctionnalités à notre application en écrivant d’abord des tests unitaires et en écrivant du code sur les tests unitaires. Dans cette itération, nous ajoutons des groupes de contacts.

- #7 d’itération-ajoutez des fonctionnalités AJAX. Dans la septième itération, nous améliorons la réactivité et les performances de notre application en ajoutant la prise en charge d’Ajax.

## <a name="this-iteration"></a>Cette itération

Dans cette deuxième itération de l’application du gestionnaire de contacts, nous ajoutons la validation de base du formulaire. Nous empêchons les utilisateurs d’envoyer un contact sans entrer de valeurs pour les champs de formulaire requis. Nous validons également les numéros de téléphone et les adresses de messagerie (voir figure 1).

[![la boîte de dialogue Nouveau projet](iteration-3-add-form-validation-cs/_static/image1.jpg)](iteration-3-add-form-validation-cs/_static/image1.png)

**Figure 01**: formulaire avec validation ([cliquez pour afficher l’image en taille réelle](iteration-3-add-form-validation-cs/_static/image2.png))

Dans cette itération, nous ajoutons la logique de validation directement aux actions du contrôleur. En général, il ne s’agit pas de la méthode recommandée pour ajouter la validation à une application MVC ASP.NET. Une meilleure approche consiste à placer une logique de validation d’application dans une [couche de service](http://martinfowler.com/eaaCatalog/serviceLayer.html)distincte. Dans l’itération suivante, nous refactorisons l’application du gestionnaire de contacts pour rendre l’application plus facile à gérer.

Dans cette itération, pour simplifier les choses, nous écrivons tout le code de validation à la main. Au lieu d’écrire le code de validation, nous pouvons tirer parti d’un Framework de validation. Par exemple, vous pouvez utiliser le bloc d’application de validation Microsoft Enterprise Library (Library) pour implémenter la logique de validation de votre application ASP.NET MVC. Pour en savoir plus sur le bloc d’application de validation, consultez :

[*http://msdn.microsoft.com/library/dd203099.aspx*](https://msdn.microsoft.com/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a>Ajout d’une validation à la vue Create

Commençons par ajouter une logique de validation à la vue Create. Heureusement, étant donné que nous avons généré la vue Create avec Visual Studio, la vue Create contient déjà la logique d’interface utilisateur nécessaire pour afficher les messages de validation. La vue Create est contenue dans la liste 1.

**Liste 1-\Views\Contact\Create.aspx**

[!code-aspx[Main](iteration-3-add-form-validation-cs/samples/sample1.aspx)]

Notez l’appel à la méthode d’assistance HTML. ValidationSummary () qui s’affiche immédiatement au-dessus du formulaire HTML. En présence de messages d’erreur de validation, cette méthode affiche les messages de validation dans une liste à puces.

Notez, en outre, les appels à html. ValidationMessage () qui s’affichent en regard de chaque champ de formulaire. L’assistance ValidationMessage () affiche un message d’erreur de validation individuel. Dans le cas de listing 1, un astérisque s’affiche en cas d’erreur de validation.

Enfin, le programme d’assistance HTML. TextBox () restitue automatiquement une classe de feuille de style en cascade quand une erreur de validation est associée à la propriété affichée par le programme d’assistance. L’assistance HTML. TextBox () restitue une classe nommée **entrée-validation-erreur**.

Lorsque vous créez une application MVC ASP.NET, une feuille de style nommée site. CSS est automatiquement créée dans le dossier Content. Cette feuille de style contient les définitions suivantes pour les classes CSS associées à l’apparence des messages d’erreur de validation :

[!code-css[Main](iteration-3-add-form-validation-cs/samples/sample2.css)]

La classe Field-validation-Error est utilisée pour appliquer un style à la sortie rendue par l’assistance HTML. ValidationMessage (). La classe d’erreur de validation d’entrée est utilisée pour appliquer un style à la zone de texte (entrée) rendue par l’assistance HTML. TextBox (). La classe validation-Summary-Errors est utilisée pour appliquer un style à la liste non triée rendue par l’assistance HTML. ValidationSummary ().

> [!NOTE] 
> 
> Vous pouvez modifier les classes de feuille de style décrites dans cette section pour personnaliser l’apparence des messages d’erreur de validation.

## <a name="adding-validation-logic-to-the-create-action"></a>Ajout d’une logique de validation à l’action de création

Pour le moment, la vue Create View n’affiche jamais de messages d’erreur de validation, car nous n’avons pas écrit la logique pour générer des messages. Pour afficher les messages d’erreur de validation, vous devez ajouter les messages d’erreur à ModelState.

> [!NOTE] 
> 
> La méthode UpdateModel () ajoute automatiquement des messages d’erreur à ModelState en cas d’erreur lors de l’assignation de la valeur d’un champ de formulaire à une propriété. Par exemple, si vous tentez d’assigner la chaîne « Apple » à une propriété BirthDate qui accepte les valeurs DateTime, la méthode UpdateModel () ajoute une erreur à ModelState.

La méthode Create () modifiée dans Listing 2 contient une nouvelle section qui valide les propriétés de la classe contact avant que le nouveau contact soit inséré dans la base de données.

**Liste 2-Controllers\ContactController.cs (création avec validation)**

[!code-csharp[Main](iteration-3-add-form-validation-cs/samples/sample3.cs)]

La section Validate applique quatre règles de validation distinctes :

- La propriété FirstName doit avoir une longueur supérieure à zéro (et ne peut pas se composer uniquement d’espaces)
- La propriété LastName doit avoir une longueur supérieure à zéro (et ne peut pas se composer uniquement d’espaces)
- Si la propriété Phone a une valeur (a une longueur supérieure à 0), la propriété Phone doit correspondre à une expression régulière.
- Si la propriété E-mail a une valeur (a une longueur supérieure à 0), la propriété email doit correspondre à une expression régulière.

En cas de violation de règle de validation, un message d’erreur est ajouté à ModelState à l’aide de la méthode AddModelError (). Lorsque vous ajoutez un message à ModelState, vous fournissez le nom d’une propriété et le texte d’un message d’erreur de validation. Ce message d’erreur s’affiche dans la vue par les méthodes d’assistance HTML. ValidationSummary () et html. ValidationMessage ().

Une fois les règles de validation exécutées, la propriété IsValid de ModelState est vérifiée. La propriété IsValid retourne la valeur false lorsqu’un message d’erreur de validation a été ajouté à ModelState. Si la validation échoue, le formulaire de création est réaffiché avec les messages d’erreur.

> [!NOTE] 
> 
> J’ai obtenu les expressions régulières pour valider le numéro de téléphone et l’adresse de messagerie à partir du référentiel d’expressions régulières à [ *http://regexlib.com* ](http://regexlib.com)

## <a name="adding-validation-logic-to-the-edit-action"></a>Ajout d’une logique de validation à l’action Edit

L’action Edit () met à jour un contact. L’action Edit () doit effectuer exactement la même validation que l’action Create (). Au lieu de dupliquer le même code de validation, nous devons Refactoriser le contrôleur de contact afin que les actions Create () et Edit () appellent la même méthode de validation.

La classe de contrôleur de contact modifiée est contenue dans la liste 3. Cette classe a une nouvelle méthode ValidateContact () qui est appelée dans les actions Create () et Edit ().

**Liste 3-Controllers\ContactController.cs**

[!code-csharp[Main](iteration-3-add-form-validation-cs/samples/sample4.cs)]

## <a name="summary"></a>Récapitulatif

Dans cette itération, nous avons ajouté la validation de base de formulaire à notre application de gestion des contacts. Notre logique de validation empêche les utilisateurs d’envoyer un nouveau contact ou de modifier un contact existant sans fournir de valeurs pour les propriétés FirstName et LastName. En outre, les utilisateurs doivent fournir des numéros de téléphone et des adresses de messagerie valides.

Dans cette itération, nous avons ajouté la logique de validation à notre application de gestionnaire de contacts de la manière la plus simple possible. Toutefois, la combinaison de notre logique de validation dans notre logique de contrôleur va créer des problèmes pour nous à long terme. Notre application sera plus difficile à gérer et à modifier au fil du temps.

Dans l’itération suivante, nous refactorisons notre logique de validation et la logique d’accès à la base de données à partir de nos contrôleurs. Nous allons tirer parti de plusieurs principes de conception de logiciels pour nous permettre de créer une application plus faiblement couplée et plus facile à gérer.

> [!div class="step-by-step"]
> [Précédent](iteration-2-make-the-application-look-nice-cs.md)
> [Suivant](iteration-4-make-the-application-loosely-coupled-cs.md)
