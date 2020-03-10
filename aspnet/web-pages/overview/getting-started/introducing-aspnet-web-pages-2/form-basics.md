---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
title: Présentation de la pages Web ASP.NET-notions de base des formulaires HTML | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous montre les principes fondamentaux de la création d’un formulaire d’entrée et de la gestion de l’entrée de l’utilisateur lorsque vous utilisez pages Web ASP.NET (Razor). Et maintenant...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 81ed82bf-b940-44f1-b94a-555d0cb7cc98
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
msc.type: authoredcontent
ms.openlocfilehash: f57661077ec3bb13f3d4ec41b130bda4d2fb9070
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78574283"
---
# <a name="introducing-aspnet-web-pages---html-form-basics"></a>Présentation des notions de base des formulaires pages Web ASP.NET-HTML

par [Tom FitzMacken](https://github.com/tfitzmac)

> Ce didacticiel vous montre les principes fondamentaux de la création d’un formulaire d’entrée et de la gestion de l’entrée de l’utilisateur lorsque vous utilisez pages Web ASP.NET (Razor). Et maintenant que vous disposez d’une base de données, vous utiliserez vos compétences de formulaire pour permettre aux utilisateurs de rechercher des films spécifiques dans la base de données. Il part du principe que vous avez terminé la série jusqu’à la [Présentation de l’affichage des données à l’aide de pages Web ASP.net](/aspnet/web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data).
> 
> Ce que vous allez apprendre :
> 
> - Comment créer un formulaire à l’aide d’éléments HTML standard.
> - Comment lire l’entrée de l’utilisateur dans un formulaire.
> - Comment créer une requête SQL qui obtient des données de manière sélective à l’aide d’un terme de recherche fourni par l’utilisateur.
> - Comment faire en sorte que les champs de la page « se souviennent » de ce que l’utilisateur a entré.
>   
> 
> Fonctionnalités/technologies présentées :
> 
> - Objet `Request`.
> - Clause SQL `Where`.

## <a name="what-youll-build"></a>Contenu

Dans le didacticiel précédent, vous avez créé une base de données, vous y avez ajouté des données, puis utilisé le programme d’assistance `WebGrid` pour afficher les données. Dans ce didacticiel, vous allez ajouter une zone de recherche qui vous permet de trouver des films d’un genre spécifique ou dont le titre contient le mot que vous entrez. (Par exemple, vous pourrez trouver tous les films dont le genre est « action » ou dont le titre contient « Harry » ou « Adventure »)

Lorsque vous avez terminé ce didacticiel, vous disposez d’une page similaire à celle-ci :

![Page films avec recherche de genre et de titre](form-basics/_static/image1.png)

La partie listing de la page est la même que dans le dernier didacticiel &mdash; une grille. La différence est que la grille n’affiche que les films que vous avez recherchés.

## <a name="about-html-forms"></a>À propos des formulaires HTML

(Si vous avez de l’expérience dans la création de formulaires HTML et avec la différence entre `GET` et `POST`, vous pouvez ignorer cette section.)

Un formulaire contient des éléments d’entrée utilisateur &mdash; des zones de texte, des boutons, des cases d’option, des cases à cocher, des listes déroulantes, etc. Les utilisateurs remplissent ces contrôles ou effectuent des sélections, puis envoient le formulaire en cliquant sur un bouton.

La syntaxe HTML de base d’un formulaire est illustrée dans cet exemple :

[!code-html[Main](form-basics/samples/sample1.html)]

Quand ce balisage s’exécute dans une page, il crée un formulaire simple qui ressemble à l’illustration suivante :

![Formulaire HTML de base tel qu’il est affiché dans le navigateur](form-basics/_static/image2.png)

L’élément `<form>` encadre les éléments HTML à envoyer. (Une erreur facile à effectuer consiste à ajouter des éléments à la page, mais à oublier de les placer à l’intérieur d’un élément `<form>`. Dans ce cas, rien n’est envoyé.) L’attribut `method` indique au navigateur comment envoyer l’entrée utilisateur. Vous définissez cette valeur sur `post` si vous effectuez une mise à jour sur le serveur ou sur `get` si vous récupérez simplement des données du serveur.

<a id="GET,_POST,_and_HTTP_Verb_Safety"></a>

> [!TIP] 
> 
> **RÉCUPÉRATION, publication et sécurité des verbes HTTP**
> 
> HTTP, le protocole que les navigateurs et les serveurs utilisent pour échanger des informations, est remarquablement simple dans ses opérations de base. Les navigateurs n’utilisent que quelques verbes pour effectuer des demandes aux serveurs. Lorsque vous écrivez du code pour le Web, il est utile de comprendre ces verbes et la façon dont le navigateur et le serveur les utilisent. Les verbes les plus couramment utilisés sont les suivants :
> 
> - `GET`. Le navigateur utilise ce verbe pour extraire un résultat du serveur. Par exemple, lorsque vous tapez une URL dans votre navigateur, le navigateur effectue une opération de `GET` pour demander la page souhaitée. Si la page comprend des graphiques, le navigateur effectue des opérations de `GET` supplémentaires pour obtenir les images. Si l’opération de `GET` doit passer des informations au serveur, les informations sont transmises dans le cadre de l’URL dans la chaîne de requête.
> - `POST`. Le navigateur envoie une demande de `POST` pour envoyer des données à ajouter ou à modifier sur le serveur. Par exemple, le verbe `POST` est utilisé pour créer des enregistrements dans une base de données ou pour modifier des enregistrements existants. La plupart du temps, lorsque vous remplissez un formulaire et cliquez sur le bouton envoyer, le navigateur effectue une opération de `POST`. Dans une opération de `POST`, les données passées au serveur se trouvent dans le corps de la page.
> 
> Une distinction importante entre ces verbes est qu’une opération de `GET` n’est pas censée modifier quoi que ce soit sur le serveur, ou pour la mettre de manière légèrement plus abstraite, une opération de `GET` n’entraîne pas de modification de l’État sur le serveur. Vous pouvez effectuer une opération de `GET` sur les mêmes ressources autant de fois que vous le souhaitez, et ces ressources ne changent pas. (Une opération de `GET` est souvent considérée comme « sûre », ou pour utiliser un terme technique, est *idempotent*). À l’inverse, bien entendu, une demande de `POST` modifie une valeur sur le serveur chaque fois que vous effectuez l’opération.
> 
> Deux exemples vous aideront à illustrer cette distinction. Lorsque vous effectuez une recherche à l’aide d’un moteur tel que Bing ou Google, vous remplissez un formulaire qui se compose d’une zone de texte, puis vous cliquez sur le bouton de recherche. Le navigateur effectue une opération de `GET`, avec la valeur que vous avez entrée dans la zone transmise dans le cadre de l’URL. L’utilisation d’une opération de `GET` pour ce type de formulaire est correcte, car une opération de recherche ne modifie aucune ressource sur le serveur, elle récupère simplement les informations.
> 
> Considérons à présent le processus de classement d’un contenu en ligne. Renseignez les détails de la commande, puis cliquez sur le bouton Envoyer. Cette opération est une demande de `POST`, car l’opération entraîne des modifications sur le serveur, telles qu’un nouvel enregistrement de commande, une modification de vos informations de compte et peut-être de nombreuses autres modifications. Contrairement à l’opération `GET`, vous ne pouvez pas répéter votre demande de `POST`. Si vous l’avez fait, chaque fois que vous avez envoyé la demande, vous générez une nouvelle commande sur le serveur. (Dans les cas de ce type, les sites Web vous avertissent souvent de ne pas cliquer plusieurs fois sur un bouton envoyer, ou ils désactivent le bouton Envoyer pour ne pas renvoyer le formulaire par inadvertance.)
> 
> Au cours de ce didacticiel, vous allez utiliser une opération de `GET` et une opération `POST` pour travailler avec des formulaires HTML. Nous expliquerons dans chaque cas pourquoi le verbe que vous utilisez est celui qui convient.
> 
> (Pour en savoir plus sur les verbes HTTP, consultez l’article [définitions de méthode](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) sur le site W3C.)

La plupart des éléments d’entrée utilisateur sont des éléments de `<input>` HTML. Elles ressemblent à `<input type="type" name="name">,` où *type* indique le type de contrôle d’entrée utilisateur souhaité. Ces éléments sont les plus courants :

- Zone de texte : `<input type="text">`
- Case à cocher : `<input type="check">`
- Case d’option : `<input type="radio">`
- Bouton : `<input type="button">`
- Bouton Envoyer : `<input type="submit">`

Vous pouvez également utiliser l’élément `<textarea>` pour créer une zone de texte multiligne et l’élément `<select>` pour créer une liste déroulante ou une liste déroulante. (Pour plus d’informations sur les éléments de formulaire HTML, consultez [formulaires HTML et entrée](http://www.w3schools.com/html/html_forms.asp) sur le site W3Schools.)

L’attribut `name` est très important, car le nom est la manière dont vous obtiendrez la valeur de l’élément ultérieurement, comme vous le verrez bientôt.

La partie intéressante est ce que vous, le développeur de pages, faites avec l’entrée de l’utilisateur. Aucun comportement intégré n’est associé à ces éléments. Au lieu de cela, vous devez obtenir les valeurs que l’utilisateur a entrées ou sélectionnées et faire un compte. C’est ce que vous allez apprendre dans ce didacticiel.

> [!TIP] 
> 
> **HTML5 et formulaires d’entrée**
> 
> Comme vous le savez peut-être, le code HTML est en transition et la version la plus récente (HTML5) prend en charge des méthodes plus intuitives permettant aux utilisateurs d’entrer des informations. Par exemple, dans HTML5, vous (le développeur de pages) pouvez indiquer à la page que vous souhaitez que l’utilisateur entre une date. Le navigateur peut ensuite afficher automatiquement un calendrier au lieu de demander à l’utilisateur d’entrer une date manuellement. Toutefois, HTML5 est nouveau et n’est pas encore pris en charge dans tous les navigateurs.
> 
> Pages Web ASP.NET prend en charge l’entrée HTML5 dans l’étendue du navigateur de l’utilisateur. Pour obtenir une idée des nouveaux attributs de l’élément `<input>` dans HTML5, consultez [attribut de type HTML &lt;entrée&gt;](http://www.w3schools.com/html/html_form_input_types.asp) sur le site W3Schools.

## <a name="creating-the-form"></a>Création du formulaire

Dans WebMatrix, dans l’espace de travail **fichiers** , ouvrez la page *movies. cshtml* .

Après la balise `</h1>` fermante et avant la balise `<div>` ouvrante de l’appel `grid.GetHtml`, ajoutez le balisage suivant :

[!code-html[Main](form-basics/samples/sample2.html)]

Ce balisage crée un formulaire qui a une zone de texte nommée `searchGenre` et un bouton Envoyer. La zone de texte et le bouton Envoyer sont placés dans un élément `<form>` dont l’attribut `method` a la valeur `get`. (N’oubliez pas que si vous ne placez pas la zone de texte et le bouton Envoyer à l’intérieur d’un élément `<form>`, rien n’est envoyé lorsque vous cliquez sur le bouton.) Vous utilisez le verbe `GET` ici, car vous créez un formulaire qui n’apporte aucune modification sur le serveur : il se contente d’effectuer une recherche. (Dans le didacticiel précédent, vous avez utilisé une méthode `post`, qui vous permet d’envoyer des modifications au serveur. Vous verrez que dans le didacticiel suivant.)

Exécutez la page. Bien que vous n’ayez défini aucun comportement pour le formulaire, vous pouvez voir à quoi il ressemble :

![Page films avec zone de recherche pour genre](form-basics/_static/image3.png)

Entrez une valeur dans la zone de texte, par exemple « comédie ». Cliquez ensuite sur **Rechercher un genre**.

Prenez note de l’URL de la page. Étant donné que vous affectez à l’attribut `method` de l’élément `<form>` la valeur `get`, la valeur que vous avez entrée fait maintenant partie de la chaîne de requête dans l’URL, comme suit :

`http://localhost:45661/Movies.cshtml?searchGenre=Comedy`

## <a name="reading-form-values"></a>Lecture des valeurs de formulaire

La page contient déjà du code qui récupère les données de la base de données et affiche les résultats dans une grille. À présent, vous devez ajouter du code qui lit la valeur de la zone de texte afin de pouvoir exécuter une requête SQL qui comprend le terme recherché.

Étant donné que vous définissez la méthode du formulaire sur `get`, vous pouvez lire la valeur qui a été entrée dans la zone de texte à l’aide d’un code similaire à ce qui suit :

`var searchTerm = Request.QueryString["searchGenre"];`

L’objet `Request.QueryString` (la propriété `QueryString` de l’objet `Request`) comprend les valeurs des éléments qui ont été envoyés dans le cadre de l’opération de `GET`. La propriété `Request.QueryString` contient une *collection* (une liste) des valeurs soumises dans le formulaire. Pour obtenir une valeur individuelle, vous spécifiez le nom de l’élément souhaité. C’est la raison pour laquelle vous devez avoir un attribut `name` sur l’élément `<input>` (`searchTerm`) qui crée la zone de texte. (Pour plus d’informations sur l’objet `Request`, consultez la [barre latérale](#BKMK_TheRequestObject) ultérieurement.)

Il est assez simple de lire la valeur de la zone de texte. Toutefois, si l’utilisateur n’a rien entré dans la zone de texte, mais a cliqué sur **Rechercher** quand même, vous pouvez ignorer ce clic, puisqu’il n’y a rien à rechercher.

Le code suivant est un exemple qui montre comment implémenter ces conditions. (Vous n’avez pas encore ajouté ce code ; vous le ferez dans un moment.)

[!code-csharp[Main](form-basics/samples/sample3.cs)]

Le test s’arrête de cette façon :

- Obtient la valeur de `Request.QueryString["searchGenre"]`, à savoir la valeur qui a été entrée dans l’élément `<input>` nommé `searchGenre`.
- Déterminez si elle est vide à l’aide de la méthode `IsEmpty`. Cette méthode est le moyen standard de déterminer si un élément (par exemple, un élément de formulaire) contient une valeur. Mais vraiment, vous vous souciez uniquement s’il ne s’agit *pas* d’un vide, par conséquent...
- Ajoutez l’opérateur `!` devant le test `IsEmpty`. (L’opérateur `!` signifie NOT logique).

En anglais, l’intégralité de la condition de `if` se traduit comme suit : *si l’élément searchGenre du formulaire n’est pas vide, alors...*

Ce bloc définit la phase de création d’une requête qui utilise le terme de recherche. Vous le ferez dans la section suivante.

<a id="BKMK_TheRequestObject"></a>

> [!TIP] 
> 
> **Objet de la requête**
> 
> L’objet `Request` contient toutes les informations que le navigateur envoie à votre application lorsqu’une page est demandée ou envoyée. Cet objet inclut toutes les informations fournies par l’utilisateur, telles que les valeurs de zone de texte ou un fichier à charger. Il comprend également toutes sortes d’informations supplémentaires, telles que les cookies, les valeurs de la chaîne de requête d’URL (le cas échéant), le chemin d’accès de la page en cours d’exécution, le type de navigateur utilisé par l’utilisateur, la liste des langues définies dans le navigateur. , et bien plus encore.
> 
> L’objet `Request` est une *collection* (liste) de valeurs. Vous récupérez une valeur individuelle de la collection en spécifiant son nom :
> 
> `var someValue = Request["name"];`
> 
> L’objet `Request` expose en fait plusieurs sous-ensembles. Exemple :
> 
> - `Request.Form` vous donne des valeurs à partir d’éléments à l’intérieur de l’élément `<form>` envoyé si la requête est une requête `POST`.
> - `Request.QueryString` vous donne uniquement les valeurs de la chaîne de requête de l’URL. (Dans une URL telle que `http://mysite/myapp/page?searchGenre=action&page=2`, la section `?searchGenre=action&page=2` de l’URL est la chaîne de requête.)
> - `Request.Cookies` collection vous donne accès aux cookies envoyés par le navigateur.
> 
> Pour obtenir une valeur que vous connaissez dans le formulaire envoyé, vous pouvez utiliser `Request["name"]`. Vous pouvez également utiliser les versions plus spécifiques `Request.Form["name"]` (pour les demandes `POST`) ou `Request.QueryString["name"]` (pour les demandes `GET`). Bien entendu, *Name* est le nom de l’élément à récupérer.
> 
> Le nom de l’élément que vous souhaitez obtenir doit être unique au sein de la collection que vous utilisez. C’est la raison pour laquelle l’objet `Request` fournit les sous-ensembles comme `Request.Form` et `Request.QueryString`. Supposons que votre page contient un élément de formulaire nommé `userName` et qu’elle contienne *également* un cookie nommé `userName`. Si vous recevez `Request["userName"]`, il est ambigu si vous souhaitez la valeur de formulaire ou le cookie. Toutefois, si vous recevez `Request.Form["userName"]` ou `Request.Cookie["userName"]`, vous êtes explicite en ce qui concerne la valeur à obtenir.
> 
> Il est recommandé d’être spécifique et d’utiliser le sous-ensemble de `Request` qui vous intéresse, comme `Request.Form` ou `Request.QueryString`. Pour les pages simples que vous créez dans ce didacticiel, cela ne fait probablement aucune différence. Toutefois, lorsque vous créez des pages plus complexes, l’utilisation de la version explicite `Request.Form` ou `Request.QueryString` peut vous aider à éviter les problèmes qui peuvent survenir lorsque la page contient un formulaire (ou plusieurs formulaires), des cookies, des valeurs de chaîne de requête, etc.

## <a name="creating-a-query-by-using-a-search-term"></a>Création d’une requête à l’aide d’un terme de recherche

Maintenant que vous savez comment obtenir le terme de recherche entré par l’utilisateur, vous pouvez créer une requête qui l’utilise. N’oubliez pas que pour récupérer tous les éléments de film de la base de données, vous utilisez une requête SQL qui ressemble à l’instruction suivante :

`SELECT * FROM Movies`

Pour obtenir uniquement certains films, vous devez utiliser une requête qui comprend une clause `Where`. Cette clause vous permet de définir une condition sur laquelle les lignes sont retournées par la requête. Voici un exemple :

`SELECT * FROM Movies WHERE Genre = 'Action'`

Le format de base est `WHERE column = value`. Vous pouvez utiliser différents opérateurs en plus de `=`, comme `>` (supérieur à), `<` (inférieur à), `<>` (différent de), `<=` (inférieur ou égal à), etc., en fonction de ce que vous recherchez.

Au cas où vous vous poseriez la question, les instructions SQL ne respectent pas la casse &mdash; `SELECT` est identique à `Select` (ou même `select`). Toutefois, les gens mettent souvent en majuscules les mots clés dans une instruction SQL, comme `SELECT` et `WHERE`, pour faciliter leur lecture.

### <a name="passing-the-search-term-as-a-parameter"></a>Passage du terme de recherche en tant que paramètre

La recherche d’un genre spécifique est suffisamment simple (`WHERE Genre = 'Action'`), mais vous souhaitez pouvoir Rechercher le genre que l’utilisateur entre. Pour ce faire, vous créez en tant que requête SQL qui comprend un espace réservé pour la valeur à rechercher. Elle doit ressembler à la commande suivante :

`SELECT * FROM Movies WHERE Genre = @0`

L’espace réservé est le caractère `@` suivi de zéro. Comme vous pouvez le deviner, une requête peut contenir plusieurs espaces réservés et se nommer `@0`, `@1`, `@2`, etc.

Pour configurer la requête et lui transmettre réellement la valeur, vous utilisez le code comme suit :

[!code-sql[Main](form-basics/samples/sample4.sql)]

Ce code est similaire à ce que vous avez déjà fait pour afficher des données dans la grille. Les seules différences sont les suivantes :

- La requête contient un espace réservé (`WHERE Genre = @0"`).
- La requête est placée dans une variable (`selectCommand`); avant, vous avez passé directement la requête à la méthode `db.Query`.
- Lorsque vous appelez la méthode `db.Query`, vous transmettez à la fois la requête et la valeur à utiliser pour l’espace réservé. (Si la requête comportait plusieurs espaces réservés, transmettez-les tous sous forme de valeurs séparées à la méthode.)

Si vous placez tous ces éléments ensemble, vous recevez le code suivant :

[!code-csharp[Main](form-basics/samples/sample5.cs)]

> [!NOTE] 
> 
> **Important !** L’utilisation d’espaces réservés (comme `@0`) pour passer des valeurs à une commande SQL est *extrêmement importante* pour la sécurité. La façon dont vous le voyez ici, avec des espaces réservés pour les données variables, est le seul moyen de construire des commandes SQL.
> 
> Ne construisez jamais une instruction SQL en rassemblant (concaténation) le texte littéral et les valeurs que vous recevez de l’utilisateur. La concaténation de l’entrée d’utilisateur dans une instruction SQL ouvre votre site à une *attaque par injection SQL* où un utilisateur malveillant soumet des valeurs à votre page qui piratent votre base de données. (Pour plus d’informations, consultez l’article [injection SQL](https://msdn.microsoft.com/library/ms161953.aspx) sur le site Web MSDN.)

## <a name="updating-the-movies-page-with-search-code"></a>Mise à jour de la page films avec code de recherche

Vous pouvez maintenant mettre à jour le code dans le fichier *movies. cshtml* . Pour commencer, remplacez le code dans le bloc de code en haut de la page par le code suivant :

[!code-csharp[Main](form-basics/samples/sample6.cs)]

La différence ici est que vous avez placé la requête dans la variable `selectCommand`, que vous passerez à `db.Query` ultérieurement. En plaçant l’instruction SQL dans une variable, vous pouvez modifier l’instruction, ce qui vous permet d’effectuer la recherche.

Vous avez également supprimé ces deux lignes, que vous allez recopier plus tard :

[!code-csharp[Main](form-basics/samples/sample7.cs)]

Vous ne souhaitez pas encore exécuter la requête (autrement dit, appeler `db.Query`) et vous ne souhaitez pas non plus initialiser le `WebGrid` Helper. Vous effectuerez ces opérations après avoir déterminé l’instruction SQL à exécuter.

Après ce bloc réécrit, vous pouvez ajouter la nouvelle logique pour gérer la recherche. Le code complet ressemble à ce qui suit. Mettez à jour le code dans votre page pour qu’il corresponde à l’exemple suivant :

[!code-cshtml[Main](form-basics/samples/sample8.cshtml)]

La page fonctionne maintenant comme suit. À chaque exécution de la page, le code ouvre la base de données et la variable `selectCommand` est définie sur l’instruction SQL qui obtient tous les enregistrements de la table `Movies`. Le code initialise également la variable `searchTerm`.

Toutefois, si la requête actuelle comprend une valeur pour l’élément `searchGenre`, le code définit `selectCommand` à une requête différente, à savoir, à une requête qui comprend la clause `Where` pour rechercher un genre. Il définit également `searchTerm` sur ce qui a été passé pour la zone de recherche (qui peut être Nothing).

Quelle que soit l’instruction SQL qui se trouve dans `selectCommand`, le code appelle ensuite `db.Query` pour exécuter la requête, en lui transmettant ce qui se trouve dans `searchTerm`. S’il n’y a rien dans `searchTerm`, cela n’a pas d’importance, car dans ce cas, il n’y a aucun paramètre pour transmettre la valeur à `selectCommand` quand même.

Enfin, le code initialise le programme d’assistance `WebGrid` à l’aide des résultats de la requête, comme auparavant.

Vous pouvez voir qu’en plaçant l’instruction SQL et le terme de recherche dans des variables, vous avez ajouté de la flexibilité au code. Comme vous le verrez plus loin dans ce didacticiel, vous pouvez utiliser ce Framework de base et continuer à ajouter de la logique pour différents types de recherche.

## <a name="testing-the-search-by-genre-feature"></a>Test de la fonctionnalité de recherche par genre

Dans WebMatrix, exécutez la page *movies. cshtml* . La page s’affiche avec la zone de texte correspondant au genre.

Entrez un genre que vous avez entré pour l’un de vos enregistrements de test, puis cliquez sur **Rechercher**. Cette fois, vous voyez une liste des films qui correspondent à ce genre :

![Liste des pages de films après la recherche du genre « Comedies »](form-basics/_static/image4.png)

Entrez un autre genre et recommencez la recherche. Essayez d’entrer le genre en utilisant des minuscules ou des majuscules afin de voir que la recherche ne respecte pas la casse.

## <a name="remembering-what-the-user-entered"></a>« Mémorisation » de la saisie de l’utilisateur

Vous avez peut-être remarqué qu’une fois que vous avez entré un genre et cliqué sur le **genre de recherche**, vous avez vu une liste pour ce genre. Toutefois, la zone de texte de recherche est vide &mdash; en d’autres termes, elle n’a pas oublié ce que vous avez entré.

Il est important de comprendre pourquoi ce comportement se produit. Lorsque vous envoyez une page, le navigateur envoie une demande au serveur Web. Quand ASP.NET obtient la demande, il crée une nouvelle instance de la page, y exécute le code, puis restitue à nouveau la page dans le navigateur. En effet, toutefois, la page ne sait pas que vous travailliez avec une version précédente d’elle-même. Tout ce qu’il sait, c’est qu’il a reçu une requête qui contient des données de formulaire.

Chaque fois que vous demandez une page &mdash; pour la première fois ou en la soumettant &mdash; vous recevez une nouvelle page. Le serveur Web n’a pas de mémoire de votre dernière demande. Ni ASP.NET, ni le navigateur. La seule connexion entre ces instances distinctes de la page correspond à toutes les données que vous transmettez entre elles. Si vous soumettez une page, par exemple, la nouvelle instance de page peut obtenir les données de formulaire envoyées par l’instance antérieure. (Une autre façon de passer des données entre les pages consiste à utiliser des cookies.)

Une façon formelle de décrire cette situation consiste à indiquer que les pages Web sont *sans état*. Les serveurs Web et les pages elles-mêmes et les éléments de la page ne gèrent pas les informations relatives à l’état précédent d’une page. Le Web a été conçu de cette façon, car la conservation de l’état des demandes individuelles épuiserait rapidement les ressources des serveurs Web, qui gèrent souvent des milliers, voire des centaines de milliers, de demandes par seconde.

C’est pourquoi la zone de texte était vide. Une fois la page envoyée, ASP.NET a créé une nouvelle instance de la page et exécuté le code et le balisage. Il n’y avait rien dans ce code qui indiquait à ASP.NET de placer une valeur dans la zone de texte. ASP.NET ne faisait rien, et la zone de texte a été rendue sans valeur.

Il existe en fait un moyen simple de contourner ce problème. Le genre que vous avez entré dans la zone de texte *est* disponible dans le code &mdash; il est `Request.QueryString["searchGenre"]`.

Mettez à jour le balisage de la zone de texte afin que l’attribut `value` obtient sa valeur de `searchTerm`, comme dans l’exemple suivant :

[!code-html[Main](form-basics/samples/sample9.html?highlight=1)]

Dans cette page, vous pouvez également définir l’attribut `value` sur la variable `searchTerm`, car cette variable contient également le genre que vous avez entré. Toutefois, l’utilisation de l’objet `Request` pour définir l’attribut `value` comme indiqué ici est la méthode standard pour accomplir cette tâche. (En supposant que vous souhaitez même effectuer cette &mdash; dans certains cas, vous souhaiterez peut-être afficher la page *sans* valeurs dans les champs. Tout dépend de ce qui se passe avec votre application.)

> [!NOTE]
> Vous ne pouvez pas « mémoriser » la valeur d’une zone de texte utilisée pour les mots de passe. Il s’agit d’une faille de sécurité pour permettre aux utilisateurs de remplir un champ de mot de passe à l’aide de code.

Réexécutez la page, entrez un genre, puis cliquez sur **Rechercher un genre**. Cette fois, vous voyez les résultats de la recherche, mais la zone de texte mémorise ce que vous avez entré la dernière fois :

![Page indiquant que la zone de texte a’mémorisé’l’entrée précédente](form-basics/_static/image5.png)

## <a name="searching-for-any-word-in-the-title"></a>Recherche de mots dans le titre

Vous pouvez maintenant Rechercher un genre, mais vous pouvez également rechercher un titre. Il est difficile d’obtenir un titre exactement juste lorsque vous recherchez. à la place, vous pouvez rechercher un mot qui apparaît n’importe où dans un titre. Pour ce faire, dans SQL, vous utilisez l’opérateur `LIKE` et la syntaxe comme suit :

`SELECT * FROM Movies WHERE Title LIKE '%adventure%'`

Cette commande obtient tous les films dont les titres contiennent « Adventure ». Lorsque vous utilisez l’opérateur `LIKE`, vous incluez le caractère générique `%` dans le terme de recherche. La `LIKE 'adventure%'` de recherche signifie « commençant par «Adventure »». (Techniquement, cela signifie « la chaîne «Adventure », suivie de n’importe quoi».) De même, le terme de recherche `LIKE '%adventure'` signifie « tout suivi par la chaîne «Adventure »», qui est une autre façon d’indiquer « se terminant par «Adventure »».

Le terme de recherche `LIKE '%adventure%'` signifie donc « avec «Adventure » n’importe où dans le titre.» (Techniquement, « tout ce qui se trouve dans le titre, suivi de «Adventure », suivi de n’importe quoi».)

À l’intérieur de l’élément `<form>`, ajoutez le balisage suivant sous la balise `</div>` de fermeture pour la recherche de genre (juste avant l’élément `</form>` de fermeture) :

[!code-html[Main](form-basics/samples/sample10.html)]

Le code permettant de gérer cette recherche est similaire au code de la recherche de genre, à ceci près que vous devez assembler la recherche de `LIKE`. Dans le bloc de code en haut de la page, ajoutez ce bloc `if` juste après le bloc `if` pour la recherche de genre :

[!code-csharp[Main](form-basics/samples/sample11.cs)]

Ce code utilise la même logique que celle que vous avez vue précédemment, à la différence près que la recherche utilise un opérateur `LIKE` et que le code place «`%`» avant et après le terme de recherche.

Notez que vous pouvez facilement ajouter une autre recherche à la page. Tout ce que vous avez à faire était :

- Créez un bloc `if` qui a été testé pour voir si la zone de recherche appropriée avait une valeur.
- Définissez la variable `selectCommand` sur une nouvelle instruction SQL.
- Affectez à la variable `searchTerm` la valeur à transmettre à la requête.

Voici le bloc de code complet, qui contient la nouvelle logique pour une recherche de titres :

[!code-cshtml[Main](form-basics/samples/sample12.cshtml)]

Voici un résumé de ce que fait ce code :

- Les variables `searchTerm` et `selectCommand` sont initialisées en haut. Vous allez définir ces variables sur le terme de recherche approprié (le cas échéant) et la commande SQL appropriée en fonction de ce que l’utilisateur fait dans la page. La recherche par défaut est le cas simple d’obtention de tous les films de la base de données.
- Dans les tests pour `searchGenre` et `searchTitle`, le code définit `searchTerm` à la valeur que vous souhaitez rechercher. Ces blocs de code définissent également `selectCommand` à une commande SQL appropriée pour cette recherche.
- La méthode `db.Query` est appelée une seule fois, en utilisant la commande SQL qui se trouve dans `selectedCommand` et toute valeur figurant dans `searchTerm`. S’il n’y a pas de terme de recherche (aucun genre et aucun mot de titre), la valeur de `searchTerm` est une chaîne vide. Toutefois, cela n’a pas d’importance, car dans ce cas, la requête ne nécessite pas de paramètre.

## <a name="testing-the-title-search-feature"></a>Test de la fonctionnalité de recherche de titres

Vous pouvez maintenant tester votre page de recherche terminée. Exécutez *movies. cshtml*.

Entrez un genre et cliquez sur **Rechercher un genre**. La grille affiche des films de ce genre, comme avant.

Entrez un mot de titre et cliquez sur **Rechercher un titre**. La grille affiche les films qui contiennent ce mot dans le titre.

![Liste des pages de films après la recherche de « The » dans le titre](form-basics/_static/image6.png)

Laissez les deux zones de texte vides et cliquez sur l’un des boutons. La grille affiche tous les films.

## <a name="combining-the-queries"></a>Combinaison des requêtes

Vous remarquerez peut-être que les recherches que vous pouvez effectuer sont exclusives. Vous ne pouvez pas effectuer une recherche dans le titre et le genre en même temps, même si les deux zones de recherche contiennent des valeurs. Par exemple, vous ne pouvez pas rechercher tous les films d’action dont le titre contient « Adventure ». (Lorsque la page est codée maintenant, si vous entrez des valeurs pour le genre et le titre, la recherche de titre est prioritaire.) Pour créer une recherche qui combine les conditions, vous devez créer une requête SQL dont la syntaxe est similaire à ce qui suit :

`SELECT * FROM Movies WHERE Genre = @0 AND Title LIKE @1`

Et vous devez exécuter la requête à l’aide d’une instruction telle que la suivante (à peu près) :

`var selectedData = db.Query(selectCommand, searchGenre, searchTitle);`

La création d’une logique pour permettre de nombreuses permutations de critères de recherche peut être un peu complexe, comme vous pouvez le voir. Par conséquent, nous allons nous arrêter ici.

## <a name="coming-up-next"></a>À venir suivant

Dans le didacticiel suivant, vous allez créer une page qui utilise un formulaire pour permettre aux utilisateurs d’ajouter des films à la base de données.

## <a name="complete-listing-for-movie-page-updated-with-search"></a>Liste complète des pages de films (mise à jour avec recherche)

[!code-cshtml[Main](form-basics/samples/sample13.cshtml)]

## <a name="additional-resources"></a>Ressources supplémentaires

- [Présentation de la programmation Web ASP.NET à l’aide de la syntaxe Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Clause SQL WHERE](http://www.w3schools.com/sql/sql_where.asp) sur le site W3Schools
- Article sur les [définitions de méthode](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) sur le site W3C

> [!div class="step-by-step"]
> [Précédent](displaying-data.md)
> [Suivant](entering-data.md)
