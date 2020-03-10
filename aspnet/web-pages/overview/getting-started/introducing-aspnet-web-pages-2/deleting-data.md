---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: Présentation de la pages Web ASP.NET-suppression de données de base de données | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous montre comment supprimer une entrée de base de données individuelle. Il part du principe que vous avez terminé la série pour mettre à jour les données de la base de données dans ASP.NET Web PA...
ms.author: riande
ms.date: 01/02/2018
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: c8620fc1abc61d514bdc039c66f7a84e67e89abe
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78629037"
---
# <a name="introducing-aspnet-web-pages---deleting-database-data"></a>Présentation de la pages Web ASP.NET-suppression de données de base de données

par [Tom FitzMacken](https://github.com/tfitzmac)

> Ce didacticiel vous montre comment supprimer une entrée de base de données individuelle. Il part du principe que vous avez terminé la série via la [mise à jour des données de la base de données dans pages Web ASP.net](updating-data.md).
> 
> Ce que vous allez apprendre :
> 
> - Comment sélectionner un enregistrement individuel à partir d’une liste d’enregistrements.
> - Comment supprimer un enregistrement unique d’une base de données.
> - Comment vérifier qu’un clic a été effectué sur un bouton spécifique dans un formulaire.
>   
> 
> Fonctionnalités/technologies présentées :
> 
> - Le programme d’assistance `WebGrid`.
> - Commande SQL `Delete`.
> - La méthode `Database.Execute` pour exécuter une commande SQL `Delete`.

## <a name="what-youll-build"></a>Contenu

Dans le didacticiel précédent, vous avez appris à mettre à jour un enregistrement de base de données existant. Ce didacticiel est similaire, à ceci près qu’au lieu de mettre à jour l’enregistrement, vous allez le supprimer. Les processus sont pratiquement les mêmes, mais la suppression est plus simple. ce didacticiel est donc concis.

Dans la page *films* , vous allez mettre à jour le `WebGrid` Helper afin qu’il affiche un lien **supprimer** en regard de chaque film pour accompagner le lien de **modification** que vous avez ajouté précédemment.

![Page films présentant un lien supprimer pour chaque film](deleting-data/_static/image1.png)

Comme pour la modification, lorsque vous cliquez sur le lien **supprimer** , vous accédez à une autre page, où les informations sur le film se trouvent déjà dans un formulaire :

![Supprimer la page de film avec un film affiché](deleting-data/_static/image2.png)

Vous pouvez ensuite cliquer sur le bouton pour supprimer définitivement l’enregistrement.

## <a name="adding-a-delete-link-to-the-movie-listing"></a>Ajout d’un lien supprimer à la liste des films

Vous allez commencer par ajouter un lien **supprimer** à l’application auxiliaire `WebGrid`. Ce lien est similaire au lien **modifier** que vous avez ajouté dans un didacticiel précédent.

Ouvrez le fichier *movies. cshtml* .

Modifiez le balisage `WebGrid` dans le corps de la page en ajoutant une colonne. Voici le balisage modifié :

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

La nouvelle colonne est celle-ci :

[!code-html[Main](deleting-data/samples/sample2.html)]

La façon dont la grille est configurée, la colonne de **modification** est la plus à gauche dans la grille et la colonne de **suppression** est la plus à droite. (Il y a une virgule après la `Year` colonne maintenant, au cas où vous ne l’auriez pas remarqué.) Il n’y a rien de spécial quant à l’emplacement de ces colonnes de lien, et vous pourriez les placer facilement les unes à côté des autres. Dans ce cas, ils sont distincts pour compliquer la combinaison.

![Page films avec les liens de modification et détails marqués pour montrer qu’ils ne sont pas les uns à côté des autres](deleting-data/_static/image3.png)

La nouvelle colonne affiche un lien (élément`<a>`) dont le texte indique « Delete ». La cible du lien (son attribut `href`) est du code qui se résout finalement en un type similaire à cette URL, avec la valeur `id` différente pour chaque film :

[!code-css[Main](deleting-data/samples/sample3.css)]

Ce lien appellera une page nommée *DeleteMovie* et lui transmet l’ID de la vidéo que vous avez sélectionnée.

Ce didacticiel n’aborde pas en détail la façon dont ce lien est construit, car il est quasiment identique au lien **Edit** du didacticiel précédent ([mise à jour des données de la base de données dans pages Web ASP.net](updating-data.md)).

## <a name="creating-the-delete-page"></a>Création de la page de suppression

Vous pouvez maintenant créer la page qui sera la cible du lien **supprimer** dans la grille.

> [!NOTE] 
> 
> **Important** La technique de la première sélection d’un enregistrement à supprimer, puis l’utilisation d’une page et d’un bouton distincts pour confirmer le processus est extrêmement importante pour la sécurité. Comme vous avez lu *dans les* didacticiels précédents, la modification de votre site Web doit *toujours* être effectuée à l’aide d’un formulaire &mdash; à l’aide d’une opération http postale. Si vous avez fait en sorte qu’il soit possible de modifier le site simplement en cliquant sur un lien (c’est-à-dire à l’aide d’une opération d’extraction), les utilisateurs peuvent effectuer des requêtes simples sur votre site et supprimer vos données. Même un robot de moteur de recherche qui indexe votre site peut supprimer par inadvertance des données en suivant les liens ci-dessous.
> 
> Lorsque votre application permet aux utilisateurs de modifier un enregistrement, vous devez tout de même présenter l’enregistrement à l’utilisateur pour le modifier. Mais vous pouvez être tenté d’ignorer cette étape pour supprimer un enregistrement. N’ignorez pas cette étape. (Il est également utile pour les utilisateurs de voir l’enregistrement et de confirmer qu’ils suppriment l’enregistrement auquel ils sont destinés.)
> 
> Dans une série de didacticiels suivante, vous verrez comment ajouter une fonctionnalité de connexion afin qu’un utilisateur ait à se connecter avant de supprimer un enregistrement.

Créez une page nommée *DeleteMovie. cshtml* et remplacez ce qui se trouve dans le fichier par le balisage suivant :

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

Ce balisage est similaire aux pages *EditMovie* , sauf qu’au lieu d’utiliser des zones de texte (`<input type="text">`), le balisage comprend `<span>` éléments. Il n’y a rien à modifier. Il vous suffit d’afficher les détails du film pour que les utilisateurs puissent s’assurer qu’ils suppriment le film approprié.

Le balisage contient déjà un lien qui permet à l’utilisateur de revenir à la page de liste des films.

Comme dans la page *EditMovie* , l’ID du film sélectionné est stocké dans un champ masqué. (Elle est transmise dans la page à la première place sous la forme d’une valeur de chaîne de requête.) Il y a un appel de `Html.ValidationSummary` qui affichera des erreurs de validation. Dans ce cas, l’erreur peut être qu’aucun ID de film n’a été transmis à la page ou que l’ID de film n’est pas valide. Cette situation peut se produire si quelqu’un a exécuté cette page sans avoir au préalable sélectionné un film dans la page *films* .

La légende du bouton est **supprimer le film**et son attribut de nom est défini sur `buttonDelete`. L’attribut `name` sera utilisé dans le code pour identifier le bouton qui a envoyé le formulaire.

Vous devrez écrire du code sur 1) lire les détails du film lorsque la page est affichée pour la première fois et 2) supprimer le film lorsque l’utilisateur clique sur le bouton.

