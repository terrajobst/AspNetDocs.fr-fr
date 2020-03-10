---
uid: web-pages/overview/getting-started/introducing-razor-syntax-c
title: Présentation de la programmation Web ASP.NET à l’aide deC#la syntaxe Razor () | Microsoft Docs
author: Rick-Anderson
description: Ce chapitre vous donne une vue d’ensemble de la programmation avec pages Web ASP.NET à l’aide de l’syntaxe Razor. ASP.NET est la technologie de Microsoft pour l’exécution d’un service Web dynamique...
ms.author: riande
ms.date: 02/07/2014
ms.assetid: aa67d304-583b-4bf8-a231-195656cfb587
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-c
msc.type: authoredcontent
ms.openlocfilehash: c2f420bb7c2f7d2e31654c20fb9ec7497a30a9f7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78641574"
---
# <a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a>Présentation de la programmation Web ASP.NET à l’aide deC#la syntaxe Razor ()

par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article vous donne une vue d’ensemble de la programmation avec pages Web ASP.NET à l’aide de l’syntaxe Razor. ASP.NET est la technologie de Microsoft pour l’exécution de pages Web dynamiques sur des serveurs Web. Cet article se concentre sur C# l’utilisation du langage de programmation.
> 
> **Ce que vous allez apprendre**:
> 
> - Les 8 principales astuces de programmation pour la prise en main de la programmation pages Web ASP.NET à l’aide de syntaxe Razor.
> - Concepts de programmation de base dont vous aurez besoin.
> - Ce que ASP.NET le code serveur et le syntaxe Razor.
>   
> 
> ## <a name="software-versions"></a>Versions de logiciels
> 
> 
> - Pages Web ASP.NET (Razor) 3
>   
> 
> Ce didacticiel fonctionne également avec pages Web ASP.NET 2.

## <a name="the-top-8-programming-tips"></a>Les 8 meilleurs conseils de programmation

Cette section répertorie quelques conseils que vous devez absolument savoir lorsque vous commencez à écrire du code serveur ASP.NET à l’aide de l’syntaxe Razor.

> [!NOTE]
> Le syntaxe Razor est basé sur le C# langage de programmation et c’est le langage le plus souvent utilisé avec pages Web ASP.net. Toutefois, le syntaxe Razor prend également en charge le langage de Visual Basic, et tout ce que vous voyez est également possible dans Visual Basic. Pour plus d’informations, consultez l’annexe [Visual Basic langage et la syntaxe](https://go.microsoft.com/fwlink/?LinkId=202908).

Vous trouverez plus d’informations sur la plupart de ces techniques de programmation plus loin dans cet article.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. vous ajoutez du code à une page à l’aide du caractère @

Le caractère `@` démarre les expressions Inline, les blocs d’instructions uniques et les blocs à instructions multiples :

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

Voici à quoi ressemblent ces instructions lorsque la page s’exécute dans un navigateur :

