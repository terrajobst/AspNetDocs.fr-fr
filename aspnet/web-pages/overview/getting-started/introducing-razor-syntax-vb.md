---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: Présentation de la programmation Web ASP.NET à l’aide de la syntaxe Razor (Visual Basic) | Microsoft Docs
author: Rick-Anderson
description: Cette annexe vous donne une vue d’ensemble de la programmation avec les pages Web ASP.NET dans Visual Basic, à l’aide de l’syntaxe Razor.
ms.author: riande
ms.date: 02/07/2014
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: 2be57655b8c9b76b94e1d9a7ae5fbee27545a0a9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78526592"
---
# <a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a>Introduction to ASP.NET Web Programming Using the Razor Syntax (Visual Basic) (Présentation de la programmation Web ASP.NET à l'aide de la syntaxe Razor (Visual Basic)).

par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article vous donne une vue d’ensemble de la programmation avec pages Web ASP.NET à l’aide du syntaxe Razor et de Visual Basic. ASP.NET est la technologie de Microsoft pour l’exécution de pages Web dynamiques sur des serveurs Web.
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

La plupart des exemples d’utilisation de pages Web ASP.NET C#avec syntaxe Razor utilisent. Mais le syntaxe Razor prend également en charge les Visual Basic. Pour programmer une page Web ASP.NET dans Visual Basic, vous créez une page Web avec une extension de nom de fichier *. vbhtml* , puis vous ajoutez Visual Basic code. Cet article vous donne une vue d’ensemble de l’utilisation du langage et de la syntaxe Visual Basic pour créer des pages Web ASP.NET.

> [!NOTE]
> Les modèles de site Web par défaut pour Microsoft WebMatrix (**boulangerie**, **Galerie Photo**et **site de démarrage**, etc.) C# sont disponibles dans et les versions Visual Basic. Vous pouvez installer les modèles Visual Basic en tant que packages NuGet. Les modèles de site Web sont installés dans le dossier racine de votre site, dans un dossier nommé *Microsoft templates*.

## <a name="the-top-8-programming-tips"></a>Les 8 meilleurs conseils de programmation

Cette section répertorie quelques conseils que vous devez absolument savoir lorsque vous commencez à écrire du code serveur ASP.NET à l’aide de l’syntaxe Razor.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. vous ajoutez du code à une page à l’aide du caractère @

Le caractère `@` démarre les expressions Inline, les blocs à instruction unique et les blocs à instructions multiples :

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

Résultat affiché dans un navigateur :