## <a name="adding-code-to-read-a-single-movie"></a>Ajout de code pour lire un seul film

En haut de la page *DeleteMovie. cshtml* , ajoutez le bloc de code suivant :

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

Ce balisage est identique au code correspondant dans la page *EditMovie* . Elle récupère l’ID de film de la chaîne de requête et utilise l’ID pour lire un enregistrement de la base de données. Le code comprend le test de validation (`IsInt()` et `row != null`) pour s’assurer que l’ID de film passé à la page est valide.

N’oubliez pas que ce code ne doit s’exécuter que lors de la première exécution de la page. Vous ne souhaitez pas relire l’enregistrement de film de la base de données quand l’utilisateur clique sur le bouton **supprimer le film** . Par conséquent, le code permettant de lire le film est à l’intérieur d’un test qui indique `if(!IsPost)` &mdash; autrement dit, *si la demande n’est pas une opération de publication (envoi de formulaire)* .

## <a name="adding-code-to-delete-the-selected-movie"></a>Ajout de code pour supprimer le film sélectionné

Pour supprimer le film lorsque l’utilisateur clique sur le bouton, ajoutez le code suivant juste à l’intérieur de l’accolade fermante du bloc `@` :

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

Ce code est similaire au code de mise à jour d’un enregistrement existant, mais plus simple. Le code exécute une instruction SQL `Delete`.

 Comme dans la page *EditMovie* , le code se trouve dans un bloc `if(IsPost)`. Cette fois-ci, la condition de `if()` est un peu plus compliquée : 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

