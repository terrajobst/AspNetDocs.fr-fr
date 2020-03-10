---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
title: Présentation de la pages Web ASP.NET-saisie de données de base de données à l’aide de formulaires | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous montre comment créer un formulaire d’entrée, puis entrer les données que vous récupérez du formulaire dans une table de base de données lorsque vous utilisez pages Web ASP.NET (...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: d37c93fc-25fd-4e94-8671-0d437beef206
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
msc.type: authoredcontent
ms.openlocfilehash: b9354a7b97a7df9020a681f709e16a92650cfcf0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78624536"
---
# <a name="introducing-aspnet-web-pages---entering-database-data-by-using-forms"></a>Présentation de la pages Web ASP.NET-saisie de données de base de données à l’aide de formulaires

par [Tom FitzMacken](https://github.com/tfitzmac)

> Ce didacticiel vous montre comment créer un formulaire d’entrée, puis entrer les données que vous récupérez du formulaire dans une table de base de données lorsque vous utilisez pages Web ASP.NET (Razor). Il part du principe que vous avez terminé la série jusqu’aux [principes fondamentaux des formulaires HTML dans pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=251581).
> 
> Ce que vous allez apprendre :
> 
> - En savoir plus sur le traitement des formulaires d’entrée.
> - Comment ajouter (insérer) des données dans une base de données.
> - Comment s’assurer que les utilisateurs ont entré une valeur obligatoire dans un formulaire (comment valider une entrée d’utilisateur).
> - Comment afficher les erreurs de validation.
> - Comment accéder à une autre page à partir de la page active.
>   
> 
> Fonctionnalités/technologies présentées :
> 
> - Méthode `Database.Execute`
> - Instruction SQL `Insert Into`
> - Le programme d’assistance `Validation`.
> - Méthode `Response.Redirect`

## <a name="what-youll-build"></a>Contenu

Dans le didacticiel précédent qui vous a montré comment créer une base de données, vous avez entré des données de base de données en modifiant la base de données directement dans WebMatrix, en travaillant dans l’espace de travail **base de données** . Dans la plupart des applications, il ne s’agit pas d’un moyen pratique de placer des données dans la base de données. Dans ce didacticiel, vous allez créer une interface basée sur le Web qui vous permet de saisir des données et de les enregistrer dans la base de données.

Vous allez créer une page dans laquelle vous pouvez entrer de nouveaux films. La page contient un formulaire d’entrée contenant des champs (zones de texte) dans lesquels vous pouvez entrer un titre de film, un genre et une année. La page doit ressembler à cette page :

![Page « Ajouter un film » dans le navigateur](entering-data/_static/image1.png)

Les zones de texte sont des éléments HTML `<input>` qui se présentent comme suit :

`<input type="text" name="genre" value="" />`

## <a name="creating-the-basic-entry-form"></a>Création du formulaire de saisie de base

Créez une page nommée *AddMovie. cshtml*.

Remplacez ce qui se trouve dans le fichier par le balisage suivant. Remplacer tout ; vous ajouterez un bloc de code en haut dans un instant.

[!code-cshtml[Main](entering-data/samples/sample1.cshtml)]

Cet exemple illustre le code HTML typique de la création d’un formulaire. Elle utilise `<input>` éléments pour les zones de texte et pour le bouton Envoyer. Les légendes des zones de texte sont créées à l’aide d’éléments de `<label>` standard. Les éléments `<fieldset>` et `<legend>` placent une belle zone dans le formulaire.

Notez que dans cette page, l’élément `<form>` utilise `post` comme valeur pour l’attribut `method`. Dans le didacticiel précédent, vous avez créé un formulaire qui utilisait la méthode `get`. Cela était correct, car même si le formulaire a envoyé des valeurs au serveur, la demande n’a pas apporté de modifications. Tout cela a été de récupérer des données de différentes façons. Toutefois, dans cette page, vous *allez* apporter des modifications, vous allez ajouter de nouveaux enregistrements de base de données. Par conséquent, ce formulaire doit utiliser la méthode `post`. (Pour plus d’informations sur la différence entre les `GET` et les opérations de `POST`, consultez la barre latérale[sécurité des verbes obtenir, poster et http](https://go.microsoft.com/fwlink/?LinkId=251581#GET,_POST,_and_HTTP_Verb_Safety) dans le didacticiel précédent.)

Notez que chaque zone de texte possède un élément `name` (`title`, `genre`, `year`). Comme vous l’avez vu dans le didacticiel précédent, ces noms sont importants, car vous devez disposer de ces noms pour pouvoir obtenir l’entrée de l’utilisateur par la suite. Vous pouvez utiliser n’importe quel nom. Il est utile d’utiliser des noms explicites qui vous permettent de vous souvenir des données que vous utilisez.

L’attribut `value` de chaque élément `<input>` contient un peu de code Razor (par exemple, `Request.Form["title"]`). Vous avez appris une version de cette astuce dans le didacticiel précédent pour conserver la valeur entrée dans la zone de texte (le cas échéant) après l’envoi du formulaire.

## <a name="getting-the-form-values"></a>Obtention des valeurs de formulaire

Ensuite, vous ajoutez du code qui traite le formulaire. Dans le plan, vous effectuerez les opérations suivantes :

1. Vérifiez si la page est en cours de publication (a été envoyée). Vous souhaitez que votre code s’exécute uniquement lorsque les utilisateurs ont cliqué sur le bouton, et non lors de la première exécution de la page.
2. Obtient les valeurs que l’utilisateur a entrées dans les zones de texte. Dans ce cas, comme le formulaire utilise le verbe `POST`, vous récupérez les valeurs de formulaire à partir de la collection `Request.Form`.
3. Insérez les valeurs sous la forme d’un nouvel enregistrement dans la table de base de données de *films* .

En haut du fichier, ajoutez le code suivant :

[!code-cshtml[Main](entering-data/samples/sample2.cshtml)]

Les premières lignes créent des variables (`title`, `genre`et `year`) pour contenir les valeurs des zones de texte. La ligne `if(IsPost)` permet de s’assurer que les variables sont définies *uniquement* lorsque les utilisateurs cliquent sur le bouton **Ajouter un film** , c’est-à-dire lorsque le formulaire a été publié.

Comme vous l’avez vu dans un didacticiel précédent, vous pouvez obtenir la valeur d’une zone de texte à l’aide d’une expression telle que `Request.Form["name"]`, où *nom* est le nom de l’élément `<input>`.

Les noms des variables (`title`, `genre`et `year`) sont arbitraires. Comme les noms que vous affectez à `<input>` éléments, vous pouvez les appeler comme vous le souhaitez. (Les noms des variables n’ont pas besoin de correspondre aux attributs de nom des éléments `<input>` du formulaire.) Mais comme avec les éléments de `<input>`, il est judicieux d’utiliser des noms de variables qui reflètent les données qu’ils contiennent. Lorsque vous écrivez du code, les noms cohérents vous permettent de vous souvenir facilement des données que vous utilisez.

## <a name="adding-data-to-the-database"></a>Ajout de données à la base de données

Dans le bloc de code que vous venez d’ajouter, juste *à l’intérieur* de l’accolade fermante (`}`) du bloc `if` (pas seulement à l’intérieur du bloc de code), ajoutez le code suivant :

[!code-csharp[Main](entering-data/samples/sample3.cs)]

Cet exemple est similaire au code que vous avez utilisé dans un didacticiel précédent pour extraire et afficher des données. La ligne qui commence par `db =` ouvre la base de données, comme précédemment, et la ligne suivante définit une instruction SQL, de nouveau comme vous l’avez vu précédemment. Toutefois, cette fois-ci, elle définit une instruction SQL `Insert Into`. L’exemple suivant illustre la syntaxe générale de l’instruction `Insert Into` :

`INSERT INTO table (column1, column2, column3, ...) VALUES (value1, value2, value3, ...)`

En d’autres termes, vous spécifiez la table dans laquelle insérer, puis vous répertoriez les colonnes à insérer dans, puis vous répertoriez les valeurs à insérer. (Comme indiqué précédemment, SQL ne respecte pas la casse, mais certaines personnes mettent en majuscules les mots clés pour faciliter la lecture de la commande.)

Les colonnes que vous insérez dans sont déjà répertoriées dans la commande, `(Title, Genre, Year)`. La partie intéressante est la façon dont vous récupérez les valeurs des zones de texte dans la partie `VALUES` de la commande. Au lieu de valeurs réelles, vous voyez `@0`, `@1`et `@2`, qui sont des espaces réservés de cours. Quand vous exécutez la commande (sur la ligne `db.Execute`), vous transmettez les valeurs que vous avez obtenues à partir des zones de texte.

**Important !** N’oubliez pas que la seule façon dont vous devez inclure les données entrées en ligne par un utilisateur dans une instruction SQL consiste à utiliser des espaces réservés, comme vous pouvez le voir ici (`VALUES(@0, @1, @2)`). Si vous concaténez une entrée d’utilisateur dans une instruction SQL, vous vous retrouvez à une attaque par injection SQL, comme expliqué dans les [concepts de base de formulaire dans pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=251581) (le didacticiel précédent).

Toujours à l’intérieur du bloc `if`, ajoutez la ligne suivante après la ligne de `db.Execute` :

[!code-css[Main](entering-data/samples/sample4.css)]

Une fois le nouveau film inséré dans la base de données, cette ligne vous redirige vers la page *films* pour vous permettre de voir le film que vous venez d’entrer. L’opérateur `~` signifie « la racine du site Web ». (L’opérateur `~` fonctionne uniquement dans les pages ASP.NET, et non en HTML en général.)

Le bloc de code complet ressemble à l’exemple suivant :

[!code-cshtml[Main](entering-data/samples/sample5.cshtml)]

## <a name="testing-the-insert-command-so-far"></a>Test de la commande Insert (jusqu’à présent)

Vous n’avez pas encore terminé, mais il est maintenant temps de tester.

Dans l’arborescence des fichiers dans WebMatrix, cliquez avec le bouton droit sur la page *AddMovie. cshtml* , puis cliquez sur **lancer dans le navigateur**.

![Page « Ajouter un film » dans le navigateur](entering-data/_static/image2.png)

(Si vous obtenez une page différente dans le navigateur, assurez-vous que l’URL est `http://localhost:nnnnn/AddMovie`), où *nnnnn* est le numéro de port que vous utilisez.)

Avez-vous obtenu une page d’erreur ? Si c’est le cas, lisez-le soigneusement et assurez-vous que le code semble exactement ce qui a été mentionné précédemment.

Entrez un film sous la forme &mdash; par exemple, utilisez « citoyen Kane », « fiction » et « 1941 ». (Ou tout.) Cliquez ensuite sur **Ajouter un film**.

Si tout se passe bien, vous êtes redirigé vers la page *films* . Assurez-vous que votre nouveau film est listé.

![Page films présentant un film récemment ajouté](entering-data/_static/image3.png)

## <a name="validating-user-input"></a>Validation de l'entrée utilisateur

Revenez à la page *AddMovie* ou réexécutez-la. Entrez un autre film, mais cette fois, entrez uniquement le titre &mdash; par exemple, entrez « Singin’in the Rain ». Cliquez ensuite sur **Ajouter un film**.

Vous êtes redirigé vers la page *films* . Vous pouvez trouver le nouveau film, mais il est incomplet.

![Page films indiquant un nouveau film dont certaines valeurs sont manquantes](entering-data/_static/image4.png)

Lorsque vous avez créé le tableau *films* , vous avez déclaré explicitement qu’aucun des champs ne peut avoir la valeur null. Ici, vous avez un formulaire de saisie pour les nouveaux films et vous laissez les champs vides. Il s’agit d’une erreur.

Dans ce cas, la base de données n’a pas réellement déclenché (ou *levé*) une erreur. Comme vous n’avez pas fourni de genre ou d’année, le code de la page *AddMovie* a traité ces valeurs comme des *chaînes vides*. Lors de l’exécution de la commande SQL `Insert Into`, les champs genre et Year ne comportaient pas de données utiles, mais ils n’avaient pas la valeur null.

Évidemment, vous ne souhaitez pas permettre aux utilisateurs d’entrer des informations de film demi-vides dans la base de données. La solution consiste à valider l’entrée de l’utilisateur. Au départ, la validation s’assure simplement que l’utilisateur a entré une valeur pour tous les champs (c’est-à-dire qu’aucun d’entre eux ne contient une chaîne vide).

> [!TIP]
> 
> **Chaînes NULL et vides**
> 
> En programmation, il y a une distinction entre les différentes notions de « aucune valeur ». En général, une valeur est *null* si elle n’a jamais été définie ou initialisée de quelque manière que ce soit. En revanche, une variable qui attend des données de type caractère (chaînes) peut être définie sur une *chaîne vide*. Dans ce cas, la valeur n’est pas null ; elle est simplement définie explicitement sur une chaîne de caractères dont la longueur est égale à zéro. Ces deux instructions montrent la différence :
> 
> [!code-csharp[Main](entering-data/samples/sample6.cs)]
> 
> C’est un peu plus compliqué, mais le point important est que `null` représente un type d’état indéterminé.
> 
> Maintenant, il est important de comprendre exactement quand une valeur est null et qu’il s’agit simplement d’une chaîne vide. Dans le code de la page *AddMovie* , vous pouvez obtenir les valeurs des zones de texte à l’aide de `Request.Form["title"]`, etc. Lorsque la page s’exécute pour la première fois (avant de cliquer sur le bouton), la valeur de `Request.Form["title"]` est null. Toutefois, lorsque vous envoyez le formulaire, `Request.Form["title"]` obtient la valeur de la zone de texte `title`. Ce n’est pas évident, mais une zone de texte vide n’est pas null. elle contient simplement une chaîne vide. Ainsi, lorsque le code s’exécute en réponse au clic sur le bouton, `Request.Form["title"]` contient une chaîne vide.
> 
> Pourquoi cette distinction est-elle importante ? Lorsque vous avez créé le tableau *films* , vous avez déclaré explicitement qu’aucun des champs ne peut avoir la valeur null. Mais ici, vous avez un formulaire de saisie pour les nouveaux films et vous laissez les champs vides. Vous vous attendez raisonnablement à ce que la base de données se plaindre lorsque vous avez essayé d’enregistrer de nouveaux films qui n’ont pas de valeurs pour le genre ou l’année. Mais c’est le point &mdash; même si vous laissez ces zones de texte vides, les valeurs ne sont pas nulles. Il s’agit de chaînes vides. En conséquence, vous pouvez enregistrer de nouveaux films dans la base de données avec ces colonnes vides &mdash; mais pas null ! des valeurs &mdash;. Par conséquent, vous devez vous assurer que les utilisateurs n’envoient pas une chaîne vide, ce que vous pouvez effectuer en validant l’entrée de l’utilisateur.

### <a name="the-validation-helper"></a>L’application auxiliaire de validation

Pages Web ASP.NET comprend un programme d’assistance &mdash; le `Validation` &mdash; d’assistance que vous pouvez utiliser pour vous assurer que les utilisateurs entrent des données qui répondent à vos besoins. Le `Validation` Helper est l’un des programmes d’assistance intégrés à pages Web ASP.NET. vous n’avez donc pas besoin de l’installer en tant que package à l’aide de NuGet, de la façon dont vous avez installé l’application auxiliaire Gravatar dans un didacticiel précédent.

Pour valider l’entrée de l’utilisateur, procédez comme suit :

- Utilisez du code pour spécifier que vous souhaitez exiger des valeurs dans les zones de texte de la page.
- Placez un test dans le code afin que les informations sur le film soient ajoutées à la base de données uniquement si tout valide correctement.
- Ajoutez du code dans le balisage pour afficher les messages d’erreur.

Dans le bloc de code de la page *AddMovie* , à droite en haut avant les déclarations de variable, ajoutez le code suivant :

[!code-csharp[Main](entering-data/samples/sample7.cs)]

Vous appelez `Validation.RequireField` une fois pour chaque champ (élément`<input>`) dans lequel vous souhaitez exiger une entrée. Vous pouvez également ajouter un message d’erreur personnalisé pour chaque appel, comme vous pouvez le voir ici. (Nous avons varié les messages simplement pour montrer que vous pouvez y placer tout ce que vous aimez.)

En cas de problème, vous souhaitez empêcher l’insertion des nouvelles informations de film dans la base de données. Dans le bloc `if(IsPost)`, utilisez `&&` (AND logique) pour ajouter une autre condition qui teste `Validation.IsValid()`. Lorsque vous avez terminé, le bloc de `if(IsPost)` entier ressemble à ce code :

[!code-csharp[Main](entering-data/samples/sample8.cs)]

En cas d’erreur de validation avec l’un des champs que vous avez enregistrés à l’aide du programme d’assistance `Validation`, la méthode `Validation.IsValid` retourne la valeur false. Dans ce cas, aucun code n’est exécuté dans ce bloc, donc aucune entrée de film non valide n’est insérée dans la base de données. Et bien sûr, vous n’êtes pas redirigé vers la page *films* .

Le bloc de code complet, y compris le code de validation, ressemble maintenant à l’exemple suivant :

[!code-cshtml[Main](entering-data/samples/sample9.cshtml?highlight=10)]

## <a name="displaying-validation-errors"></a>Affichage des erreurs de validation

La dernière étape consiste à afficher tous les messages d’erreur. Vous pouvez afficher des messages individuels pour chaque erreur de validation, ou vous pouvez afficher un résumé, ou les deux. Pour ce didacticiel, vous allez effectuer les deux opérations afin que vous puissiez voir comment il fonctionne.

En regard de chaque élément de `<input>` que vous validez, appelez la méthode `Html.ValidationMessage` et transmettez-lui le nom de l’élément `<input>` que vous validez. Vous placez la méthode `Html.ValidationMessage` juste à l’endroit où vous souhaitez que le message d’erreur s’affiche. Lorsque la page s’exécute, la méthode `Html.ValidationMessage` restitue un élément `<span>` où l’erreur de validation se produira. (S’il n’y a pas d’erreur, l’élément `<span>` est affiché, mais il n’y a pas de texte.)

Modifiez le balisage dans la page afin qu’il inclue une méthode de `Html.ValidationMessage` pour chacun des trois éléments `<input>` sur la page, comme dans cet exemple :

[!code-cshtml[Main](entering-data/samples/sample10.cshtml?highlight=3,8,13)]

Pour voir comment le résumé fonctionne, ajoutez également le balisage et le code suivants juste après l’élément `<h1>Add a Movie</h1>` sur la page :

[!code-cshtml[Main](entering-data/samples/sample11.cshtml)]

Par défaut, la méthode `Html.ValidationSummary` affiche tous les messages de validation dans une liste (élément `<ul>` qui se trouve à l’intérieur d’un élément `<div>`). Comme avec la méthode `Html.ValidationMessage`, le balisage du résumé des validations est toujours rendu ; s’il n’y a pas d’erreurs, aucun élément de liste n’est rendu.

Le résumé peut être une autre façon d’afficher des messages de validation plutôt que d’utiliser la méthode `Html.ValidationMessage` pour afficher chaque erreur spécifique à un champ. Vous pouvez aussi utiliser un résumé et les détails. Vous pouvez utiliser la méthode `Html.ValidationSummary` pour afficher une erreur générique, puis utiliser des appels de `Html.ValidationMessage` individuels pour afficher les détails.

La page complète ressemble maintenant à l’exemple suivant :

[!code-cshtml[Main](entering-data/samples/sample12.cshtml)]

C'est tout ! Vous pouvez maintenant tester la page en ajoutant un film, mais en conservant un ou plusieurs champs. Dans ce cas, vous voyez l’affichage d’erreur suivant :

![Page Ajouter un film indiquant les messages d’erreur de validation](entering-data/_static/image5.png)

## <a name="styling-the-validation-error-messages"></a>Stylisation des messages d’erreur de validation

Vous pouvez voir qu’il y a des messages d’erreur, mais qu’ils ne sont pas vraiment très bien. Toutefois, il existe un moyen simple de styliser les messages d’erreur.

Pour créer un style pour les messages d’erreur individuels qui sont affichés par `Html.ValidationMessage`, créez une classe de style CSS nommée `field-validation-error`. Pour définir l’apparence du résumé des validations, créez une classe de style CSS nommée `validation-summary-errors`.

Pour voir comment cette technique fonctionne, ajoutez un élément `<style>` à l’intérieur de la section `<head>` de la page. Définissez ensuite les classes de style nommées `field-validation-error` et `validation-summary-errors` qui contiennent les règles suivantes :

[!code-cshtml[Main](entering-data/samples/sample13.cshtml?highlight=4-17)]

Normalement, vous pouvez placer des informations de style dans un fichier *. CSS* distinct, mais pour des raisons de simplicité, vous pouvez les placer dans la page pour l’instant. (Plus loin dans ce didacticiel, vous allez déplacer les règles CSS dans un fichier *. CSS* distinct.)

En cas d’erreur de validation, la méthode `Html.ValidationMessage` restitue un élément `<span>` qui inclut `class="field-validation-error"`. En ajoutant une définition de style pour cette classe, vous pouvez configurer l’apparence du message. En cas d’erreur, la méthode `ValidationSummary` restitue de manière dynamique l’attribut `class="validation-summary-errors"`.

Réexécutez la page et quittez délibérément quelques champs. Les erreurs sont désormais plus visibles. (En fait, ils sont sempiternels, mais il s’agit simplement de montrer ce que vous pouvez faire.)

![Page Ajouter un film indiquant les erreurs de validation qui ont été stylisées](entering-data/_static/image6.png)

## <a name="adding-a-link-to-the-movies-page"></a>Ajout d’un lien vers la page films

Une dernière étape consiste à faciliter l’accès à la page *AddMovie* à partir de la liste des films d’origine.

Ouvrez de nouveau la page *films* . Après la balise `</div>` fermante qui suit le `WebGrid` Helper, ajoutez le balisage suivant :

[!code-cshtml[Main](entering-data/samples/sample14.cshtml)]

Comme vous l’avez vu précédemment, ASP.NET interprète l’opérateur `~` comme la racine du site Web. Vous n’êtes pas obligé d’utiliser l’opérateur `~` ; vous pouvez utiliser le balisage `<a href="./AddMovie">Add a movie</a>` ou d’une autre façon pour définir le chemin d’accès compréhensible par le langage HTML. Toutefois, l’opérateur `~` est une bonne approche générale lorsque vous créez des liens pour les pages Razor, car cela rend le site plus flexible. Si vous déplacez la page actuelle vers un sous-dossier, le lien s’affiche toujours dans la page *AddMovie* . (N’oubliez pas que l’opérateur `~` fonctionne uniquement dans les pages *. cshtml* . ASP.NET le comprend, mais ce n’est pas un code HTML standard.)

Lorsque vous avez terminé, exécutez la page *films* . Elle doit ressembler à cette page :

![Page films avec lien vers la page Ajouter des films](entering-data/_static/image7.png)

Cliquez sur le lien **Ajouter un film** pour vous assurer qu’il accède à la page *AddMovie* .

## <a name="coming-up-next"></a>À venir suivant

Dans le didacticiel suivant, vous apprendrez comment permettre aux utilisateurs de modifier des données qui se trouvent déjà dans la base de données.

## <a name="complete-listing-for-addmovie-page"></a>Liste complète de la page AddMovie

[!code-cshtml[Main](entering-data/samples/sample15.cshtml)]

## <a name="additional-resources"></a>Ressources supplémentaires

- [Présentation de la programmation Web ASP.NET à l’aide de la syntaxe Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Instruction SQL INSERT INTO](http://www.w3schools.com/sql/sql_insert.asp) sur le site W3Schools
- [Validation de l’entrée d’utilisateur dans les Sites pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=253002). Plus d’informations sur l’utilisation du programme d’assistance `Validation`.

> [!div class="step-by-step"]
> [Précédent](form-basics.md)
> [Suivant](updating-data.md)