![Rasoir-Img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> **Encodage HTML**
> 
> Lorsque vous affichez le contenu d’une page à l’aide du caractère `@`, comme dans les exemples précédents, ASP.NET code HTML de la sortie. Cela remplace les caractères HTML réservés (tels que `<` et `>` et `&`) par des codes qui permettent d’afficher les caractères sous forme de caractères dans une page Web au lieu d’être interprétés comme des balises ou des entités HTML. Sans l’encodage HTML, la sortie de votre code serveur peut ne pas s’afficher correctement et exposer une page à des risques de sécurité.
> 
> Si votre objectif est de générer un balisage HTML qui rend les balises sous forme de balisage (par exemple `<p></p>` pour un paragraphe ou `<em></em>` pour mettre en évidence du texte), consultez la section [combinaison de texte, de balises et de code dans les blocs de code](#BM_CombiningTextMarkupAndCode) plus loin dans cet article.
> 
> Pour plus d’informations sur l’encodage HTML, consultez [utilisation des formulaires HTML dans les Sites pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=202892).

### <a name="2-you-enclose-code-blocks-with-codeend-code"></a>2. Placez les blocs de code avec le code... Code de fin

Un bloc de code comprend une ou plusieurs instructions de code et est inclus avec les mots clés `Code` et `End Code`. Placez le mot clé `Code` d’ouverture immédiatement après le &#8212; `@` caractère où il ne peut pas y avoir d’espace blanc.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

Résultat affiché dans un navigateur :

![Razor-Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a>3. à l’intérieur d’un bloc, vous terminez chaque instruction de code par un saut de ligne

Dans un bloc de code Visual Basic, chaque instruction se termine par un saut de ligne. (Plus loin dans cet article, vous verrez un moyen d’encapsuler une longue instruction de code sur plusieurs lignes, si nécessaire.)

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a>4. vous utilisez des variables pour stocker des valeurs

Vous pouvez stocker des valeurs dans une *variable*, y compris des chaînes, des nombres et des dates, etc. Vous créez une variable à l’aide du mot clé `Dim`. Vous pouvez insérer des valeurs de variable directement dans une page à l’aide de `@`.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

Résultat affiché dans un navigateur :

![Rasoir-Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. vous placez les valeurs de chaîne littérale entre des guillemets doubles

Une *chaîne* est une séquence de caractères qui sont traités comme du texte. Pour spécifier une chaîne, vous devez la placer entre guillemets doubles :

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

Pour incorporer des guillemets doubles dans une valeur de chaîne, insérez deux guillemets doubles. Si vous souhaitez que le caractère de guillemet double apparaisse une fois dans la sortie de la page, entrez-le en tant que `""` dans la chaîne entre guillemets, et si vous souhaitez qu’il apparaisse deux fois, entrez-le comme `""""` dans la chaîne entre guillemets.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

Résultat affiché dans un navigateur :

![Rasoir-Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a>6. Visual Basic code ne respecte pas la casse

Le langage de Visual Basic ne respecte pas la casse. Les mots clés de programmation (comme `Dim`, `If`et `True`) et les noms de variables (comme `myString`ou `subTotal`) peuvent être écrits dans tous les cas.

Les lignes de code suivantes affectent une valeur à la variable `lastname` à l’aide d’un nom en minuscules, puis génèrent la valeur de la variable sur la page à l’aide d’un nom en majuscules.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

Résultat affiché dans un navigateur :

![VB-syntaxe-5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a>7. la majeure partie de votre code implique l’utilisation d’objets

Un objet représente un élément que vous pouvez programmer à &#8212; l’aide d’une page, d’une zone de texte, d’un fichier, d’une image, d’une requête Web, d’un message électronique, d’un enregistrement de client (ligne de base de données), etc. Les objets ont des propriétés qui décrivent leurs caractéristiques &#8212; . un objet de zone de texte a une propriété `Text`, un objet Request a une propriété `Url`, un message électronique a une propriété `From` et un objet Customer a une propriété `FirstName`. Les objets ont également des méthodes qui sont les verbes de &quot;&quot; ils peuvent effectuer. Les exemples incluent la méthode `Save` d’un objet fichier, la méthode `Rotate` d’un objet image et la méthode `Send` d’un objet email.

Vous travaillez souvent avec l’objet `Request`, qui donne des informations telles que les valeurs des champs de formulaire sur la page (zones de texte, etc.), le type de navigateur qui a effectué la demande, l’URL de la page, l’identité de l’utilisateur, etc. Cet exemple montre comment accéder aux propriétés de l’objet `Request` et comment appeler la méthode `MapPath` de l’objet `Request`, qui vous donne le chemin d’accès absolu de la page sur le serveur :

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

Résultat affiché dans un navigateur :

![Rasoir-Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. vous pouvez écrire du code qui prend des décisions

Une fonctionnalité clé des pages Web dynamiques est que vous pouvez déterminer ce que vous devez faire en fonction des conditions. La méthode la plus courante consiste à utiliser l’instruction `If` (et l’instruction facultative `Else`).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

L’instruction `If IsPost` est un moyen simple d’écrire des `If IsPost = True`. Outre les instructions `If`, il existe différentes façons de tester des conditions, de répéter des blocs de code, etc., qui sont décrits plus loin dans cet article.

Résultat affiché dans un navigateur (après avoir cliqué sur **Envoyer**) :

![Rasoir-Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> **Méthodes HTTP d’extraction et de publication et propriété IsPost**
> 
> Le protocole utilisé pour les pages Web (HTTP) prend en charge un nombre très limité de méthodes (&quot;verbes&quot;) utilisées pour effectuer des requêtes sur le serveur. Les deux types les plus courants sont obtenir, qui est utilisé pour lire une page, et la publication, qui est utilisée pour envoyer une page. En général, la première fois qu’un utilisateur demande une page, la page est demandée à l’aide de la requête obtenir. Si l’utilisateur remplit un formulaire, puis clique sur **Envoyer**, le navigateur envoie une demande de publication au serveur.
> 
> Dans la programmation Web, il est souvent utile de savoir si une page est demandée en tant que « obtenir » ou « publication », afin que vous sachiez comment traiter la page. Dans pages Web ASP.NET, vous pouvez utiliser la propriété `IsPost` pour déterminer si une requête est une requête d’extraction ou une publication. Si la demande est une publication, la propriété `IsPost` retourne la valeur true et vous pouvez effectuer des opérations telles que lire les valeurs des zones de texte sur un formulaire. De nombreux exemples vous montrent comment traiter la page différemment en fonction de la valeur de `IsPost`.

## <a name="a-simple-code-example"></a>Exemple de code simple

Cette procédure vous montre comment créer une page qui illustre des techniques de programmation de base. Dans l’exemple, vous créez une page qui permet aux utilisateurs d’entrer deux nombres, puis de les ajouter et d’afficher le résultat.

1. Dans votre éditeur, créez un nouveau fichier et nommez-le *AddNumbers. vbhtml*.
2. Copiez le code et le balisage suivants dans la page, en remplaçant tout ce qui se trouve déjà dans la page.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    Voici quelques points à noter :

    - Le caractère `@` démarre le premier bloc de code dans la page et précède la variable `totalMessage` incorporée dans la partie inférieure.
    - Le bloc en haut de la page est placé dans `Code...End Code`.
    - Les variables `total`, `num1`, `num2`et `totalMessage` stockent plusieurs nombres et une chaîne.
    - La valeur de chaîne littérale assignée à la variable `totalMessage` est entre guillemets doubles.
    - Étant donné que Visual Basic code ne respecte pas la casse, lorsque la variable `totalMessage` est utilisée vers le bas de la page, son nom ne doit correspondre qu’à l’orthographe de la déclaration de variable en haut de la page. La casse n’a pas d’importance.
    - L’expression `num1.AsInt()` + `num2.AsInt()` montre comment utiliser des objets et des méthodes. La méthode `AsInt` sur chaque variable convertit la chaîne entrée par un utilisateur en un nombre entier (entier) qui peut être ajouté.
    - La balise `<form>` comprend un attribut `method="post"`. Cela spécifie que lorsque l’utilisateur clique sur **Ajouter**, la page est envoyée au serveur à l’aide de la méthode http. Lorsque la page est envoyée, le code `If IsPost` prend la valeur true et le code conditionnel s’exécute et affiche le résultat de l’ajout des nombres.
3. Enregistrez la page et exécutez-la dans un navigateur. (Assurez-vous que la page est sélectionnée dans l’espace de travail **fichiers** avant de l’exécuter.) Entrez deux nombres entiers, puis cliquez sur le bouton **Ajouter** .

    ![Rasoir-Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a>Langage et syntaxe de Visual Basic

Vous avez vu précédemment un exemple de base de la création d’une page Web ASP.NET et la façon dont vous pouvez ajouter du code serveur au balisage HTML. Vous découvrirez ici les principes fondamentaux de l’utilisation de Visual Basic pour écrire du code serveur &#8212; ASP.net à l’aide de la syntaxe Razor, c’est-à-dire des règles du langage de programmation.

Si vous êtes familiarisé avec la programmation (surtout si vous avez utilisé C C++, C#,, Visual Basic ou JavaScript), la plupart des éléments que vous avez lus ici vous seront familiers. Vous devrez probablement vous familiariser avec la façon dont le code WebMatrix est ajouté au balisage dans les fichiers *. vbhtml* .

### <a id="BM_CombiningTextMarkupAndCode"></a>Combinaison de texte, de balisage et de code dans des blocs de code

Dans les blocs de code serveur, vous souhaiterez souvent sortir le texte et le balisage de la page. Si un bloc de code serveur contient du texte qui n’est pas du code et qu’il doit à la place être rendu tel quel, ASP.NET doit être en mesure de distinguer ce texte du code. Plusieurs méthodes sont possibles.

- Placez le texte dans un élément de bloc HTML comme `<p></p>` ou `<em></em>`:

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    L’élément HTML peut inclure du texte, des éléments HTML supplémentaires et des expressions de code de serveur. Quand ASP.NET voit la balise HTML d’ouverture (par exemple, `<p>`), il restitue tout l’élément et son contenu tel quel dans le navigateur (et résout les expressions de code serveur).

- Utilisez l’opérateur `@:` ou l’élément `<text>`. Le `@:` génère une seule ligne de contenu contenant du texte brut ou des balises HTML sans correspondance ; l’élément `<text>` encadre plusieurs lignes à la sortie. Ces options sont utiles lorsque vous ne souhaitez pas restituer un élément HTML dans le cadre de la sortie.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    L’exemple suivant répète l’exemple précédent, mais utilise une seule paire de balises `<text>` pour encadrer le texte à afficher.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    Dans l’exemple suivant, les balises `<text>` et `</text>` encadrent trois lignes, qui ont toutes un texte sans relation et des balises HTML non appariées (`<br />`), ainsi que du code serveur et des balises HTML correspondantes. Là encore, vous pouvez également faire précéder chaque ligne d’un opérateur `@:` ; les deux méthodes fonctionnent.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > Lorsque vous affichez le texte comme indiqué dans &#8212; cette section à l’aide d’un élément HTML, l’opérateur `@:` &#8212; ou l’élément `<text>` ASP.net n’encode pas la sortie au format html. (Comme indiqué précédemment, ASP.NET encode la sortie des expressions de code serveur et des blocs de code serveur précédés de `@`, sauf dans les cas particuliers indiqués dans cette section.)

### <a name="whitespace"></a>Whitespace

Les espaces supplémentaires dans une instruction (et en dehors d’un littéral de chaîne) n’affectent pas l’instruction :

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a>Fractionnement des instructions longues en plusieurs lignes

Vous pouvez scinder une longue instruction de code en plusieurs lignes à l’aide du caractère de soulignement `_` (qui, dans Visual Basic, est appelé *caractère de continuation*) après chaque ligne de code. Pour arrêter une instruction sur la ligne suivante, à la fin de la ligne, ajoutez un espace, puis le caractère de continuation. Poursuivez l’instruction sur la ligne suivante. Vous pouvez encapsuler des instructions sur autant de lignes que nécessaire pour améliorer la lisibilité. Les instructions suivantes sont les mêmes :

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

Toutefois, vous ne pouvez pas encapsuler une ligne au milieu d’un littéral de chaîne. L’exemple suivant ne fonctionne pas :

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

Pour combiner une longue chaîne encapsulée sur plusieurs lignes comme le code ci-dessus, vous devez utiliser l' *opérateur de concaténation* (`&`), que vous verrez plus loin dans cet article.

### <a name="code-comments"></a>Commentaires de code

Les commentaires vous permettent de laisser des notes pour vous-même ou d’autres. Syntaxe Razor commentaires sont précédés de `@*` et se terminent par `*@`.

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

Dans les blocs de code, vous pouvez utiliser les commentaires syntaxe Razor, ou vous pouvez utiliser le caractère de commentaire Visual Basic ordinaire, qui est un guillemet simple (`'`) préfixé pour chaque ligne.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a>Variables

Une variable est un objet nommé que vous utilisez pour stocker des données. Vous pouvez nommer toutes les variables, mais le nom doit commencer par un caractère alphabétique et ne peut pas contenir d’espaces blancs ou de caractères réservés. Dans Visual Basic, comme vous l’avez vu précédemment, la casse des lettres d’un nom de variable n’a pas d’importance.

### <a name="variables-and-data-types"></a>Variables et types de données

Une variable peut avoir un type de données spécifique, qui indique le type de données qui est stocké dans la variable. Vous pouvez avoir des variables de chaîne qui stockent des valeurs de chaîne (comme &quot;Hello World&quot;), des variables de type entier qui stockent des valeurs de nombres entiers (comme 3 ou 79) et des variables de date qui stockent des valeurs de date dans divers formats (comme 4/12/2012 ou 2009). Et il existe de nombreux autres types de données que vous pouvez utiliser.

Toutefois, vous n’êtes pas obligé de spécifier un type pour une variable. Dans la plupart des cas, ASP.NET peut déterminer le type en fonction de la façon dont les données de la variable sont utilisées. (Parfois, vous devez spécifier un type ; vous verrez des exemples où cela est vrai.)

Pour déclarer une variable sans spécifier de type, utilisez `Dim` plus le nom de la variable (par exemple, `Dim myVar`). Pour déclarer une variable avec un type, utilisez `Dim` plus le nom de la variable, suivi de `As` puis du nom du type (par exemple, `Dim myVar As String`).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

L’exemple suivant montre des expressions inline qui utilisent les variables dans une page Web.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

Résultat affiché dans un navigateur :

![Rasoir-Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>Conversion et test des types de données

Bien que ASP.NET puisse généralement déterminer automatiquement un type de données, il est parfois impossible de le faire. Par conséquent, vous devrez peut-être aider ASP.NET en effectuant une conversion explicite. Même si vous n’avez pas besoin de convertir les types, il est parfois utile de tester le type de données que vous pouvez utiliser.

Le cas le plus courant est que vous devez convertir une chaîne en un autre type, tel qu’un entier ou une date. L’exemple suivant montre un cas typique dans lequel vous devez convertir une chaîne en nombre.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

En règle générale, l’entrée utilisateur vous est fournie sous forme de chaînes. Même si vous avez demandé à l’utilisateur d’entrer un nombre et, même s’il a entré un chiffre, lorsque l’entrée de l’utilisateur est envoyée et que vous la lisez dans le code, les données sont au format de chaîne. Par conséquent, vous devez convertir la chaîne en nombre. Dans l’exemple, si vous essayez d’effectuer des opérations arithmétiques sur les valeurs sans les convertir, l’erreur suivante se produit, car ASP.NET ne peut pas ajouter deux chaînes :

`Cannot implicitly convert type 'string' to 'int'.`

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
        Convertit une chaîne qui représente un nombre entier (comme &quot;593&quot;) en un entier.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)]
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
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)]
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
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)]
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
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)]
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
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)]
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
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)]
    :::column-end:::