![Rasoir-Img1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> **Encodage HTML**
> 
> Lorsque vous affichez le contenu d’une page à l’aide du caractère `@`, comme dans les exemples précédents, ASP.NET code HTML de la sortie. Cela remplace les caractères HTML réservés (tels que `<` et `>` et `&`) par des codes qui permettent d’afficher les caractères sous forme de caractères dans une page Web au lieu d’être interprétés comme des balises ou des entités HTML. Sans l’encodage HTML, la sortie de votre code serveur peut ne pas s’afficher correctement et exposer une page à des risques de sécurité.
> 
> Si votre objectif est de générer un balisage HTML qui rend les balises sous forme de balisage (par exemple `<p></p>` pour un paragraphe ou `<em></em>` pour mettre en évidence du texte), consultez la section [combinaison de texte, de balises et de code dans les blocs de code](#BM_CombiningTextMarkupAndCode) plus loin dans cet article.
> 
> Vous pouvez en savoir plus sur l’encodage HTML dans [utilisation des formulaires](https://go.microsoft.com/fwlink/?LinkId=202892).

### <a name="2-you-enclose-code-blocks-in-braces"></a>2. Placez les blocs de code entre accolades

Un *bloc de code* comprend une ou plusieurs instructions de code et est placé entre accolades.

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

Résultat affiché dans un navigateur :

![Razor-Img2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a>3. à l’intérieur d’un bloc, vous terminez chaque instruction de code avec un point-virgule

À l’intérieur d’un bloc de code, chaque instruction de code complète doit se terminer par un point-virgule. Les expressions Inline ne se terminent pas par un point-virgule.

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a>4. vous utilisez des variables pour stocker des valeurs

Vous pouvez stocker des valeurs dans une *variable*, y compris des chaînes, des nombres et des dates, etc. Vous créez une variable à l’aide du mot clé `var`. Vous pouvez insérer des valeurs de variable directement dans une page à l’aide de `@`.

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

Résultat affiché dans un navigateur :

![Rasoir-Img3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. vous placez les valeurs de chaîne littérale entre des guillemets doubles

Une *chaîne* est une séquence de caractères qui sont traités comme du texte. Pour spécifier une chaîne, vous devez la placer entre guillemets doubles :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

Si la chaîne que vous souhaitez afficher contient une barre oblique inverse (`\`) ou des guillemets doubles (`"`), utilisez un *littéral de chaîne Verbatim* préfixé avec l’opérateur `@`. (Dans C#, le caractère \ a une signification spéciale, à moins que vous n’utilisiez un littéral de chaîne textuelle.)

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

Pour incorporer des guillemets doubles, utilisez un littéral de chaîne Verbatim et répétez les guillemets :

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

Voici le résultat de l’utilisation de ces deux exemples dans une page :

![Rasoir-Img4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> Notez que le caractère `@` est utilisé à la fois pour marquer des littéraux C# de chaîne textuels dans et pour marquer le code dans les pages ASP.net.

### <a name="6-code-is-case-sensitive"></a>6. le code respecte la casse

Dans C#, les mots clés (comme `var`, `true`et `if`) et les noms de variable respectent la casse. Les lignes de code suivantes créent deux variables différentes, `lastName` et `LastName.`

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

Si vous déclarez une variable en tant que `var lastName = "Smith";` et que vous tentez de référencer cette variable dans votre page comme `@LastName`, vous obtiendriez la valeur `"Jones"` au lieu de `"Smith"`.

> [!NOTE]
> Dans Visual Basic, les mots clés et les variables ne respectent *pas* la casse.

### <a name="7-much-of-your-coding-involves-objects"></a>7. la majeure partie de votre codage implique des objets

Un *objet* représente un élément que vous pouvez programmer à &#8212; l’aide d’une page, d’une zone de texte, d’un fichier, d’une image, d’une requête Web, d’un message électronique, d’un enregistrement de client (ligne de base de données), etc. Les objets ont des propriétés qui décrivent leurs caractéristiques et que vous pouvez &#8212; lire ou modifier un objet de zone de texte a une propriété `Text` (entre autres), un objet de requête possède une propriété `Url`, un message électronique a une propriété `From` et un objet Customer possède une propriété `FirstName`. Les objets ont également des méthodes qui sont les verbes de &quot;&quot; ils peuvent effectuer. Les exemples incluent la méthode `Save` d’un objet fichier, la méthode `Rotate` d’un objet image et la méthode `Send` d’un objet email.

Vous travaillez souvent avec l’objet `Request`, qui vous donne des informations telles que les valeurs des zones de texte (champs de formulaire) sur la page, le type de navigateur qui a effectué la demande, l’URL de la page, l’identité de l’utilisateur, etc. L’exemple suivant montre comment accéder aux propriétés de l’objet `Request` et comment appeler la méthode `MapPath` de l’objet `Request`, qui vous donne le chemin d’accès absolu de la page sur le serveur :

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

Résultat affiché dans un navigateur :

![Rasoir-Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. vous pouvez écrire du code qui prend des décisions

Une fonctionnalité clé des pages Web dynamiques est que vous pouvez déterminer ce que vous devez faire en fonction des conditions. La méthode la plus courante consiste à utiliser l’instruction `if` (et l’instruction facultative `else`).

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

L’instruction `if(IsPost)` est un moyen simple d’écrire des `if(IsPost == true)`. Outre les instructions `if`, il existe différentes façons de tester des conditions, de répéter des blocs de code, etc., qui sont décrits plus loin dans cet article.

Résultat affiché dans un navigateur (après avoir cliqué sur **Envoyer**) :

![Rasoir-Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a>Méthodes HTTP d’extraction et de publication et propriété IsPost
> 
> Le protocole utilisé pour les pages Web (HTTP) prend en charge un nombre très limité de méthodes (verbes) utilisées pour effectuer des requêtes sur le serveur. Les deux types les plus courants sont obtenir, qui est utilisé pour lire une page, et la publication, qui est utilisée pour envoyer une page. En général, la première fois qu’un utilisateur demande une page, la page est demandée à l’aide de la requête obtenir. Si l’utilisateur remplit un formulaire, puis clique sur un bouton envoyer, le navigateur envoie une demande de publication au serveur.
> 
> Dans la programmation Web, il est souvent utile de savoir si une page est demandée en tant que « obtenir » ou « publication », afin que vous sachiez comment traiter la page. Dans pages Web ASP.NET, vous pouvez utiliser la propriété `IsPost` pour déterminer si une requête est une requête d’extraction ou une publication. Si la demande est une publication, la propriété `IsPost` retourne la valeur true et vous pouvez effectuer des opérations telles que lire les valeurs des zones de texte sur un formulaire. De nombreux exemples vous montrent comment traiter la page différemment en fonction de la valeur de `IsPost`.

## <a name="a-simple-code-example"></a>Exemple de code simple

Cette procédure vous montre comment créer une page qui illustre des techniques de programmation de base. Dans l’exemple, vous créez une page qui permet aux utilisateurs d’entrer deux nombres, puis de les ajouter et d’afficher le résultat.

1. Dans votre éditeur, créez un nouveau fichier et nommez-le *AddNumbers. cshtml*.
2. Copiez le code et le balisage suivants dans la page, en remplaçant tout ce qui se trouve déjà dans la page.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    Voici quelques points à noter :

    - Le caractère `@` démarre le premier bloc de code dans la page et précède la variable `totalMessage` qui est incorporée près du bas de la page.
    - Le bloc en haut de la page est placé entre accolades.
    - Dans le bloc en haut, toutes les lignes se terminent par un point-virgule.
    - Les variables `total`, `num1`, `num2`et `totalMessage` stockent plusieurs nombres et une chaîne.
    - La valeur de chaîne littérale assignée à la variable `totalMessage` est entre guillemets doubles.
    - Étant donné que le code respecte la casse, lorsque la variable `totalMessage` est utilisée vers le bas de la page, son nom doit correspondre exactement à la variable située en haut.
    - L' `num1.AsInt() + num2.AsInt()` d’expression montre comment utiliser des objets et des méthodes. La méthode `AsInt` sur chaque variable convertit la chaîne entrée par un utilisateur en nombre (un entier) afin que vous puissiez y effectuer une opération arithmétique.
    - La balise `<form>` comprend un attribut `method="post"`. Cela spécifie que lorsque l’utilisateur clique sur **Ajouter**, la page est envoyée au serveur à l’aide de la méthode http. Lorsque la page est envoyée, le test de `if(IsPost)` prend la valeur true et le code conditionnel s’exécute et affiche le résultat de l’ajout des nombres.
3. Enregistrez la page et exécutez-la dans un navigateur. (Assurez-vous que la page est sélectionnée dans l’espace de travail **fichiers** avant de l’exécuter.) Entrez deux nombres entiers, puis cliquez sur le bouton **Ajouter** . 

    ![Rasoir-Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a>Concepts de programmation de base

Cet article vous fournit une vue d’ensemble de la programmation Web ASP.NET. Il ne s’agit pas d’un examen exhaustif, juste une brève présentation des concepts de programmation que vous utiliserez le plus souvent. Même dans ce cas, il couvre presque tout ce dont vous avez besoin pour commencer à utiliser pages Web ASP.NET.

Mais tout d’abord, un peu de support technique.

### <a name="the-razor-syntax-server-code-and-aspnet"></a>Syntaxe Razor, code du serveur et ASP.NET

Syntaxe Razor est une syntaxe de programmation simple pour incorporer du code serveur dans une page Web. Dans une page Web qui utilise la syntaxe Razor, il existe deux types de contenu : le contenu du client et le code du serveur. Le contenu client est le fichier que vous utilisez dans les pages Web : balisage HTML (éléments), informations de style, telles que CSS, peut-être un script client tel que JavaScript et du texte brut.

Syntaxe Razor vous permet d’ajouter du code serveur au contenu de ce client. Si la page contient du code serveur, le serveur exécute ce code en premier, avant d’envoyer la page au navigateur. En s’exécutant sur le serveur, le code peut effectuer des tâches qui peuvent être beaucoup plus complexes en utilisant uniquement le contenu du client, comme l’accès aux bases de données basées sur le serveur. Plus important encore, le code serveur peut créer dynamiquement du &#8212; contenu client, il peut générer un balisage HTML ou tout autre contenu à la volée, puis l’envoyer au navigateur avec tout code HTML statique que la page peut contenir. Du point de vue du navigateur, le contenu client généré par votre code serveur n’est pas différent de tout autre contenu client. Comme vous l’avez déjà vu, le code serveur requis est assez simple.

Les pages Web ASP.NET qui incluent les syntaxe Razor ont une extension de fichier spéciale ( *. cshtml* ou *. vbhtml*). Le serveur reconnaît ces extensions, exécute le code marqué avec syntaxe Razor, puis envoie la page au navigateur.

### <a name="where-does-aspnet-fit-in"></a>Où ASP.NET s’intègre-t-il ?

Syntaxe Razor est basé sur une technologie de Microsoft appelée ASP.NET, qui est à son tour basée sur le Microsoft .NET Framework. The.NET Framework est une infrastructure de programmation volumineuse et complète de Microsoft pour le développement de pratiquement n’importe quel type d’application informatique. ASP.NET est la partie du .NET Framework spécifiquement conçue pour la création d’applications Web. Les développeurs ont utilisé ASP.NET pour créer un grand nombre des sites Web les plus volumineux et les plus fréquents dans le monde. (Chaque fois que vous voyez l’extension de nom de fichier *. aspx* dans le cadre de l’URL d’un site, vous savez que le site a été écrit à l’aide de ASP.net.)

Le syntaxe Razor vous donne toute la puissance de ASP.NET, mais à l’aide d’une syntaxe simplifiée qui est plus facile à apprendre si vous êtes débutant et qui vous permet de vous rendre plus productif si vous êtes un expert. Même si cette syntaxe est simple à utiliser, sa relation de famille avec ASP.NET et la .NET Framework signifie que lorsque vos sites Web sont plus sophistiqués, vous bénéficiez de la puissance des infrastructures plus grandes disponibles.

![Razor-Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> **Classes et instances**
> 
> ASP.NET Server Code utilise des objets, qui sont à leur tour basés sur l’idée des classes. La classe est la définition ou le modèle d’un objet. Par exemple, une application peut contenir une classe `Customer` qui définit les propriétés et les méthodes dont tout objet client a besoin.
> 
> Lorsque l’application doit utiliser des informations client réelles, elle crée une instance de (ou *instancie*) un objet client. Chaque client individuel est une instance distincte de la classe `Customer`. Chaque instance prend en charge les mêmes propriétés et méthodes, mais les valeurs de propriété pour chaque instance sont généralement différentes, car chaque objet Customer est unique. Dans un objet client, la propriété `LastName` peut être « Smith ». dans un autre objet client, la propriété `LastName` peut être « Jones ».
> 
> De même, toute page Web individuelle de votre site est un objet `Page` qui est une instance de la classe `Page`. Un bouton sur la page est un objet `Button` qui est une instance de la classe `Button`, et ainsi de suite. Chaque instance a ses propres caractéristiques, mais elles sont toutes basées sur ce qui est spécifié dans la définition de classe de l’objet.

## <a name="basic-syntax"></a>Syntaxe de base

Vous avez vu précédemment un exemple de base de la création d’une page de pages Web ASP.NET, ainsi que la façon dont vous pouvez ajouter du code serveur au balisage HTML. Ici, vous allez apprendre les principes fondamentaux de l’écriture du code serveur &#8212; ASP.net à l’aide de la syntaxe Razor qui correspond aux règles du langage de programmation.

Si vous êtes familiarisé avec la programmation (surtout si vous avez utilisé C C++, C#,, Visual Basic ou JavaScript), la plupart des éléments que vous avez lus ici vous seront familiers. Vous devrez probablement vous familiariser avec la façon dont le code du serveur est ajouté au balisage dans les fichiers *. cshtml* .

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a>Combinaison de texte, de balisage et de code dans des blocs de code

Dans les blocs de code serveur, vous souhaitez souvent afficher du texte ou des balises (ou les deux) sur la page. Si un bloc de code serveur contient du texte qui n’est pas du code et qu’il doit à la place être rendu tel quel, ASP.NET doit être en mesure de distinguer ce texte du code. Plusieurs méthodes sont possibles.

- Placez le texte dans un élément HTML comme `<p></p>` ou `<em></em>`:   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    L’élément HTML peut inclure du texte, des éléments HTML supplémentaires et des expressions de code de serveur. Quand ASP.NET voit la balise HTML d’ouverture (par exemple, `<p>`), elle affiche tout ce qui inclut l’élément et son contenu tel quel dans le navigateur, en résolvant les expressions de code serveur au fur et à mesure.
- Utilisez l’opérateur `@:` ou l’élément `<text>`. Le `@:` génère une seule ligne de contenu contenant du texte brut ou des balises HTML sans correspondance ; l’élément `<text>` encadre plusieurs lignes à la sortie. Ces options sont utiles lorsque vous ne souhaitez pas restituer un élément HTML dans le cadre de la sortie.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    Si vous souhaitez générer plusieurs lignes de texte ou des balises HTML sans correspondance, vous pouvez faire précéder chaque ligne d' `@:`, ou vous pouvez placer la ligne dans un élément `<text>`. À l’instar de l’opérateur `@:`, les balises`<text>` sont utilisées par ASP.NET pour identifier le contenu de texte et ne sont jamais rendues dans la sortie de page.

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    Le premier exemple répète l’exemple précédent, mais utilise une seule paire de balises `<text>` pour encadrer le texte à afficher. Dans le deuxième exemple, les balises `<text>` et `</text>` encadrent trois lignes, qui ont toutes des textes sans relation contenant-contenu et des balises HTML non appariées (`<br />`), ainsi que du code serveur et des balises HTML correspondantes. Là encore, vous pouvez également faire précéder chaque ligne d’un opérateur `@:` ; les deux méthodes fonctionnent.

    > [!NOTE]
    > Lorsque vous affichez le texte comme indiqué dans &#8212; cette section à l’aide d’un élément HTML, l’opérateur `@:` &#8212; ou l’élément `<text>` ASP.net n’encode pas la sortie au format html. (Comme indiqué précédemment, ASP.NET encode la sortie des expressions de code serveur et des blocs de code serveur précédés de `@`, sauf dans les cas particuliers indiqués dans cette section.)

### <a name="whitespace"></a>Whitespace

Les espaces supplémentaires dans une instruction (et en dehors d’un littéral de chaîne) n’affectent pas l’instruction :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

Un saut de ligne dans une instruction n’a aucun effet sur l’instruction et vous pouvez encapsuler des instructions pour améliorer la lisibilité. Les instructions suivantes sont les mêmes :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

Toutefois, vous ne pouvez pas encapsuler une ligne au milieu d’un littéral de chaîne. L’exemple suivant ne fonctionne pas :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

Pour combiner une longue chaîne encapsulée sur plusieurs lignes, comme le code ci-dessus, il existe deux options. Vous pouvez utiliser l’opérateur de concaténation (`+`), que vous verrez plus loin dans cet article. Vous pouvez également utiliser le caractère `@` pour créer un littéral de chaîne textuelle, comme vous l’avez vu précédemment dans cet article. Vous pouvez rompre les littéraux de chaîne Verbatim sur les lignes :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a>Commentaires sur le code (et le balisage)

Les commentaires vous permettent de laisser des notes pour vous-même ou d’autres. Ils vous permettent également de désactiver (*commenter*) une section de code ou de balisage que vous ne souhaitez pas exécuter, mais que vous souhaitez conserver dans votre page pour le moment.

Il existe une syntaxe de commentaire différente pour le code Razor et pour le balisage HTML. Comme avec tout le code Razor, les commentaires Razor sont traités (puis supprimés) sur le serveur avant que la page ne soit envoyée au navigateur. Par conséquent, la syntaxe de commentaire Razor vous permet de placer des commentaires dans le code (ou même dans le balisage) que vous pouvez voir lorsque vous modifiez le fichier, mais que les utilisateurs ne voient pas, même dans la source de la page.

Pour les commentaires Razor ASP.NET, vous démarrez le commentaire avec `@*` et vous le Terminez avec `*@`. Le commentaire peut être sur une ou plusieurs lignes :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

Voici un commentaire dans un bloc de code :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

Voici le même bloc de code, avec la ligne de code commentée afin qu’il ne s’exécute pas :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

À l’intérieur d’un bloc de code, comme alternative à l’utilisation de la syntaxe des commentaires Razor, vous pouvez utiliser la syntaxe de commentaire du langage de C#programmation que vous utilisez, par exemple :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

Dans C#, les commentaires sur une seule ligne sont précédés des caractères `//` et les commentaires sur plusieurs lignes commencent par `/*` et se terminent par `*/`. (Comme avec les commentaires Razor C# , les commentaires ne sont pas rendus dans le navigateur.)

Pour le balisage, comme vous le savez probablement, vous pouvez créer un commentaire HTML :

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

Les commentaires HTML commencent par `<!--` caractères et se terminent par `-->`. Vous pouvez utiliser des commentaires HTML pour entourer non seulement du texte, mais également tout balisage HTML que vous pouvez conserver dans la page, mais que vous ne souhaitez pas afficher. Ce commentaire HTML masque l’intégralité du contenu des balises et le texte qu’ils contiennent :

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

Contrairement aux commentaires Razor, les commentaires HTML *sont* rendus dans la page et l’utilisateur peut les voir en affichant la source de la page.

Razor présente des limitations sur les blocs imbriqués de C#. Pour plus d’informations [, C# consultez variables nommées et blocs imbriqués générer du code endommagé](http://aspnetwebstack.codeplex.com/workitem/1914)

## <a name="variables"></a>Variables

Une variable est un objet nommé que vous utilisez pour stocker des données. Vous pouvez nommer toutes les variables, mais le nom doit commencer par un caractère alphabétique et ne peut pas contenir d’espaces blancs ou de caractères réservés.

### <a name="variables-and-data-types"></a>Variables et types de données

Une variable peut avoir un type de données spécifique, qui indique le type de données qui est stocké dans la variable. Vous pouvez avoir des variables de chaîne qui stockent des valeurs de chaîne (comme &quot;Hello World&quot;), des variables de type entier qui stockent des valeurs de nombres entiers (comme 3 ou 79) et des variables de date qui stockent des valeurs de date dans divers formats (comme 4/12/2012 ou 2009). Et il existe de nombreux autres types de données que vous pouvez utiliser.

Toutefois, vous n’êtes généralement pas obligé de spécifier un type pour une variable. La plupart du temps, ASP.NET peut déterminer le type en fonction de la façon dont les données de la variable sont utilisées. (Parfois, vous devez spécifier un type ; vous verrez des exemples où cela est vrai.)

Vous déclarez une variable à l’aide du mot clé `var` (si vous ne souhaitez pas spécifier un type) ou en utilisant le nom du type :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

L’exemple suivant montre des utilisations typiques de variables dans une page Web :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

Si vous combinez les exemples précédents dans une page, vous voyez que cela s’affiche dans un navigateur :

![Rasoir-Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>Conversion et test des types de données

Bien que ASP.NET puisse généralement déterminer automatiquement un type de données, il est parfois impossible de le faire. Par conséquent, vous devrez peut-être aider ASP.NET en effectuant une conversion explicite. Même si vous n’avez pas besoin de convertir les types, il est parfois utile de tester le type de données que vous pouvez utiliser.

Le cas le plus courant est que vous devez convertir une chaîne en un autre type, tel qu’un entier ou une date. L’exemple suivant montre un cas typique dans lequel vous devez convertir une chaîne en nombre.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

En règle générale, l’entrée utilisateur vous est fournie sous forme de chaînes. Même si vous avez demandé aux utilisateurs d’entrer un nombre, et même s’ils ont entré un chiffre, lorsque l’entrée de l’utilisateur est envoyée et que vous la lisez dans le code, les données sont au format de chaîne. Par conséquent, vous devez convertir la chaîne en nombre. Dans l’exemple, si vous essayez d’effectuer des opérations arithmétiques sur les valeurs sans les convertir, l’erreur suivante se produit, car ASP.NET ne peut pas ajouter deux chaînes :

*Impossible de convertir implicitement le type’String’en’int'.*

Pour convertir les valeurs en entiers, vous appelez la méthode `AsInt`. Si la conversion réussit, vous pouvez ajouter les nombres.

Le tableau suivant répertorie quelques méthodes de conversion et de test courantes pour les variables.

:::row:::
    :::column:::
    <strong>Méthode</strong>
    :::column-end:::
    :::column:::
    <strong>Description</strong>
    :::column-end:::
    :::column:::
    <strong>Exemple</strong>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
    Convertit une chaîne qui représente un nombre entier (comme « 593 ») en entier.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample28.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsBool(), IsBool()`
    :::column-end:::
    :::column:::
    Convertit une chaîne comme &quot;true&quot; ou &quot;false&quot; en un type booléen.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample29.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsFloat(), IsFloat()`
    :::column-end:::
    :::column:::
    Convertit une chaîne qui a une valeur décimale comme &quot;1,3&quot; ou &quot;7,439&quot; en un nombre à virgule flottante.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample30.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsDecimal(), IsDecimal()`
    :::column-end:::
    :::column:::
    Convertit une chaîne qui a une valeur décimale comme &quot;1,3&quot; ou &quot;7,439&quot; en nombre décimal. (Dans ASP.NET, un nombre décimal est plus précis qu’un nombre à virgule flottante.)
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample31.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsDateTime(), IsDateTime()`
    :::column-end:::
    :::column:::
    Convertit une chaîne qui représente une valeur de date et d’heure dans le type de `DateTime` ASP.NET.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample32.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `ToString()`
    :::column-end:::
    :::column:::
    Convertit tout autre type de données en chaîne.
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)]
    :::column-end:::
:::row-end:::

## <a name="operators"></a>Opérateurs

Un opérateur est un mot clé ou un caractère qui indique à ASP.NET le type de commande à effectuer dans une expression. La C# langue (et le syntaxe Razor basé sur celle-ci) prend en charge de nombreux opérateurs, mais vous ne devez en reconnaître que quelques-uns pour commencer. Le tableau suivant récapitule les opérateurs les plus courants.

:::row:::
    :::column:::
    <strong>Operator</strong>
    :::column-end:::
    :::column:::
    <strong>Description</strong>
    :::column-end:::
    :::column:::
    <strong>Exemples</strong>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+` `-` `*` `/`
    :::column-end:::
    :::column:::
    Opérateurs mathématiques utilisés dans les expressions numériques.
    :::column-end:::
    :::column:::
        [!code-css[Main](introducing-razor-syntax-c/samples/sample34.css)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
    Assignation. Assigne la valeur du côté droit d’une instruction à l’objet situé à gauche.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample35.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `==`
    :::column-end:::
    :::column:::
    Égalité Retourne `true` si les valeurs sont égales. (Notez la distinction entre l’opérateur `=` et l’opérateur `==`.)
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample36.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `!=`
    :::column-end:::
    :::column:::
    Inégalité Retourne `true` si les valeurs ne sont pas égales.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample37.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
    Inférieur à, supérieur à, inférieur ou égal à, et supérieur à ou égal à égal.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample38.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+`
    :::column-end:::
    :::column:::
    Concaténation, qui est utilisée pour joindre des chaînes. ASP.NET sait la différence entre cet opérateur et l’opérateur d’addition en fonction du type de données de l’expression.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+=` `-=`
    :::column-end:::
    :::column:::
    Les opérateurs d’incrémentation et de décrémentation, qui ajoutent et soustraient 1 (respectivement) d’une variable.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample40.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `.`
    :::column-end:::
    :::column:::
    Cédé. Utilisé pour distinguer les objets et leurs propriétés et méthodes.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample41.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
    Parenthèses. Utilisé pour regrouper des expressions et passer des paramètres à des méthodes.
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample42.js)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `[]`
    :::column-end:::
    :::column:::
    Crochets. Utilisé pour accéder aux valeurs dans les tableaux ou les collections.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample43.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `!`
    :::column-end:::
    :::column:::
    Pas. Inverse une valeur `true` pour `false` et vice versa. Généralement utilisé comme un moyen simple de tester `false` (autrement dit, pour ne pas `true`).
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `&&` `||`
    :::column-end:::
    :::column:::
    AND logique and ou, qui sont utilisés pour lier des conditions ensemble.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)]
    :::column-end:::
:::row-end:::

<a id="ID_WorkingWithFileAndFolderPaths"></a>
## <a name="working-with-file-and-folder-paths-in-code"></a>Utilisation des chemins d’accès aux fichiers et aux dossiers dans le code

Vous travaillez souvent avec les chemins d’accès de fichier et de dossier dans votre code. Voici un exemple de structure de dossiers physiques pour un site Web tel qu’il peut apparaître sur votre ordinateur de développement :

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

Voici quelques détails essentiels sur les URL et les chemins d’accès :

- Une URL commence par un nom de domaine (`http://www.example.com`) ou un nom de serveur (`http://localhost`, `http://mycomputer`).
- Une URL correspond à un chemin d’accès physique sur un ordinateur hôte. Par exemple, `http://myserver` peut correspondre au dossier *C:\websites\mywebsite* sur le serveur.
- Un chemin d’accès virtuel est raccourci pour représenter les chemins d’accès dans le code sans avoir à spécifier le chemin d’accès complet. Il comprend la partie d’une URL qui suit le nom de domaine ou de serveur. Lorsque vous utilisez des chemins d’accès virtuels, vous pouvez déplacer votre code vers un autre domaine ou serveur sans avoir à mettre à jour les chemins d’accès.

Voici un exemple pour vous aider à comprendre les différences :

| URL complète | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| Nom du serveur | *mycompanyserver* |
| Chemin d'accès virtuel | */humanresources/CompanyPolicy.htm* |
| Chemin d’accès physique | *C:\mywebsites\humanresources\CompanyPolicy.htm* |

La racine virtuelle est/, tout comme la racine de votre lecteur C :. (Les chemins d’accès aux dossiers virtuels utilisent toujours des barres obliques.) Le chemin d’accès virtuel d’un dossier ne doit pas nécessairement avoir le même nom que le dossier physique ; Il peut s’agir d’un alias. (Sur les serveurs de production, le chemin d’accès virtuel correspond rarement à un chemin d’accès physique exact.)

Lorsque vous travaillez avec des fichiers et des dossiers dans le code, vous devez parfois faire référence au chemin d’accès physique et parfois à un chemin d’accès virtuel, selon les objets que vous utilisez. ASP.NET vous offre les outils nécessaires pour utiliser les chemins d’accès aux fichiers et aux dossiers dans le code : la méthode `Server.MapPath`, ainsi que l’opérateur `~` et la méthode `Href`.

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a>Conversion de chemins d’accès virtuels en éléments physiques : méthode Server. MapPath

La méthode `Server.MapPath` convertit un chemin d’accès virtuel (par exemple, */default.cshtml*) en chemin d’accès physique absolu (comme *C:\WebSites\MyWebSiteFolder\default.cshtml*). Vous utilisez cette méthode chaque fois que vous avez besoin d’un chemin d’accès physique complet. Voici un exemple typique de la lecture ou de l’écriture d’un fichier texte ou d’un fichier image sur le serveur Web.

En général, vous ne connaissez pas le chemin d’accès physique absolu de votre site sur le serveur d’un site d’hébergement. cette méthode peut donc convertir le chemin d’accès virtuel (le chemin d’accès virtuel) vers le chemin d’accès correspondant sur le serveur pour vous. Vous transmettez le chemin d’accès virtuel d’un fichier ou d’un dossier à la méthode et il retourne le chemin d’accès physique :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>Référencement de la racine virtuelle : l’opérateur ~ et la méthode href

Dans un fichier *. cshtml* ou *. vbhtml* , vous pouvez référencer le chemin d’accès de la racine virtuelle à l’aide de l’opérateur `~`. C’est très pratique, car vous pouvez déplacer des pages dans un site et tous les liens qu’ils contiennent vers d’autres pages ne sont pas rompus. C’est également pratique si vous déplacez votre site Web vers un autre emplacement. Voici quelques exemples :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

Si le site Web est `http://myserver/myapp`, voici comment ASP.NET traite ces chemins lors de l’exécution de la page :

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(Vous ne verrez pas ces chemins comme valeurs de la variable, mais ASP.NET traitera les chemins comme s’ils étaient.)

Vous pouvez utiliser l’opérateur `~` dans le code serveur (comme ci-dessus) et dans le balisage, comme suit :

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

Dans le balisage, vous utilisez l’opérateur `~` pour créer des chemins d’accès à des ressources telles que des fichiers image, d’autres pages Web et des fichiers CSS. Lorsque la page s’exécute, ASP.NET parcourt la page (code et balisage) et résout toutes les références `~` au chemin d’accès approprié.

## <a name="conditional-logic-and-loops"></a>Logique conditionnelle et boucles

ASP.NET Server Code vous permet d’effectuer des tâches en fonction de conditions et d’écrire du code qui répète des instructions un nombre spécifique de fois (autrement dit, le code qui exécute une boucle).

### <a name="testing-conditions"></a>Conditions de test

Pour tester une condition simple, vous utilisez l’instruction `if`, qui retourne true ou false en fonction d’un test que vous spécifiez :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

Le mot clé `if` démarre un bloc. Le test réel (condition) est entre parenthèses et retourne true ou false. Les instructions qui s’exécutent si le test a la valeur true sont placées entre accolades. Une instruction `if` peut inclure un bloc `else` qui spécifie les instructions à exécuter si la condition a la valeur false :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

Vous pouvez ajouter plusieurs conditions à l’aide d’un bloc de `else if` :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

Dans cet exemple, si la première condition du bloc If n’est pas true, la condition `else if` est cochée. Si cette condition est remplie, les instructions du bloc `else if` sont exécutées. Si aucune des conditions n’est remplie, les instructions du bloc `else` sont exécutées. Vous pouvez ajouter un nombre quelconque d’autres blocs if, puis fermer avec un bloc `else` comme &quot;tout le reste&quot; condition.

Pour tester un grand nombre de conditions, utilisez un bloc `switch` :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

La valeur à tester est entre parenthèses (dans l’exemple, la variable `weekday`). Chaque test individuel utilise une instruction `case` qui se termine par un signe deux-points ( :). Si la valeur d’une instruction `case` correspond à la valeur de test, le code dans ce bloc case est exécuté. Vous fermez chaque instruction case avec une instruction `break`. (Si vous oubliez d’inclure Break dans chaque bloc `case`, le code de l’instruction `case` suivante s’exécutera également.) Un bloc de `switch` a souvent une instruction `default` comme dernier cas pour une option &quot;tout le reste&quot; qui s’exécute si aucun des autres cas n’a la valeur true.

Résultat des deux derniers blocs conditionnels affichés dans un navigateur :

![Rasoir-Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a>Code en boucle

Vous devez souvent exécuter les mêmes instructions à plusieurs reprises. Pour ce faire, vous devez effectuer une boucle. Par exemple, vous exécutez souvent les mêmes instructions pour chaque élément d’une collection de données. Si vous savez exactement combien de fois vous souhaitez effectuer une boucle, vous pouvez utiliser une boucle `for`. Ce type de boucle est particulièrement utile pour le comptage ou le comptage :

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

La boucle commence par le mot clé `for`, suivi de trois instructions entre parenthèses, chacune se terminant par un point-virgule.

- À l’intérieur des parenthèses, la première instruction (`var i=10;`) crée un compteur et l’initialise à 10. Vous n’avez pas besoin de nommer le &#8212; compteur `i` vous pouvez utiliser n’importe quelle variable. Lors de l’exécution de la boucle `for`, le compteur est incrémenté automatiquement.
- La deuxième instruction (`i < 21;`) définit la condition pour le nombre de fois que vous souhaitez compter. Dans ce cas, vous souhaitez qu’il passe à un maximum de 20 (autrement dit, continuez alors que le compteur est inférieur à 21).
- La troisième instruction (`i++`) utilise un opérateur d’incrément, qui spécifie simplement que la valeur 1 a été ajoutée au compteur chaque fois que la boucle s’exécute.

À l’intérieur des accolades se trouve le code qui s’exécute pour chaque itération de la boucle. Le balisage crée chaque fois un nouveau paragraphe (`<p>` élément) et ajoute une ligne à la sortie, en affichant la valeur de `i` (le compteur). Lorsque vous exécutez cette page, l’exemple crée 11 lignes affichant la sortie, avec le texte de chaque ligne indiquant le numéro de l’élément.

![Razor-Img11](introducing-razor-syntax-c/_static/image11.jpg)

Si vous utilisez une collection ou un tableau, vous utilisez souvent une boucle `foreach`. Une collection est un groupe d’objets similaires, et la boucle `foreach` vous permet d’effectuer une tâche sur chaque élément de la collection. Ce type de boucle est pratique pour les collections, car contrairement à une boucle `for`, il n’est pas nécessaire d’incrémenter le compteur ou de définir une limite. Au lieu de cela, le `foreach` code de boucle passe simplement par la collection jusqu’à ce qu’il soit terminé.

Par exemple, le code suivant retourne les éléments de la collection `Request.ServerVariables`, qui est un objet qui contient des informations sur votre serveur Web. Elle utilise une boucle `foreac` h pour afficher le nom de chaque élément en créant un nouvel élément `<li>` dans une liste à puces HTML.

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

Le mot clé `foreach` est suivi de parenthèses où vous déclarez une variable qui représente un élément unique dans la collection (dans l’exemple, `var item`), suivi du mot clé `in`, suivi de la collection dans laquelle vous souhaitez effectuer une boucle. Dans le corps de la boucle `foreach`, vous pouvez accéder à l’élément actuel à l’aide de la variable que vous avez déclarée précédemment.

![Rasoir-Img12](introducing-razor-syntax-c/_static/image12.jpg)

Pour créer une boucle plus générale, utilisez l’instruction `while` :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

Une boucle `while` commence par le mot clé `while`, suivi de parenthèses où vous spécifiez la durée de la boucle (ici, pendant tant que `countNum` est inférieure à 50), puis le bloc à répéter. En général, les boucles incrémentent (ajoutent à) ou décrémente (soustrait de) une variable ou un objet utilisé pour le comptage. Dans l’exemple, l’opérateur `+=` ajoute 1 à `countNum` à chaque exécution de la boucle. (Pour décrémenter une variable dans une boucle qui est décomptée, vous devez utiliser l’opérateur de décrémentation `-=`).

## <a name="objects-and-collections"></a>Objets et collections

Presque tout ce qui se trouve dans un site Web ASP.NET est un objet, y compris la page Web elle-même. Cette section décrit certains objets importants que vous allez utiliser fréquemment dans votre code.

### <a name="page-objects"></a>Objets page

L’objet le plus basique dans ASP.NET est la page. Vous pouvez accéder aux propriétés de l’objet page directement sans aucun objet éligible. Le code suivant obtient le chemin d’accès au fichier de la page, à l’aide de l’objet `Request` de la page :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

Pour indiquer clairement que vous référencez des propriétés et des méthodes sur l’objet de page actuel, vous pouvez éventuellement utiliser le mot clé `this` pour représenter l’objet page dans votre code. Voici l’exemple de code précédent, avec `this` ajoutée pour représenter la page :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

Vous pouvez utiliser les propriétés de l’objet `Page` pour obtenir un grand nombre d’informations, telles que :

- `Request`. Comme vous l’avez déjà vu, il s’agit d’une collection d’informations sur la requête actuelle, notamment le type de navigateur qui a effectué la demande, l’URL de la page, l’identité de l’utilisateur, etc.
- `Response`. Il s’agit d’une collection d’informations sur la réponse (page) qui seront envoyées au navigateur une fois l’exécution du code serveur terminée. Par exemple, vous pouvez utiliser cette propriété pour écrire des informations dans la réponse. 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a>Objets de collection (tableaux et dictionnaires)

Une *collection* est un groupe d’objets du même type, par exemple une collection d’objets `Customer` à partir d’une base de données. ASP.NET contient de nombreuses collections intégrées, telles que la collection `Request.Files`.

Vous utiliserez souvent des données dans des collections. Deux types de collections courants sont le *tableau* et le *dictionnaire*. Un tableau est utile lorsque vous souhaitez stocker une collection d’éléments similaires, mais que vous ne voulez pas créer une variable distincte pour contenir chaque élément :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

Avec les tableaux, vous déclarez un type de données spécifique, tel que `string`, `int`ou `DateTime`. Pour indiquer que la variable peut contenir un tableau, vous ajoutez des crochets à la déclaration (par exemple, `string[]` ou `int[]`). Vous pouvez accéder aux éléments d’un tableau à l’aide de leur position (index) ou à l’aide de l’instruction `foreach`. Les index de tableau sont de base &#8212; zéro, c’est-à-dire que le premier élément est à la position 0, le deuxième à la position 1, et ainsi de suite.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

Vous pouvez déterminer le nombre d’éléments d’un tableau en obtenant sa propriété `Length`. Pour obtenir la position d’un élément spécifique dans le tableau (pour effectuer une recherche dans le tableau), utilisez la méthode `Array.IndexOf`. Vous pouvez également effectuer des opérations telles que inverser le contenu d’un tableau (méthode `Array.Reverse`) ou trier le contenu (méthode `Array.Sort`).

Sortie du code du tableau de chaînes affiché dans un navigateur :

![Rasoir-Img13](introducing-razor-syntax-c/_static/image13.jpg)

Un dictionnaire est une collection de paires clé/valeur, dans laquelle vous fournissez la clé (ou le nom) pour définir ou récupérer la valeur correspondante :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

Pour créer un dictionnaire, vous utilisez le mot clé `new` pour indiquer que vous créez un nouvel objet dictionnaire. Vous pouvez assigner un dictionnaire à une variable à l’aide du mot clé `var`. Vous indiquez les types de données des éléments dans le dictionnaire à l’aide de crochets pointus (`< >`). À la fin de la déclaration, vous devez ajouter une paire de parenthèses, car il s’agit en fait d’une méthode qui crée un nouveau dictionnaire.

Pour ajouter des éléments au dictionnaire, vous pouvez appeler la méthode `Add` de la variable dictionnaire (`myScores` dans ce cas), puis spécifier une clé et une valeur. Vous pouvez également utiliser des crochets pour indiquer la clé et effectuer une assignation simple, comme dans l’exemple suivant :

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

Pour obtenir une valeur à partir du dictionnaire, vous spécifiez la clé entre crochets :

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a>Appel de méthodes avec des paramètres

Comme vous l’avez lu plus haut dans cet article, les objets que vous programmez peuvent avoir des méthodes. Par exemple, un objet `Database` peut avoir une méthode `Database.Connect`. De nombreuses méthodes possèdent également un ou plusieurs paramètres. Un *paramètre* est une valeur que vous transmettez à une méthode pour permettre à la méthode d’effectuer sa tâche. Par exemple, examinez une déclaration pour la méthode `Request.MapPath`, qui accepte trois paramètres :

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

(La ligne a été encapsulée pour le rendre plus lisible. N’oubliez pas que vous pouvez placer des sauts de ligne presque n’importe quel endroit, sauf dans les chaînes placées entre guillemets.)

Cette méthode retourne le chemin d’accès physique sur le serveur qui correspond à un chemin d’accès virtuel spécifié. Les trois paramètres de la méthode sont `virtualPath`, `baseVirtualDir`et `allowCrossAppMapping`. (Notez que dans la déclaration, les paramètres sont répertoriés avec les types de données des données qu’ils accepteront.) Lorsque vous appelez cette méthode, vous devez fournir des valeurs pour les trois paramètres.

L’syntaxe Razor vous offre deux options pour passer des paramètres à une méthode : les *paramètres positionnels* et les *paramètres nommés*. Pour appeler une méthode à l’aide de paramètres positionnels, vous devez passer les paramètres dans un ordre strict spécifié dans la déclaration de méthode. (Vous connaissez généralement cet ordre en lisant la documentation de la méthode.) Vous devez suivre l’ordre et vous ne pouvez pas ignorer les paramètres &#8212; si nécessaire, vous transmettez une chaîne vide (`""`) ou `null` pour un paramètre positionnel dont vous n’avez pas de valeur.

L’exemple suivant suppose que vous avez un dossier nommé *scripts* sur votre site Web. Le code appelle la méthode `Request.MapPath` et passe des valeurs pour les trois paramètres dans le bon ordre. Il affiche ensuite le chemin d’accès mappé résultant.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

Quand une méthode a de nombreux paramètres, vous pouvez rendre votre code plus lisible en utilisant des paramètres nommés. Pour appeler une méthode à l’aide de paramètres nommés, vous spécifiez le nom du paramètre suivi d’un signe deux-points ( :), puis de la valeur. L’avantage des paramètres nommés est que vous pouvez les passer dans l’ordre de votre choix. (Un inconvénient est que l’appel de la méthode n’est pas aussi compact.)

L’exemple suivant appelle la même méthode que ci-dessus, mais utilise des paramètres nommés pour fournir les valeurs :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

Comme vous pouvez le voir, les paramètres sont passés dans un ordre différent. Toutefois, si vous exécutez l’exemple précédent et cet exemple, ils retournent la même valeur.

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a>Gestion des erreurs

### <a name="try-catch-statements"></a>Instructions Try-Catch

Dans votre code, vous aurez souvent des instructions qui peuvent échouer pour des raisons extérieures à votre contrôle. Exemple :

- Si votre code essaie de créer ou d’accéder à un fichier, toutes sortes d’erreurs peuvent se produire. Le fichier que vous souhaitez peut-être n’existe pas, il est peut-être verrouillé, le code n’a peut-être pas d’autorisations, et ainsi de suite.
- De même, si votre code tente de mettre à jour des enregistrements dans une base de données, il peut y avoir des problèmes d’autorisations, la connexion à la base de données peut être supprimée, les données à enregistrer peuvent ne pas être valides, etc.

En termes de programmation, ces situations sont appelées *exceptions*. Si votre code rencontre une exception, il génère (lève) un message d’erreur qui est, au mieux, ennuyeux pour les utilisateurs :

![Razor-Img14](introducing-razor-syntax-c/_static/image14.jpg)

Dans les cas où votre code risque de rencontrer des exceptions et afin d’éviter les messages d’erreur de ce type, vous pouvez utiliser des instructions `try/catch`. Dans l’instruction `try`, vous exécutez le code que vous êtes en train de vérifier. Dans une ou plusieurs instructions `catch`, vous pouvez rechercher des erreurs spécifiques (types spécifiques d’exceptions) qui ont pu se produire. Vous pouvez inclure autant d’instructions `catch` que vous le souhaitez pour rechercher les erreurs que vous anticipez.

> [!NOTE]
> Nous vous recommandons d’éviter d’utiliser la méthode `Response.Redirect` dans les instructions `try/catch`, car cela peut provoquer une exception dans votre page.

L’exemple suivant montre une page qui crée un fichier texte lors de la première demande, puis affiche un bouton qui permet à l’utilisateur d’ouvrir le fichier. L’exemple utilise délibérément un nom de fichier incorrect afin qu’il génère une exception. Le code comprend des instructions `catch` pour deux exceptions possibles : `FileNotFoundException`, qui se produit si le nom de fichier est incorrect, et `DirectoryNotFoundException`, qui se produit si ASP.NET ne peut même pas trouver le dossier. (Vous pouvez supprimer les marques de commentaire d’une instruction dans l’exemple afin de voir comment elle s’exécute quand tout fonctionne correctement.)

Si votre code n’a pas géré l’exception, vous verrez une page d’erreur telle que la capture d’écran précédente. Toutefois, la section `try/catch` permet d’empêcher l’utilisateur de voir ces types d’erreurs.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a>Ressources supplémentaires

**Programmation avec Visual Basic**

[Annexe : langage et syntaxe de Visual Basic](https://go.microsoft.com/fwlink/?LinkId=202908)

**Documentation de référence**

[ASP.NET](https://msdn.microsoft.com/library/ee532866.aspx)

[C#Sous](https://msdn.microsoft.com/library/kx37x362.aspx)