Deux conditions s’imposent ici. La première est que la page est envoyée, comme vous l’avez vu avant &mdash; `if(IsPost)`.

La deuxième condition est `!Request["buttonDelete"].IsEmpty()`, ce qui signifie que la demande a un objet nommé `buttonDelete`. Il est vrai qu’il s’agit d’un moyen indirect de tester le bouton qui a envoyé le formulaire. Si un formulaire contient plusieurs boutons Envoyer, seul le nom du bouton sur lequel l’utilisateur a cliqué apparaît dans la demande. Par conséquent, logiquement, si le nom d’un bouton particulier apparaît dans la demande &mdash; ou comme indiqué dans le code, si ce bouton n’est pas vide &mdash; c’est le bouton qui a envoyé le formulaire.

L’opérateur `&&` signifie « and » (AND logique). Par conséquent, la condition `if` entière est...

*Cette demande est une publication (et non une demande initiale)*  
  
 AND  
  
*Le* *bouton `buttonDelete`était le bouton qui a envoyé le formulaire.*

Ce formulaire (en fait, cette page) ne contient qu’un seul bouton. par conséquent, le test supplémentaire pour `buttonDelete` n’est techniquement pas nécessaire. Toutefois, vous allez effectuer une opération qui supprimera définitivement les données. Vous souhaitez donc être aussi sûr que possible d’effectuer l’opération uniquement lorsque l’utilisateur l’a demandée explicitement. Par exemple, supposons que vous développiez cette page par la suite et y ajoutiez d’autres boutons. Même si vous avez cliqué sur le bouton `buttonDelete`, le code qui supprime le film s’exécutera uniquement.

Comme dans la page *EditMovie* , vous récupérez l’ID à partir du champ masqué, puis exécutez la commande SQL. La syntaxe de l’instruction `Delete` est la suivante :

`DELETE FROM table WHERE ID = value`

Il est essentiel d’inclure la clause `WHERE` et l’ID. Si vous omettez la clause WHERE, *tous les enregistrements de la table seront supprimés*. Comme vous l’avez vu, vous transmettez la valeur d’ID à la commande SQL à l’aide d’un espace réservé.

## <a name="testing-the-movie-delete-process"></a>Test du processus de suppression du film

Vous pouvez maintenant tester. Exécutez la page *films* , puis cliquez sur **supprimer** en regard d’un film. Lorsque la page *DeleteMovie* s’affiche, cliquez sur **supprimer le film**.

![Page supprimer le film avec le bouton supprimer le film mis en surbrillance](deleting-data/_static/image4.png)

Lorsque vous cliquez sur le bouton, le code supprime les films et retourne à la liste des films. Vous pouvez y rechercher le film supprimé et confirmer qu’il a été supprimé.

## <a name="coming-up-next"></a>À venir suivant

Le didacticiel suivant vous montre comment donner une apparence et une disposition communes à toutes les pages sur votre site.

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a>Liste complète des pages de films (mise à jour avec les liens supprimer)

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a>Liste complète de la page DeleteMovie

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a>Ressources supplémentaires

- [Présentation de la programmation Web ASP.NET à l’aide de la syntaxe Razor](../introducing-razor-syntax-c.md)
- [Instruction SQL Delete](http://www.w3schools.com/sql/sql_delete.asp) sur le site W3Schools

> [!div class="step-by-step"]
> [Précédent](updating-data.md)
> [Suivant](layouts.md)