:::row-end:::

## <a name="operators"></a>Opérateurs

Un opérateur est un mot clé ou un caractère qui indique à ASP.NET le type de commande à effectuer dans une expression. Visual Basic prend en charge de nombreux opérateurs, mais vous ne devez en reconnaître que quelques-uns pour commencer à développer des pages Web ASP.NET. Le tableau suivant récapitule les opérateurs les plus courants.

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
        `+ - * /`
    :::column-end:::
    :::column:::
        Opérateurs mathématiques utilisés dans les expressions numériques.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
        Assignation et égalité. Selon le contexte, affecte la valeur à droite d’une instruction à l’objet situé à gauche, ou vérifie si les valeurs sont égales.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `<>`
    :::column-end:::
    :::column:::
        Inégalité Retourne `True` si les valeurs ne sont pas égales.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
        Inférieur à, supérieur à, inférieur ou égal à et supérieur ou égal à.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `&`
    :::column-end:::
    :::column:::
        Concaténation, qui est utilisée pour joindre des chaînes.
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+= -=`
    :::column-end:::
    :::column:::
        Les opérateurs d’incrémentation et de décrémentation, qui ajoutent et soustraient 1 (respectivement) d’une variable.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)]
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
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
        Parenthèses. Utilisé pour regrouper des expressions, pour passer des paramètres à des méthodes et pour accéder aux membres de tableaux et de collections.
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `Not`
    :::column-end:::
    :::column:::
        Pas. Inverse la valeur true et vice versa. Généralement utilisé comme un moyen simple de tester `False` (autrement dit, pour ne pas `True`).
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AndAlso OrElse`
    :::column-end:::
    :::column:::
        AND logique and ou, qui sont utilisés pour lier des conditions ensemble.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]
    :::column-end:::
