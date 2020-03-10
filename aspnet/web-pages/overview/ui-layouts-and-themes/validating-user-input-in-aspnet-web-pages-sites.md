---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: Validation de l’entrée d’utilisateur dans les sites pages Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Cet article explique comment valider les informations que vous recevez auprès des utilisateurs &mdash; autrement dit, pour s’assurer que les utilisateurs entrent des informations valides dans les formulaires HTML d’un AS...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: e6f8e1051d09d11f1756bfada44a73ba7c2a1db2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78563503"
---
# <a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a>Validation des entrées d’utilisateur dans les sites pages Web ASP.NET (Razor)

par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article explique comment valider les informations que vous recevez auprès des utilisateurs &mdash; autrement dit, pour s’assurer que les utilisateurs entrent des informations valides dans les formulaires HTML d’un site pages Web ASP.NET (Razor).
> 
> Ce que vous allez apprendre :
> 
> - Comment vérifier que l’entrée d’un utilisateur correspond aux critères de validation que vous définissez.
> - Comment déterminer si tous les tests de validation ont réussi.
> - Comment afficher les erreurs de validation (et comment les mettre en forme).
> - Comment valider des données qui ne proviennent pas directement des utilisateurs.
> 
> Voici les concepts de programmation ASP.NET présentés dans l’article :
> 
> - Le programme d’assistance `Validation`.
> - Méthodes `Html.ValidationSummary` et `Html.ValidationMessage`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions logicielles utilisées dans le didacticiel
> 
> 
> - Pages Web ASP.NET (Razor) 3
>   
> 
> Ce didacticiel fonctionne également avec pages Web ASP.NET 2.

Cet article contient les sections suivantes :

