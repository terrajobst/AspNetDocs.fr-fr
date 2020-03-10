---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
title: Présentation de la mise à jour des données de la base de données de pages Web ASP.NET | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous montre comment mettre à jour (modifier) une entrée de base de données existante lorsque vous utilisez pages Web ASP.NET (Razor). Il part du principe que vous avez terminé la série th...
ms.author: riande
ms.date: 01/02/2018
ms.assetid: ac86ec9c-6b69-485b-b9e0-8b9127b13e6b
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
msc.type: authoredcontent
ms.openlocfilehash: 8f8bcfb7d9d2416a2699776cadbdaae8e12415ba
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78574227"
---
# <a name="introducing-aspnet-web-pages---updating-database-data"></a>Présentation de la mise à jour des données de la base de données pages Web ASP.NET

par [Tom FitzMacken](https://github.com/tfitzmac)

> Ce didacticiel vous montre comment mettre à jour (modifier) une entrée de base de données existante lorsque vous utilisez pages Web ASP.NET (Razor). Elle suppose que vous avez terminé la série [en entrant des données à l’aide de formulaires à l’aide de pages Web ASP.net](entering-data.md).
> 
> Ce que vous allez apprendre :
> 
> - Comment sélectionner un enregistrement individuel dans le `WebGrid` Helper.
> - Comment lire un enregistrement unique à partir d’une base de données.
> - Comment précharger un formulaire avec les valeurs de l’enregistrement de base de données.
> - Comment mettre à jour un enregistrement existant dans une base de données.
> - Comment stocker des informations dans la page sans les afficher.
> - Comment utiliser un champ masqué pour stocker des informations.
>   
> 
> Fonctionnalités/technologies présentées :
> 
> - Le programme d’assistance `WebGrid`.
> - Commande SQL `Update`.
> - Méthode `Database.Execute`
> - Champs masqués (`<input type="hidden">`).

## <a name="what-youll-build"></a>Contenu

Dans le didacticiel précédent, vous avez appris à ajouter un enregistrement à une base de données. Ici, vous allez apprendre à afficher un enregistrement à des fins de modification. Dans la page *films* , vous allez mettre à jour le `WebGrid` Helper afin qu’il affiche un lien de **modification** en regard de chaque film :

![Affichage de WebGrid incluant un lien « Modifier » pour chaque film](updating-data/_static/image1.png)

Lorsque vous cliquez sur le lien **modifier** , vous accédez à une page différente, où les informations sur le film se trouvent déjà dans un formulaire :

![Page de modification de la vidéo présentant le film à modifier](updating-data/_static/image2.png)

Vous pouvez modifier n’importe laquelle des valeurs. Lorsque vous envoyez les modifications, le code de la page met à jour la base de données et vous ramène à la liste des films.

Cette partie du processus fonctionne presque exactement comme la page *AddMovie. cshtml* que vous avez créée dans le didacticiel précédent, si bien que la majeure partie de ce didacticiel vous sera familière.

Il existe plusieurs façons d’implémenter une méthode de modification d’un film individuel. L’approche indiquée a été choisie, car elle est facile à implémenter et facile à comprendre.

## <a name="adding-an-edit-link-to-the-movie-listing"></a>Ajout d’un lien modifier à la liste des films

Pour commencer, vous allez mettre à jour la page *films* afin que chaque liste de films contienne également un lien **modifier** .

Ouvrez le fichier *movies. cshtml* .

Dans le corps de la page, modifiez le balisage `WebGrid` en ajoutant une colonne. Voici le balisage modifié :

[!code-html[Main](updating-data/samples/sample1.html?highlight=6)]

La nouvelle colonne est celle-ci :

[!code-html[Main](updating-data/samples/sample2.html)]

Le point de cette colonne est d’afficher un lien (élément`<a>`) dont le texte indique « modifier ». Nous allons ensuite créer un lien ressemblant à ce qui suit lors de l’exécution de la page, avec la valeur `id` différente pour chaque film :

[!code-css[Main](updating-data/samples/sample3.css)]

Ce lien appellera une page nommée *EditMovie*et passera la chaîne de requête `?id=7` à cette page.

La syntaxe de la nouvelle colonne peut paraître un peu complexe, mais c’est uniquement parce qu’elle regroupe plusieurs éléments. Chaque élément individuel est simple. Si vous vous concentrez uniquement sur l’élément `<a>`, vous voyez le balisage suivant :

[!code-html[Main](updating-data/samples/sample4.html)]

Informations générales sur le fonctionnement de la grille : la grille affiche des lignes, une pour chaque enregistrement de base de données, et affiche des colonnes pour chaque champ de l’enregistrement de base de données. Pendant la construction de chaque ligne de la grille, l’objet `item` contient l’enregistrement (élément) de la base de données pour cette ligne. Cette organisation vous donne un moyen de code pour accéder aux données de cette ligne. C’est ce que vous voyez ici : l’expression `item.ID` obtient la valeur d’ID de l’élément de base de données actuel. Vous pouvez récupérer les valeurs de la base de données (titre, genre ou année) de la même manière à l’aide de `item.Title`, `item.Genre`ou `item.Year`.

L’expression `"~/EditMovie?id=@item.ID` combine la partie codée en dur de l’URL cible (`~/EditMovie?id=`) avec cet ID dérivé de manière dynamique. (Vous avez vu l’opérateur `~` dans le didacticiel précédent. il s’agit d’un opérateur ASP.NET qui représente la racine du site Web actuel.)

Le résultat est que cette partie du balisage de la colonne produit simplement un aspect semblable au balisage suivant au moment de l’exécution :

[!code-xml[Main](updating-data/samples/sample5.xml)]

Naturellement, la valeur réelle de `id` sera différente pour chaque ligne.

## <a name="creating-a-custom-display-for-a-grid-column"></a>Création d’un affichage personnalisé pour une colonne de grille

À présent, revenez à la colonne de la grille. Les trois colonnes que vous aviez initialement dans la grille affichaient uniquement des valeurs de données (titre, genre et année). Vous avez spécifié cet affichage en passant le nom de la colonne de base de données &mdash; par exemple, `grid.Column("Title")`.

Cette nouvelle colonne de lien de **modification** est différente. Au lieu de spécifier un nom de colonne, vous transmettez un paramètre `format`. Ce paramètre vous permet de définir le balisage que le programme d’assistance de `WebGrid` restituera avec la valeur `item` pour afficher les données de la colonne en gras ou en vert ou dans le format de votre choix. Par exemple, si vous souhaitez que le titre apparaisse en gras, vous pouvez créer une colonne comme dans l’exemple suivant :

[!code-html[Main](updating-data/samples/sample6.html)]

(Les différents caractères de `@` que vous voyez dans la propriété `format` marquent la transition entre le balisage et une valeur de code.)

Une fois que vous connaissez la propriété de `format`, il est plus facile de comprendre comment la nouvelle colonne de lien de **modification** est regroupée :

[!code-html[Main](updating-data/samples/sample7.html)]

La colonne se compose *uniquement* du balisage qui restitue le lien, ainsi que d’informations (l’ID) extraites de l’enregistrement de la base de données pour la ligne.

> [!TIP]
> 
> **Paramètres nommés et paramètres positionnels pour une méthode**
> 
> Souvent, lorsque vous avez appelé une méthode et que vous lui avez passé des paramètres, vous avez simplement répertorié les valeurs des paramètres en les séparant par des virgules. Voici quelques exemples :
> 
> `db.Execute(insertCommand, title, genre, year)`
> 
> `Validation.RequireField("title", "You must enter a title")`
> 
> Nous n’avons pas mentionné le problème lorsque vous avez vu ce code pour la première fois, mais dans chaque cas, vous transmettez des paramètres aux méthodes dans un ordre spécifique &mdash;, à savoir, l’ordre dans lequel les paramètres sont définis dans cette méthode. Par `db.Execute` et `Validation.RequireFields`, si vous mélangez l’ordre des valeurs que vous transmettez, vous obtenez un message d’erreur lorsque la page s’exécute ou, au moins, des résultats étranges. En clair, vous devez connaître l’ordre dans lequel les paramètres doivent être transmis. (Dans WebMatrix, IntelliSense peut vous aider à déterminer le nom, le type et l’ordre des paramètres.)
> 
> Comme alternative au passage de valeurs dans l’ordre, vous pouvez utiliser des *paramètres nommés*. (Le passage de paramètres dans l’ordre est connu sous le nom d’utilisation de *paramètres positionnels*.) Pour les paramètres nommés, vous incluez explicitement le nom du paramètre lors du passage de sa valeur. Vous avez déjà utilisé des paramètres nommés plusieurs fois dans ces didacticiels. Exemple :
> 
> [!code-csharp[Main](updating-data/samples/sample8.cs)]
> 
> et
> 
> [!code-css[Main](updating-data/samples/sample9.css)]
> 
> Les paramètres nommés sont pratiques pour plusieurs situations, en particulier lorsqu’une méthode utilise de nombreux paramètres. Vous ne pouvez passer qu’un seul ou deux paramètres, mais les valeurs que vous souhaitez passer ne sont pas parmi les premières positions de la liste de paramètres. Une autre situation est lorsque vous souhaitez rendre votre code plus lisible en passant les paramètres dans l’ordre qui vous convient le mieux.
> 
> Évidemment, pour utiliser des paramètres nommés, vous devez connaître les noms des paramètres. WebMatrix IntelliSense peut vous *indiquer* les noms, mais il ne peut pas les remplir pour vous.

## <a name="creating-the-edit-page"></a>Création de la page de modification

Vous pouvez maintenant créer la page *EditMovie* . Quand les utilisateurs cliquent sur le lien **modifier** , ils finissent sur cette page.

Créez une page nommée *EditMovie. cshtml* et remplacez ce qui se trouve dans le fichier par le balisage suivant :

[!code-cshtml[Main](updating-data/samples/sample10.cshtml)]

Ce balisage et ce code sont semblables à ce que vous avez dans la page *AddMovie* . Il existe une petite différence dans le texte du bouton Envoyer. Comme avec la page *AddMovie* , il existe un appel `Html.ValidationSummary` qui affiche les erreurs de validation, le cas échéant. Cette fois, nous laissons des appels à `Validation.Message`, car les erreurs seront affichées dans le résumé des validations. Comme indiqué dans le didacticiel précédent, vous pouvez utiliser le résumé des validations et les messages d’erreur individuels dans différentes combinaisons.

Remarquez que l’attribut `method` de l’élément `<form>` a la valeur `post`. Comme avec la page *AddMovie. cshtml* , cette page apporte des modifications à la base de données. Par conséquent, ce formulaire doit effectuer une opération de `POST`. (Pour plus d’informations sur la différence entre les opérations de `GET` et de `POST`, consultez la barre latérale [sécurité des verbes obtenir, poster et http](form-basics.md#GET,_POST,_and_HTTP_Verb_Safety) dans le didacticiel sur les formulaires HTML.)

Comme vous l’avez vu dans un didacticiel précédent, les attributs `value` des zones de texte sont définis avec du code Razor afin de les précharger. Cette fois, cependant, vous utilisez des variables comme `title` et `genre` pour cette tâche au lieu de `Request.Form["title"]`:

`<input type="text" name="title" value="@title" />`

Comme précédemment, cette balise préchargera les valeurs de zone de texte avec les valeurs de film. Vous verrez dans un moment pourquoi il est pratique d’utiliser des variables cette fois-ci plutôt que d’utiliser l’objet `Request`.

Cette page contient également un élément `<input type="hidden">`. Cet élément stocke l’ID de film sans le rendre visible sur la page. L’ID est initialement passé à la page à l’aide d’une valeur de chaîne de requête (`?id=7` ou similaire dans l’URL). En plaçant la valeur d’ID dans un champ masqué, vous pouvez vous assurer qu’elle est disponible lorsque le formulaire est envoyé, même si vous n’avez plus accès à l’URL d’origine avec laquelle la page a été appelée.

Contrairement à la page *AddMovie* , le code de la page *EditMovie* a deux fonctions distinctes. La première fonction est que, lorsque la page est affichée pour la première fois (et *seulement* ensuite), le code obtient l’ID de la vidéo à partir de la chaîne de requête. Le code utilise ensuite l’ID pour lire le film correspondant dans la base de données et l’afficher (précharger) dans les zones de texte.

La deuxième fonction est que lorsque l’utilisateur clique sur le bouton **submit changes** , le code doit lire les valeurs des zones de texte et les valider. Le code doit également mettre à jour l’élément de base de données avec les nouvelles valeurs. Cette technique est similaire à l’ajout d’un enregistrement, comme vous l’avez vu dans *AddMovie*.

## <a name="adding-code-to-read-a-single-movie"></a>Ajout de code pour lire un seul film

Pour exécuter la première fonction, ajoutez le code suivant en haut de la page :

[!code-cshtml[Main](updating-data/samples/sample11.cshtml)]

La majeure partie de ce code se trouve à l’intérieur d’un bloc qui démarre `if(!IsPost)`. L’opérateur de `!` signifie « not ». par conséquent, l’expression signifie *si cette demande n’est pas une soumission de publication*, ce qui est un moyen indirect de dire *si cette demande est la première fois que cette page a été exécutée*. Comme indiqué précédemment, ce code ne doit s’exécuter *que* lors de la première exécution de la page. Si vous n’avez pas placé le code dans `if(!IsPost)`, il s’exécute à chaque fois que la page est appelée, que la première fois ou en réponse à un clic sur un bouton.

Notez que le code comprend un bloc `else` cette fois-ci. Comme nous l’avons dit quand nous avons introduit des blocs `if`, vous pouvez parfois exécuter un autre code si la condition que vous testez n’est pas vraie. C’est le cas ici. Si la condition est satisfaite (autrement dit, si l’ID passé à la page est OK), vous lisez une ligne de la base de données. Toutefois, si la condition n’est pas satisfaite, le bloc `else` s’exécute et le code définit un message d’erreur.

## <a name="validating-a-value-passed-to-the-page"></a>Validation d’une valeur passée à la page

Le code utilise `Request.QueryString["id"]` pour récupérer l’ID qui est passé à la page. Le code vérifie qu’une valeur a bien été passée pour l’ID. Si aucune valeur n’a été passée, le code définit une erreur de validation.

Ce code montre une autre façon de valider des informations. Dans le didacticiel précédent, vous travailliez avec le programme d’assistance `Validation`. Vous avez inscrit les champs à valider et ASP.NET automatiquement la validation et les erreurs affichées à l’aide de `Html.ValidationMessage` et `Html.ValidationSummary`. Dans ce cas, toutefois, vous ne validez pas véritablement l’entrée de l’utilisateur. Au lieu de cela, vous validez une valeur qui a été transmise à la page à partir d’un autre emplacement. Le programme d’assistance `Validation` ne le fait pas pour vous.

Par conséquent, vous vérifiez la valeur vous-même, en la testant avec `if(!Request.QueryString["ID"].IsEmpty()`). En cas de problème, vous pouvez afficher l’erreur à l’aide de `Html.ValidationSummary`, comme vous l’avez fait avec l’application auxiliaire `Validation`. Pour ce faire, appelez `Validation.AddFormError` et transmettez-lui un message à afficher. `Validation.AddFormError` est une méthode intégrée qui vous permet de définir des messages personnalisés qui s’intègrent au système de validation que vous connaissez déjà. (Plus loin dans ce didacticiel, nous décrirons comment rendre ce processus de validation un peu plus robuste.)

Après avoir vérifié qu’il existe un ID pour le film, le code lit la base de données, en recherchant un seul élément de base de données. (Vous avez probablement remarqué le modèle général pour les opérations de base de données : Ouvrez la base de données, définissez une instruction SQL, puis exécutez l’instruction.) Cette fois-ci, l’instruction SQL `Select` comprend `WHERE ID = @0`. Étant donné que l’ID est unique, un seul enregistrement peut être retourné.

La requête est exécutée à l’aide de `db.QuerySingle` (non `db.Query`, comme vous l’avez fait pour la liste des films) et le code place le résultat dans la variable `row`. Le nom `row` est arbitraire ; vous pouvez nommer les variables comme vous le souhaitez. Les variables initialisées en haut sont ensuite remplies avec les détails du film afin que ces valeurs puissent être affichées dans les zones de texte.

## <a name="testing-the-edit-page-so-far"></a>Test de la page de modification (jusqu’à présent)

Si vous souhaitez tester votre page, exécutez la page *films* maintenant et cliquez sur un lien **modifier** en regard de tout film. Vous verrez la page *EditMovie* avec les détails renseignés pour le film que vous avez sélectionné :

![Page de modification de la vidéo présentant le film à modifier](updating-data/_static/image3.png)

Notez que l’URL de la page comprend des éléments tels que `?id=10` (ou un autre nombre). Jusqu’à présent, vous avez testé que les liens de **modification** dans la page de *film* fonctionnent, que votre page lit l’ID à partir de la chaîne de requête et que la requête de base de données pour obtenir un seul enregistrement de film fonctionne.

Vous pouvez modifier les informations du film, mais rien ne se produit lorsque vous cliquez sur **soumettre les modifications**.

## <a name="adding-code-to-update-the-movie-with-the-users-changes"></a>Ajout de code pour mettre à jour le film avec les modifications de l’utilisateur

Dans le fichier *EditMovie. cshtml* , pour implémenter la deuxième fonction (enregistrement des modifications), ajoutez le code suivant à l’intérieur de l’accolade fermante du bloc `@`. (Si vous ne savez pas exactement où placer le code, vous pouvez consulter la [liste complète du code pour la page modifier le film](#Complete_Page_Listing_for_EditMovie) qui s’affiche à la fin de ce didacticiel.)

[!code-csharp[Main](updating-data/samples/sample12.cs)]

Là encore, le balisage et le code sont similaires au code de *AddMovie*. Le code se trouve dans un bloc `if(IsPost)`, car ce code s’exécute uniquement quand l’utilisateur clique sur le bouton **submit changes** &mdash; autrement dit, lorsque (et uniquement lorsque) le formulaire a été publié. Dans ce cas, vous n’utilisez pas un test comme `if(IsPost && Validation.IsValid())`, autrement dit, vous ne combinez pas les deux tests à l’aide de et. Dans cette page, vous déterminez tout d’abord s’il existe un envoi de formulaire (`if(IsPost)`) et que vous enregistrez uniquement les champs à des fins de validation. Vous pouvez ensuite tester les résultats de la validation (`if(Validation.IsValid()`). Le Flow est légèrement différent de celui de la page *AddMovie. cshtml* , mais l’effet est le même.

Vous pouvez obtenir les valeurs des zones de texte à l’aide de `Request.Form["title"]` et de code similaire pour les autres éléments `<input>`. Notez que cette fois-ci, le code obtient l’ID de film hors du champ masqué (`<input type="hidden">`). Lorsque la page s’est exécutée pour la première fois, le code a obtenu l’ID de la chaîne de requête. Vous obtenez la valeur du champ masqué pour vous assurer que vous obtenez l’ID du film qui a été affiché à l’origine, en cas de modification de la chaîne de requête depuis.

La différence très importante entre le code *AddMovie* et ce code est que dans ce code vous utilisez l’instruction SQL `Update` au lieu de l’instruction `Insert Into`. L’exemple suivant illustre la syntaxe de l’instruction SQL `Update` :

`UPDATE table SET col1="value", col2="value", col3="value" ... WHERE ID = value`

Vous pouvez spécifier toutes les colonnes dans n’importe quel ordre et vous n’avez pas nécessairement à mettre à jour chaque colonne pendant une opération de `Update`. (Vous ne pouvez pas mettre à jour l’ID lui-même, car cela permet d’enregistrer l’enregistrement en tant que nouvel enregistrement, ce qui n’est pas autorisé pour une opération `Update`.)

> [!NOTE] 
> 
> **Important** La clause `Where` avec l’ID est très importante, car c’est comme cela que la base de données sait quel enregistrement de base de données vous souhaitez mettre à jour. Si vous avez quitté la clause `Where`, la base de données met à jour *chaque* enregistrement dans la base de données. Dans la plupart des cas, il s’agit d’un incident.

Dans le code, les valeurs à mettre à jour sont transmises à l’instruction SQL à l’aide d’espaces réservés. Pour répéter ce que nous avons dit précédemment : pour des raisons de sécurité, utilisez *uniquement* des espaces réservés pour passer des valeurs à une instruction SQL.

Une fois que le code a utilisé `db.Execute` pour exécuter l’instruction `Update`, il redirige vers la page de liste, où vous pouvez voir les modifications.

> [!TIP] 
> 
> **Différentes instructions SQL, différentes méthodes**
> 
> Vous avez peut-être remarqué que vous utilisez des méthodes légèrement différentes pour exécuter différentes instructions SQL. Pour exécuter une requête `Select` qui retourne potentiellement plusieurs enregistrements, vous utilisez la méthode `Query`. Pour exécuter une requête `Select` dont vous savez qu’elle ne renverra qu’un seul élément de base de données, vous utilisez la méthode `QuerySingle`. Pour exécuter des commandes qui apportent des modifications, mais qui ne retournent pas d’éléments de base de données, vous utilisez la méthode `Execute`.
> 
> Vous devez avoir différentes méthodes, car chacune d’elles retourne des résultats différents, comme vous l’avez vu déjà dans la différence entre les `Query` et les `QuerySingle`. (La méthode `Execute` retourne en fait une valeur également &mdash;, à savoir, le nombre de lignes de base de données affectées par la commande &mdash; mais vous l’avez ignorée jusqu’à présent.)
> 
> Bien entendu, la méthode `Query` ne peut retourner qu’une seule ligne de base de données. Toutefois, ASP.NET traite toujours les résultats de la méthode `Query` en tant que collection. Même si la méthode ne retourne qu’une seule ligne, vous devez extraire cette seule ligne de la collection. Par conséquent, dans les situations où vous *savez* que vous n’obtiendrez qu’une seule ligne, il est un peu plus pratique d’utiliser `QuerySingle`.
> 
> Il existe d’autres méthodes qui effectuent des types spécifiques d’opérations de base de données. Vous trouverez une liste des méthodes de base de données dans la [référence rapide de l’API pages Web ASP.net](../../api-reference/asp-net-web-pages-api-reference.md#Data).

## <a name="making-validation-for-the-id-more-robust"></a>Rendre la validation de l’ID plus robuste

La première fois que la page est exécutée, vous recevez l’ID de la vidéo à partir de la chaîne de requête afin de pouvoir obtenir ce film à partir de la base de données. Vous avez fait en sorte qu’il y ait une valeur à rechercher, ce que vous avez fait à l’aide de ce code :

[!code-csharp[Main](updating-data/samples/sample13.cs)]

Vous avez utilisé ce code pour vous assurer que si un utilisateur parviendra à la page *EditMovies* sans sélectionner d’abord un film dans la page *films* , la page afficherait un message d’erreur convivial. (Dans le cas contraire, les utilisateurs verraient une erreur qui pourrait simplement les confondre.)

Toutefois, cette validation n’est pas très fiable. La page peut également être appelée avec les erreurs suivantes :

- L’ID n’est pas un nombre. Par exemple, la page peut être appelée avec une URL telle que `http://localhost:nnnnn/EditMovie?id=abc`.
- L’ID est un nombre, mais il fait référence à un film qui n’existe pas (par exemple, `http://localhost:nnnnn/EditMovie?id=100934`).

Si vous êtes curieux de voir les erreurs qui résultent de ces URL, exécutez la page *films* . Sélectionnez un film à modifier, puis remplacez l’URL de la page *EditMovie* par une URL qui contient un ID alphabétique ou l’ID d’un film inexistant.

Que devez-vous faire ? Le premier correctif consiste à s’assurer que non seulement un ID est passé à la page, mais que l’ID est un entier. Modifiez le code du test `!IsPost` pour qu’il ressemble à l’exemple suivant :

[!code-csharp[Main](updating-data/samples/sample14.cs)]

Vous avez ajouté une deuxième condition au test `IsEmpty`, liée à `&&` (AND logique) :

[!code-csharp[Main](updating-data/samples/sample15.cs)]

Vous vous souvenez peut-être du didacticiel [Introduction à la programmation de pages Web ASP.net](../introducing-razor-syntax-c.md) que les méthodes comme `AsBool` une `AsInt` convertir une chaîne de caractères en un autre type de données. La méthode `IsInt` (ainsi que d’autres, comme `IsBool` et `IsDateTime`) sont similaires. Toutefois, ils testent uniquement si vous *pouvez* convertir la chaîne, sans réellement effectuer la conversion. Ici, vous dites essentiellement *si la valeur de la chaîne de requête peut être convertie en un entier...* .

L’autre problème potentiel consiste à rechercher un film qui n’existe pas. Le code permettant d’obtenir un film ressemble à ce code :

[!code-csharp[Main](updating-data/samples/sample16.cs)]

Si vous transmettez une valeur `movieId` à la méthode `QuerySingle` qui ne correspond pas à un film réel, rien n’est retourné et les instructions qui suivent (par exemple, `title=row.Title`) génèrent des erreurs.

Là encore, il existe un correctif simple. Si la méthode `db.QuerySingle` ne retourne aucun résultat, la variable `row` a la valeur null. Ainsi, vous pouvez vérifier si la variable `row` est null avant d’essayer d’en extraire des valeurs. Le code suivant ajoute un bloc `if` autour des instructions qui récupèrent les valeurs de l’objet `row` :

[!code-csharp[Main](updating-data/samples/sample17.cs)]

Avec ces deux tests de validation supplémentaires, la page devient plus une preuve de la liste à puces. Le code complet de la branche `!IsPost` ressemble maintenant à l’exemple suivant :

[!code-csharp[Main](updating-data/samples/sample18.cs)]

Nous noterons une fois de plus que cette tâche est une bonne utilisation d’un bloc de `else`. Si les tests ne réussissent pas, les blocs de `else` définissent des messages d’erreur.

## <a name="adding-a-link-to-return-to-the-movies-page"></a>Ajout d’un lien pour revenir à la page films

Un détail final et utile consiste à ajouter un lien à la page *films* . Dans le workflow ordinaire des événements, les utilisateurs démarrent à la page *films* et cliquent sur un lien **modifier** . Cela les amène à la page *EditMovie* , où ils peuvent modifier le film et cliquer sur le bouton. Une fois que le code a traité la modification, il redirige vers la page *films* .

Cependant :

- L’utilisateur peut décider de ne pas modifier quoi que ce soit.
- Il se peut que l’utilisateur ait obtenu cette page sans cliquer d’abord sur un lien **modifier** dans la page *films* .

Dans les deux cas, vous souhaitez qu’ils reviennent facilement à la liste principale. Il s’agit d’un correctif simple &mdash; ajouter le balisage suivant juste après la balise de fermeture `</form>` dans le balisage :

[!code-html[Main](updating-data/samples/sample19.html)]

Ce balisage utilise la même syntaxe pour un élément `<a>` que vous avez vu ailleurs. L’URL comprend `~` pour signifier « la racine du site Web ».

## <a name="testing-the-movie-update-process"></a>Test du processus de mise à jour des films

Vous pouvez maintenant tester. Exécutez la page *films* , puis cliquez sur **modifier** en regard d’un film. Lorsque la page *EditMovie* s’affiche, modifiez le film et cliquez sur **soumettre les modifications**. Lorsque la liste des films s’affiche, vérifiez que vos modifications sont affichées.

Pour vous assurer que la validation fonctionne, cliquez sur **modifier** pour un autre film. Lorsque vous accédez à la page *EditMovie* , effacez le champ **genre** (ou le champ **year** , ou les deux) et essayez d’envoyer vos modifications. Une erreur s’affiche, comme vous pouvez vous y attendre :

![Page de modification de la vidéo présentant des erreurs de validation](updating-data/_static/image4.png)

Cliquez sur le lien **revenir à la liste des films** pour abandonner vos modifications et revenir à la page *films* .

## <a name="coming-up-next"></a>À venir suivant

Dans le didacticiel suivant, vous verrez comment supprimer un enregistrement de film.

## <a name="complete-listing-for-movie-page-updated-with-edit-links"></a>Liste complète des pages de films (mise à jour avec les liens de modification)

[!code-cshtml[Main](updating-data/samples/sample20.cshtml)]

<a id="Complete_Page_Listing_for_EditMovie"></a>
## <a name="complete-page-listing-for-edit-movie-page"></a>Liste complète des pages pour la page modifier la vidéo

[!code-cshtml[Main](updating-data/samples/sample21.cshtml)]

## <a name="additional-resources"></a>Ressources supplémentaires

- [Présentation de la programmation Web ASP.NET à l’aide de la syntaxe Razor](../../getting-started/introducing-razor-syntax-c.md)
- [Instruction SQL Update](http://www.w3schools.com/sql/sql_update.asp) sur le site W3Schools

> [!div class="step-by-step"]
> [Précédent](entering-data.md)
> [Suivant](deleting-data.md)