:::row-end:::

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

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>Référencement de la racine virtuelle : l’opérateur ~ et la méthode href

Dans un fichier *. cshtml* ou *. vbhtml* , vous pouvez référencer le chemin d’accès de la racine virtuelle à l’aide de l’opérateur `~`. C’est très pratique, car vous pouvez déplacer des pages dans un site et tous les liens qu’ils contiennent vers d’autres pages ne sont pas rompus. C’est également pratique si vous déplacez votre site Web vers un autre emplacement. Voici quelques exemples :

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

Si le site Web est `http://myserver/myapp`, voici comment ASP.NET traite ces chemins lors de l’exécution de la page :

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(Vous ne verrez pas ces chemins comme valeurs de la variable, mais ASP.NET traitera les chemins comme s’ils étaient.)

Vous pouvez utiliser l’opérateur `~` dans le code serveur (comme ci-dessus) et dans le balisage, comme suit :

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

Dans le balisage, vous utilisez l’opérateur `~` pour créer des chemins d’accès à des ressources telles que des fichiers image, d’autres pages Web et des fichiers CSS. Lorsque la page s’exécute, ASP.NET parcourt la page (code et balisage) et résout toutes les références `~` au chemin d’accès approprié.

## <a name="conditional-logic-and-loops"></a>Logique conditionnelle et boucles