- [Vue d’ensemble de la validation des entrées d’utilisateur](#Overview_of_User_Input_Validation)
- [Validation des entrées utilisateur](#Validating_User_Input)
- [Ajout de la validation côté client](#Adding_Client-Side_Validation)
- [Mise en forme des erreurs de validation](#Formatting_Validation_Errors)
- [Validation des données qui ne proviennent pas directement des utilisateurs](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a>Vue d’ensemble de la validation des entrées d’utilisateur

Si vous demandez aux utilisateurs d’entrer des informations dans une page (par exemple, dans un formulaire), il est important de s’assurer que les valeurs entrées sont valides. Par exemple, vous ne souhaitez pas traiter un formulaire auquel il manque des informations critiques.

Lorsque les utilisateurs entrent des valeurs dans un formulaire HTML, les valeurs qu’ils entrent sont des chaînes. Dans de nombreux cas, les valeurs dont vous avez besoin sont d’autres types de données, tels que des entiers ou des dates. Par conséquent, vous devez également vous assurer que les valeurs entrées par les utilisateurs peuvent être correctement converties en types de données appropriés.

Vous pouvez également avoir certaines restrictions sur les valeurs. Même si les utilisateurs entrent correctement un entier, par exemple, vous devrez peut-être vous assurer que la valeur est comprise dans une certaine plage.

![Erreurs de validation qui utilisent des classes de style CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> **Important** La validation de l’entrée utilisateur est également importante pour la sécurité. Lorsque vous limitez les valeurs que les utilisateurs peuvent entrer dans les formulaires, vous réduisez le risque que quelqu’un puisse entrer dans une valeur susceptible de compromettre la sécurité de votre site.

<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a>Validation de l'entrée utilisateur

Dans pages Web ASP.NET 2, vous pouvez utiliser le programme d’assistance `Validator` pour tester l’entrée d’utilisateur. L’approche de base consiste à effectuer les opérations suivantes :

1. Déterminez les éléments d’entrée (champs) que vous souhaitez valider.

    En général, vous validez les valeurs dans `<input>` éléments d’un formulaire. Toutefois, il est recommandé de valider toutes les entrées, même celles qui proviennent d’un élément restreint comme une liste de `<select>`. Cela permet de s’assurer que les utilisateurs ne contournent pas les contrôles sur une page et envoient un formulaire.
2. Dans le code de la page, ajoutez des contrôles de validation individuels pour chaque élément d’entrée à l’aide des méthodes du programme d’assistance `Validation`.

    Pour rechercher les champs obligatoires, utilisez `Validation.RequireField(field, [error message])` (pour un champ individuel) ou `Validation.RequireFields(field1, field2, ...))` (pour une liste de champs). Pour les autres types de validation, utilisez `Validation.Add(field, ValidationType)`. Pour `ValidationType`, vous pouvez utiliser les options suivantes :

    `Validator.DateTime ([error message])`  
   `Validator.Decimal([error message])`  
   `Validator.EqualsTo(otherField [, error message])`  
   `Validator.Float([error message])`  
   `Validator.Integer([error message])`  
   `Validator.Range(min, max [, error message])`  
   `Validator.RegEx(pattern [, error message])`  
   `Validator.Required([error message])`  
   `Validator.StringLength(length)`  
   `Validator.Url([error message])`
3. Lorsque la page est envoyée, vérifiez si la validation a réussi en vérifiant `Validation.IsValid`:

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    Si des erreurs de validation se produisent, vous ignorez le traitement de page normal. Par exemple, si l’objectif de la page est de mettre à jour une base de données, vous ne le faites pas tant que toutes les erreurs de validation n’ont pas été corrigées.
4. En cas d’erreurs de validation, affichez les messages d’erreur dans le balisage de la page à l’aide de `Html.ValidationSummary` ou `Html.ValidationMessage`, ou les deux.

L’exemple suivant montre une page qui illustre ces étapes.

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

Pour voir comment fonctionne la validation, exécutez cette page et faites délibérément des erreurs. Par exemple, voici à quoi ressemble la page si vous oubliez d’entrer un nom de cours, si vous entrez un, et si vous entrez une date non valide :

![Erreurs de validation dans la page rendue](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a>Ajout de la validation côté client

Par défaut, l’entrée utilisateur est validée une fois que les utilisateurs envoient la page, c’est-à-dire que la validation est effectuée dans le code serveur. L’inconvénient de cette approche est que les utilisateurs ne savent pas qu’ils ont effectué une erreur jusqu’à ce qu’ils envoient la page. Si un formulaire est long ou complexe, le fait de signaler des erreurs uniquement après l’envoi de la page peut être gênant pour l’utilisateur.

Vous pouvez ajouter la prise en charge pour effectuer la validation dans le script client. Dans ce cas, la validation est effectuée au fur et à mesure que les utilisateurs travaillent dans le navigateur. Supposons, par exemple, que vous spécifiez qu’une valeur doit être un entier. Si un utilisateur entre une valeur non entière, l’erreur est signalée dès que l’utilisateur quitte le champ d’entrée. Les utilisateurs reçoivent des commentaires immédiats, ce qui est pratique pour eux. La validation basée sur le client peut également réduire le nombre de fois où l’utilisateur doit envoyer le formulaire pour corriger plusieurs erreurs.

> [!NOTE]
> Même si vous utilisez la validation côté client, la validation est toujours également effectuée dans le code serveur. L’exécution de la validation dans le code serveur est une mesure de sécurité, au cas où les utilisateurs contournent la validation basée sur le client.

1. Inscrivez les bibliothèques JavaScript suivantes dans la page :  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   Deux des bibliothèques sont chargées à partir d’un réseau de distribution de contenu (CDN). vous n’avez donc pas besoin de les disposer sur votre ordinateur ou serveur. Toutefois, vous devez disposer d’une copie locale de *jQuery. Validate. undiscrets. js*. Si vous n’utilisez pas déjà un modèle WebMatrix (comme **Starter Site** ) qui comprend la bibliothèque, créez un site Web pages basé sur le site de **démarrage**. Copiez ensuite le fichier *. js* sur votre site actuel.
2. Dans le balisage, pour chaque élément que vous validez, ajoutez un appel à `Validation.For(field)`. Cette méthode émet des attributs qui sont utilisés par la validation côté client. (Plutôt que d’émettre du code JavaScript réel, la méthode émet des attributs comme `data-val-...`. Ces attributs prennent en charge la validation client discrète qui utilise jQuery pour effectuer le travail.)

La page suivante montre comment ajouter des fonctionnalités de validation client à l’exemple indiqué plus haut.

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

Les vérifications de validation ne sont pas toutes exécutées sur le client. En particulier, la validation du type de données (entier, date, etc.) ne s’exécute pas sur le client. Les vérifications suivantes fonctionnent sur le client et sur le serveur :

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

Dans cet exemple, le test d’une date valide ne fonctionnera pas dans le code client. Toutefois, le test sera effectué dans le code serveur.

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a>Mise en forme des erreurs de validation

Vous pouvez contrôler la façon dont les erreurs de validation s’affichent en définissant des classes CSS qui possèdent les noms réservés suivants :

- `field-validation-error`. Définit la sortie de la méthode `Html.ValidationMessage` lorsqu’elle affiche une erreur.
- `field-validation-valid`. Définit la sortie de la méthode `Html.ValidationMessage` lorsqu’il n’y a aucune erreur.
- `input-validation-error`. Définit le mode de rendu des éléments `<input>` en cas d’erreur. (Par exemple, vous pouvez utiliser cette classe pour définir la couleur d’arrière-plan d’un &lt;d’entrée&gt; élément sur une couleur différente si sa valeur n’est pas valide.) Cette classe CSS est utilisée uniquement lors de la validation du client (dans pages Web ASP.NET 2).
- `input-validation-valid`. Définit l’apparence des éléments de `<input>` lorsqu’il n’y a aucune erreur.
- `validation-summary-errors`. Définit la sortie de la méthode `Html.ValidationSummary` elle affiche une liste d’erreurs.
- `validation-summary-valid`. Définit la sortie de la méthode `Html.ValidationSummary` lorsqu’il n’y a aucune erreur.

Le bloc de `<style>` suivant affiche les règles des conditions d’erreur.

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

Si vous incluez ce bloc de style dans les exemples de pages de plus haut dans l’article, l’affichage de l’erreur ressemble à l’illustration suivante :

![Erreurs de validation qui utilisent des classes de style CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> Si vous n’utilisez pas la validation client dans pages Web ASP.NET 2, les classes CSS pour les éléments `<input>` (`input-validation-error` et `input-validation-valid` n’ont aucun effet.

### <a name="static-and-dynamic-error-display"></a>Affichage des erreurs statiques et dynamiques

Les règles CSS sont en paire, par exemple `validation-summary-errors` et `validation-summary-valid`. Ces paires vous permettent de définir des règles pour les deux conditions : une condition d’erreur et une condition « normale » (non-erreur). Il est important de comprendre que le balisage de l’affichage des erreurs est toujours rendu, même s’il n’y a aucune erreur. Par exemple, si une page a une méthode `Html.ValidationSummary` dans le balisage, la source de la page contient le balisage suivant, même lorsque la page est demandée pour la première fois :

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

En d’autres termes, la méthode `Html.ValidationSummary` restitue toujours un élément `<div>` et une liste, même si la liste d’erreurs est vide. De même, la méthode `Html.ValidationMessage` restitue toujours un élément `<span>` en tant qu’espace réservé pour une erreur de champ individuel, même s’il n’y a aucune erreur.

Dans certains cas, l’affichage d’un message d’erreur peut entraîner le redistribution de la page et peut entraîner le déplacement des éléments sur la page. Les règles CSS qui se terminent par `-valid` vous permettent de définir une mise en page qui peut aider à éviter ce problème. Par exemple, vous pouvez définir `field-validation-error` et `field-validation-valid` ont la même taille fixe. De cette façon, la zone d’affichage du champ est statique et ne modifie pas le déroulement de la page si un message d’erreur est affiché.

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a>Validation des données qui ne proviennent pas directement des utilisateurs

Vous devez parfois valider des informations qui ne proviennent pas directement d’un formulaire HTML. Un exemple classique est une page dans laquelle une valeur est passée dans une chaîne de requête, comme dans l’exemple suivant :

`http://server/myapp/EditClassInformation?classid=1022`

Dans ce cas, vous souhaitez vous assurer que la valeur qui est transmise à la page (ici, 1022 pour la valeur de `classid`) est valide. Vous ne pouvez pas utiliser directement le programme d’assistance `Validation` pour effectuer cette validation. Toutefois, vous pouvez utiliser d’autres fonctionnalités du système de validation, telles que la possibilité d’afficher des messages d’erreur de validation.

> [!NOTE] 
> 
> **Important** Validez toujours les valeurs que vous recevez d’une source, y compris *les* valeurs de champ de formulaire, les valeurs de chaîne de requête et les valeurs de cookie. Il est facile pour les utilisateurs de modifier ces valeurs (peut-être à des fins malveillantes). Vous devez donc vérifier ces valeurs pour protéger votre application.

L’exemple suivant montre comment vous pouvez valider une valeur qui est passée dans une chaîne de requête. Le code teste que la valeur n’est pas vide et qu’il s’agit d’un entier.

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

Notez que le test est effectué lorsque la demande n’est pas une soumission de formulaire (`if(!IsPost)`). Ce test réussit la première fois que la page est demandée, mais pas lorsque la demande est une soumission de formulaire.

Pour afficher cette erreur, vous pouvez ajouter l’erreur à la liste des erreurs de validation en appelant `Validation.AddFormError("message")`. Si la page contient un appel à la méthode `Html.ValidationSummary`, l’erreur s’affiche, comme une erreur de validation d’entrée utilisateur.

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

[Utilisation des formulaires HTML dans les sites pages Web ASP.NET](https://go.microsoft.com/fwlink/?LinkID=202892)