ASP.NET Server Code vous permet d’effectuer des tâches en fonction de conditions et d’écrire du code qui répète des instructions un nombre spécifique de fois, le code qui exécute une boucle).

### <a name="testing-conditions"></a>Conditions de test

Pour tester une condition simple, vous utilisez l’instruction `If...Then`, qui retourne `True` ou `False` en fonction d’un test que vous spécifiez :

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

Le mot clé `If` démarre un bloc. Le test réel (condition) suit le mot clé `If` et retourne true ou false. L’instruction `If` se termine par `Then`. Les instructions qui s’exécutent si le test a la valeur true sont encadrées par `If` et `End If`. Une instruction `If` peut inclure un bloc `Else` qui spécifie les instructions à exécuter si la condition a la valeur false :

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

Si une instruction `If` démarre un bloc de code, vous n’êtes pas obligé d’utiliser les instructions `Code...End Code` normales pour inclure les blocs. Vous pouvez simplement ajouter `@` au bloc et cela fonctionnera. Cette approche fonctionne avec `If` ainsi que d’autres mots clés de programmation Visual Basic qui sont suivis de blocs de code, notamment `For`, `For Each`, `Do While`, etc.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

Vous pouvez ajouter plusieurs conditions à l’aide d’un ou de plusieurs blocs `ElseIf` :

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

Dans cet exemple, si la première condition du bloc `If` n’est pas vraie, la condition `ElseIf` est vérifiée. Si cette condition est remplie, les instructions du bloc `ElseIf` sont exécutées. Si aucune des conditions n’est remplie, les instructions du bloc `Else` sont exécutées. Vous pouvez ajouter un nombre quelconque de blocs `ElseIf`, puis fermer avec un bloc `Else` en tant que &quot;tout le reste&quot; condition.

Pour tester un grand nombre de conditions, utilisez un bloc `Select Case` :

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

La valeur à tester est entre parenthèses (dans l’exemple, la variable Weekday). Chaque test individuel utilise une instruction `Case` qui répertorie une valeur. Si la valeur d’une instruction `Case` correspond à la valeur de test, le code de ce `Case` bloc est exécuté.

Résultat des deux derniers blocs conditionnels affichés dans un navigateur :

![Rasoir-Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a>Code en boucle

Vous devez souvent exécuter les mêmes instructions à plusieurs reprises. Pour ce faire, vous devez effectuer une boucle. Par exemple, vous exécutez souvent les mêmes instructions pour chaque élément d’une collection de données. Si vous savez exactement combien de fois vous souhaitez effectuer une boucle, vous pouvez utiliser une boucle `For`. Ce type de boucle est particulièrement utile pour le comptage ou le comptage :

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

La boucle commence par le mot clé `For`, suivi de trois éléments :

- Immédiatement après l’instruction `For`, vous déclarez une variable de compteur (vous n’êtes pas obligé d’utiliser `Dim`), puis vous indiquez la plage, comme dans `i = 10 to 20`. Cela signifie que la variable `i` commence à compter 10 et continue jusqu’à ce qu’elle atteigne 20 (inclusive).
- Entre les instructions `For` et `Next` correspond au contenu du bloc. Il peut contenir une ou plusieurs instructions de code qui s’exécutent avec chaque boucle.
- L’instruction `Next i` termine la boucle. Elle incrémente le compteur et démarre l’itération suivante de la boucle.

La ligne de code entre les lignes `For` et `Next` contient le code qui s’exécute pour chaque itération de la boucle. Le balisage crée chaque fois un nouveau paragraphe (`<p>` élément) et ajoute une ligne à la sortie, en affichant la valeur i (le compteur). Lorsque vous exécutez cette page, l’exemple crée 11 lignes affichant la sortie, avec le texte de chaque ligne indiquant le numéro de l’élément.

![Razor-Img11](introducing-razor-syntax-vb/_static/image11.jpg)

Si vous utilisez une collection ou un tableau, vous utilisez souvent une boucle `For Each`. Une collection est un groupe d’objets similaires, et la boucle `For Each` vous permet d’effectuer une tâche sur chaque élément de la collection. Ce type de boucle est pratique pour les collections, car contrairement à une boucle `For`, il n’est pas nécessaire d’incrémenter le compteur ou de définir une limite. Au lieu de cela, le `For Each` code de boucle passe simplement par la collection jusqu’à ce qu’il soit terminé.

Cet exemple retourne les éléments de la collection `Request.ServerVariables` (qui contient des informations sur votre serveur Web). Elle utilise une boucle `For Each` pour afficher le nom de chaque élément en créant un nouvel élément `<li>` dans une liste à puces HTML.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

Le mot clé `For Each` est suivi d’une variable qui représente un élément unique dans la collection (dans l’exemple, `myItem`), suivi du mot clé `In`, suivi de la collection dans laquelle vous souhaitez effectuer une boucle. Dans le corps de la boucle `For Each`, vous pouvez accéder à l’élément actuel à l’aide de la variable que vous avez déclarée précédemment.

![Rasoir-Img12](introducing-razor-syntax-vb/_static/image12.jpg)

Pour créer une boucle plus générale, utilisez l’instruction `Do While` :

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

Cette boucle commence par le mot clé `Do While`, suivi d’une condition, suivi du bloc à répéter. En général, les boucles incrémentent (ajoutent à) ou décrémente (soustrait de) une variable ou un objet utilisé pour le comptage. Dans l’exemple, l’opérateur `+=` ajoute 1 à la valeur d’une variable chaque fois que la boucle s’exécute. (Pour décrémenter une variable dans une boucle qui est décomptée, vous devez utiliser l’opérateur de décrémentation `-=`.)

## <a name="objects-and-collections"></a>Objets et collections

Presque tout ce qui se trouve dans un site Web ASP.NET est un objet, y compris la page Web elle-même. Cette section décrit certains objets importants que vous allez utiliser fréquemment dans votre code.

### <a name="page-objects"></a>Objets page

L’objet le plus basique dans ASP.NET est la page. Vous pouvez accéder aux propriétés de l’objet page directement sans aucun objet éligible. Le code suivant obtient le chemin d’accès au fichier de la page, à l’aide de l’objet `Request` de la page :

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

Vous pouvez utiliser les propriétés de l’objet `Page` pour obtenir un grand nombre d’informations, telles que :

- `Request`. Comme vous l’avez déjà vu, il s’agit d’une collection d’informations sur la requête actuelle, notamment le type de navigateur qui a effectué la demande, l’URL de la page, l’identité de l’utilisateur, etc.
- `Response`. Il s’agit d’une collection d’informations sur la réponse (page) qui seront envoyées au navigateur une fois l’exécution du code serveur terminée. Par exemple, vous pouvez utiliser cette propriété pour écrire des informations dans la réponse.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a>Objets de collection (tableaux et dictionnaires)

Une collection est un groupe d’objets du même type, par exemple une collection d’objets `Customer` à partir d’une base de données. ASP.NET contient de nombreuses collections intégrées, telles que la collection `Request.Files`.

Vous utiliserez souvent des données dans des collections. Deux types de collections courants sont le *tableau* et le *dictionnaire*. Un tableau est utile lorsque vous souhaitez stocker une collection d’éléments similaires, mais que vous ne voulez pas créer une variable distincte pour contenir chaque élément :

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

Avec les tableaux, vous déclarez un type de données spécifique, tel que `String`, `Integer`ou `DateTime`. Pour indiquer que la variable peut contenir un tableau, vous ajoutez des parenthèses au nom de la variable dans la déclaration (par exemple `Dim myVar() As String`). Vous pouvez accéder aux éléments d’un tableau à l’aide de leur position (index) ou à l’aide de l’instruction `For Each`. Les index de tableau sont de base &#8212; zéro, c’est-à-dire que le premier élément est à la position 0, le deuxième à la position 1, et ainsi de suite.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

Vous pouvez déterminer le nombre d’éléments d’un tableau en obtenant sa propriété `Length`. Pour obtenir la position d’un élément spécifique dans le tableau (autrement dit, pour effectuer une recherche dans le tableau), utilisez la méthode `Array.IndexOf`. Vous pouvez également effectuer des opérations telles que inverser le contenu d’un tableau (méthode `Array.Reverse`) ou trier le contenu (méthode `Array.Sort`).

Sortie du code du tableau de chaînes affiché dans un navigateur :

![Rasoir-Img13](introducing-razor-syntax-vb/_static/image13.jpg)

Un dictionnaire est une collection de paires clé/valeur, dans laquelle vous fournissez la clé (ou le nom) pour définir ou récupérer la valeur correspondante :

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

Pour créer un dictionnaire, vous utilisez le mot clé `New` pour indiquer que vous créez un nouvel objet `Dictionary`. Vous pouvez assigner un dictionnaire à une variable à l’aide du mot clé `Dim`. Vous indiquez les types de données des éléments du dictionnaire à l’aide de parenthèses (`( )`). À la fin de la déclaration, vous devez ajouter une autre paire de parenthèses, car il s’agit en fait d’une méthode qui crée un nouveau dictionnaire.

Pour ajouter des éléments au dictionnaire, vous pouvez appeler la méthode `Add` de la variable dictionnaire (`myScores` dans ce cas), puis spécifier une clé et une valeur. Vous pouvez également utiliser des parenthèses pour indiquer la clé et effectuer une assignation simple, comme dans l’exemple suivant :

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

Pour obtenir une valeur à partir du dictionnaire, vous spécifiez la clé entre parenthèses :

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a>Appel de méthodes avec des paramètres

Comme vous l’avez vu précédemment dans cet article, les objets que vous programmez ont des méthodes. Par exemple, un objet `Database` peut avoir une méthode `Database.Connect`. De nombreuses méthodes possèdent également un ou plusieurs paramètres. Un *paramètre* est une valeur que vous transmettez à une méthode pour permettre à la méthode d’effectuer sa tâche. Par exemple, examinez une déclaration pour la méthode `Request.MapPath`, qui accepte trois paramètres :

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

Cette méthode retourne le chemin d’accès physique sur le serveur qui correspond à un chemin d’accès virtuel spécifié. Les trois paramètres de la méthode sont `virtualPath`, `baseVirtualDir`et `allowCrossAppMapping`. (Notez que dans la déclaration, les paramètres sont répertoriés avec les types de données des données qu’ils accepteront.) Lorsque vous appelez cette méthode, vous devez fournir des valeurs pour les trois paramètres.

Lorsque vous utilisez Visual Basic avec le syntaxe Razor, vous avez deux options pour passer des paramètres à une méthode : les *paramètres positionnels* ou les *paramètres nommés*. Pour appeler une méthode à l’aide de paramètres positionnels, vous devez passer les paramètres dans un ordre strict spécifié dans la déclaration de méthode. (Vous connaissez généralement cet ordre en lisant la documentation de la méthode.) Vous devez suivre l’ordre et vous ne pouvez pas ignorer les paramètres &#8212; si nécessaire, vous transmettez une chaîne vide (`""`) ou null pour un paramètre positionnel dont vous n’avez pas de valeur.

L’exemple suivant suppose que vous avez un dossier nommé *scripts* sur votre site Web. Le code appelle la méthode `Request.MapPath` et passe des valeurs pour les trois paramètres dans le bon ordre. Il affiche ensuite le chemin d’accès mappé résultant.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

Lorsqu’il existe de nombreux paramètres pour une méthode, vous pouvez maintenir votre code plus propre et plus lisible en utilisant des paramètres nommés. Pour appeler une méthode à l’aide de paramètres nommés, spécifiez le nom du paramètre suivi de `:=`, puis fournissez la valeur. L’un des avantages des paramètres nommés est que vous pouvez les ajouter dans l’ordre de votre choix. (Un inconvénient est que l’appel de la méthode n’est pas aussi compact.)

L’exemple suivant appelle la même méthode que ci-dessus, mais utilise des paramètres nommés pour fournir les valeurs :

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

Comme vous pouvez le voir, les paramètres sont passés dans un ordre différent. Toutefois, si vous exécutez l’exemple précédent et cet exemple, ils retournent la même valeur.

## <a name="handling-errors"></a>Gestion des erreurs

### <a name="try-catch-statements"></a>Instructions Try-Catch

Dans votre code, vous aurez souvent des instructions qui peuvent échouer pour des raisons extérieures à votre contrôle. Exemple :

- Si votre code tente d’ouvrir, de créer, de lire ou d’écrire un fichier, toutes sortes d’erreurs peuvent se produire. Le fichier que vous souhaitez peut-être n’existe pas, il est peut-être verrouillé, le code n’a peut-être pas d’autorisations, et ainsi de suite.
- De même, si votre code tente de mettre à jour des enregistrements dans une base de données, il peut y avoir des problèmes d’autorisations, la connexion à la base de données peut être supprimée, les données à enregistrer peuvent ne pas être valides, etc.

En termes de programmation, ces situations sont appelées *exceptions*. Si votre code rencontre une exception, il génère (lève) un message d’erreur qui est, au mieux, ennuyeux pour les utilisateurs.

![Razor-Img14](introducing-razor-syntax-vb/_static/image14.jpg)

Dans les cas où votre code risque de rencontrer des exceptions et afin d’éviter les messages d’erreur de ce type, vous pouvez utiliser des instructions `Try/Catch`. Dans l’instruction `Try`, vous exécutez le code que vous êtes en train de vérifier. Dans une ou plusieurs instructions `Catch`, vous pouvez rechercher des erreurs spécifiques (types spécifiques d’exceptions) qui ont pu se produire. Vous pouvez inclure autant d’instructions `Catch` que vous le souhaitez pour rechercher les erreurs que vous prévoyez d’anticiper.

> [!NOTE]
> Nous vous recommandons d’éviter d’utiliser la méthode `Response.Redirect` dans les instructions `Try/Catch`, car cela peut provoquer une exception dans votre page.

L’exemple suivant montre une page qui crée un fichier texte lors de la première demande, puis affiche un bouton qui permet à l’utilisateur d’ouvrir le fichier. L’exemple utilise délibérément un nom de fichier incorrect afin qu’il génère une exception. Le code comprend des instructions `Catch` pour deux exceptions possibles : `FileNotFoundException`, qui se produit si le nom de fichier est incorrect, et `DirectoryNotFoundException`, qui se produit si ASP.NET ne peut même pas trouver le dossier. (Vous pouvez supprimer les marques de commentaire d’une instruction dans l’exemple afin de voir comment elle s’exécute quand tout fonctionne correctement.)

Si votre code n’a pas géré l’exception, vous verrez une page d’erreur telle que la capture d’écran précédente. Toutefois, la section `Try/Catch` permet d’empêcher l’utilisateur de voir ces types d’erreurs.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a>Ressources supplémentaires

### <a name="reference-documentation"></a>Documentation de référence

- [ASP.NET](https://msdn.microsoft.com/library/ee532866.aspx)
- [Langage Visual Basic](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
