---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
title: Présentation de la pages Web ASP.NET de base de la programmation | Microsoft Docs
author: Rick-Anderson
description: 'Ce didacticiel vous donne une vue d’ensemble de la façon de programmer dans pages Web ASP.NET avec syntaxe Razor. Ce que vous allez apprendre : la syntaxe « Razor » de base que vous utilisez pour la PR...'
ms.author: riande
ms.date: 06/17/2015
ms.assetid: 7526ed45-a97d-4e8a-8301-01324ef0eff9
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
msc.type: authoredcontent
ms.openlocfilehash: 474de7671ac2931e5ba9ff635d77385403644521
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628477"
---
# <a name="introducing-aspnet-web-pages---programming-basics"></a>Présentation des notions de base de la programmation pages Web ASP.NET

par [Tom FitzMacken](https://github.com/tfitzmac)

> Ce didacticiel vous donne une vue d’ensemble de la façon de programmer dans pages Web ASP.NET avec syntaxe Razor.
> 
> Ce que vous allez apprendre :
> 
> - La syntaxe « Razor » de base que vous utilisez pour la programmation dans pages Web ASP.NET.
> - Une partie C#de base, qui est le langage de programmation que vous allez utiliser.
> - Certains concepts fondamentaux de la programmation pour les pages Web.
> - Comment installer des packages (des composants qui contiennent du code prédéfini) à utiliser avec votre site.
> - Comment utiliser les *programmes d’assistance* pour effectuer des tâches de programmation courantes.
>   
> 
> Fonctionnalités/technologies présentées :
> 
> - NuGet et le gestionnaire de package.
> - Le programme d’assistance `Gravatar`.

Ce didacticiel est principalement un exercice de présentation de la syntaxe de programmation que vous allez utiliser pour pages Web ASP.NET. Vous en apprendrez plus sur les *syntaxe Razor* et le code C# écrit dans le langage de programmation. Vous avez obtenu un aperçu de cette syntaxe dans le didacticiel précédent. dans ce didacticiel, nous expliquerons plus en détail la syntaxe.

Nous garantissons que ce didacticiel implique la plus grande programmation que vous verrez dans un seul didacticiel et qu’il s’agit du seul didacticiel qui concerne *uniquement* la programmation. Dans les autres didacticiels de cet ensemble, vous créerez en fait des pages qui sont intéressantes.

Vous en apprendrez également plus sur les *assistances*. Une application d’assistance est un composant, un morceau de code, que vous pouvez ajouter à une page. Le programme d’assistance effectue des tâches qui, dans le cas contraire, peuvent être fastidieuses ou complexes.

## <a name="creating-a-page-to-play-with-razor"></a>Création d’une page à lire avec Razor

Dans cette section, vous allez jouer un peu avec Razor afin d’avoir une idée de la syntaxe de base.

Démarrez WebMatrix s’il n’est pas déjà en cours d’exécution. Vous utiliserez le site Web que vous avez créé dans le didacticiel précédent ([prise en main avec les pages Web](https://go.microsoft.com/fwlink/?LinkId=251578)). Pour le rouvrir, cliquez sur **mes sites** , puis choisissez **WebPageMovies**:

![Écran de démarrage de WebMatrix avec les options ouvrir le site et mes sites mis en surbrillance](intro-to-web-pages-programming/_static/image1.png)

Sélectionnez l’espace de travail **fichiers** .

Dans le ruban, cliquez sur **nouveau** pour créer une page. Sélectionnez **cshtml** et nommez la nouvelle page *TestRazor. cshtml*.

Cliquez sur **OK**.

Copiez le code suivant dans le fichier, en remplaçant complètement ce qui existe déjà.

> [!NOTE]
> Lorsque vous copiez le code ou le balisage des exemples dans une page, la mise en retrait et l’alignement peuvent ne pas être les mêmes que dans le didacticiel. Toutefois, la mise en retrait et l’alignement n’affectent pas la manière dont le code s’exécute.

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample1.cshtml)]

## <a name="examining-the-example-page"></a>Examen de la page d’exemple

La plupart des éléments que vous voyez sont du code HTML ordinaire. Toutefois, en haut, il y a ce bloc de code :

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample2.cshtml)]

Notez les points suivants sur ce bloc de code :

- Le caractère @ indique à ASP.NET que ce qui suit est le code Razor, et non le format HTML. ASP.NET traite tout ce qui suit le caractère @ comme du code jusqu’à ce qu’il s’exécute à nouveau dans du code HTML. (Dans ce cas, c’est le &lt;! Élément DOCTYPE&gt;.
- Les accolades ({et}) encadrent un bloc de code Razor si le code a plus d’une ligne. Les accolades indiquent à ASP.NET où le code pour ce bloc démarre et se termine.
- Les caractères//marquent un commentaire, c’est-à-dire une partie du code qui ne s’exécute pas.
- Chaque instruction doit se terminer par un point-virgule (;). (Mais pas les commentaires.)
- Vous pouvez stocker des valeurs dans des *variables*que vous créez (*Declare*) avec le mot clé var. Lorsque vous créez une variable, vous lui donnez un nom, qui peut inclure des lettres, des chiffres et un trait de soulignement (\_). Les noms de variables ne peuvent pas commencer par un nombre et ne peuvent pas utiliser le nom d’un mot clé de programmation (comme var).
- Vous placez les chaînes de caractères (telles que « ASP.NET » et « Web pages ») entre guillemets. (Ils doivent être des guillemets doubles.) Les nombres ne sont pas entre guillemets.
- L’espace en dehors des guillemets n’a pas d’importance. Les sauts de ligne ne sont généralement pas importants ; l’exception est que vous ne pouvez pas fractionner une chaîne entre guillemets sur plusieurs lignes. La mise en retrait et l’alignement n’ont pas d’importance.

Ce qui n’est pas évident dans cet exemple est que tout le code respecte la casse. Cela signifie que la variable somme est une variable différente des variables qui peuvent être nommées somme ou somme. De même, var est un mot clé, mais var ne l’est pas.

### <a name="objects-and-properties-and-methods"></a>Objets et propriétés et méthodes

Ensuite, il y a l’expression DateTime. Now. En termes simples, DateTime est un *objet*. Un objet est un élément que vous pouvez programmer : une page, une zone de texte, un fichier, une image, une demande Web, un e-mail, un enregistrement de client, etc. Les objets ont une ou plusieurs *Propriétés* qui décrivent leurs caractéristiques. Un objet de zone de texte a une propriété de texte (entre autres), un objet de requête a une propriété URL (et d’autres), un message électronique a une propriété from et une propriété to, et ainsi de suite. Les objets ont également des *méthodes* qui sont les « verbes » qu’ils peuvent effectuer. Vous allez travailler avec des objets beaucoup.

Comme vous pouvez le voir dans l’exemple, DateTime est un objet qui vous permet de programmer des dates et des heures. Elle a une propriété nommée Now qui retourne la date et l’heure actuelles.

### <a name="using-code-to-render-markup-in-the-page"></a>Utilisation du code pour restituer le balisage dans la page

Dans le corps de la page, notez les points suivants :

[!code-html[Main](intro-to-web-pages-programming/samples/sample3.html)]

Là encore, le caractère @ indique à ASP.NET que ce qui suit est le code, et non le format HTML. Dans le balisage, vous pouvez ajouter @ suivi d’une expression de code, et ASP.NET affichera la valeur de cette expression à ce stade. Dans l’exemple, @a restitue la valeur de la variable nommée a, @product affiche tout ce qui se trouve dans la variable nommée Product, et ainsi de suite.

Toutefois, vous n’êtes pas limité aux variables. Dans quelques instances, le caractère @ précède une expression :

- @ (a\*b) restitue le produit de tout ce qui se trouve dans les variables a et b. (L’opérateur \* signifie la multiplication.)
- @ (technologie + «» + produit) restitue les valeurs dans la technologie et le produit variables après les avoir concaténées et ajouté un espace entre les deux. L’opérateur (+) pour concaténer des chaînes est le même que l’opérateur pour l’ajout de nombres. ASP.NET peut généralement savoir si vous utilisez des nombres ou des chaînes et fait la bonne chose avec l’opérateur +.
- @Request.Url restitue la propriété URL de l’objet de requête. L’objet Request contient des informations sur la requête actuelle à partir du navigateur et, bien sûr, la propriété URL contient l’URL de cette requête actuelle.

L’exemple est également conçu pour vous montrer que vous pouvez effectuer des tâches de différentes façons. Vous pouvez effectuer des calculs dans le bloc de code en haut, placer les résultats dans une variable, puis restituer la variable dans le balisage. Ou vous pouvez effectuer des calculs dans une expression directement dans le balisage. L’approche que vous utilisez dépend de ce que vous faites et, dans une certaine mesure, de votre propre préférence.

### <a name="seeing-the-code-in-action"></a>Affichage du code en action

Cliquez avec le bouton droit sur le nom du fichier, puis choisissez **lancer dans le navigateur**. La page s’affiche dans le navigateur avec toutes les valeurs et expressions résolues dans la page.

![Page « TestRazor » en cours d’exécution dans le navigateur](intro-to-web-pages-programming/_static/image2.png)

Examinez la source dans le navigateur.

![Source de la page « test Razor » dans le navigateur](intro-to-web-pages-programming/_static/image3.png)

Comme prévu dans le didacticiel précédent, aucun code Razor n’est présent dans la page. Tout ce que vous voyez est les valeurs d’affichage réelles. Lorsque vous exécutez une page, vous effectuez en fait une requête au serveur Web qui est intégré à WebMatrix. Lorsque la demande est reçue, ASP.NET résout toutes les valeurs et expressions et restitue leurs valeurs dans la page. Il envoie ensuite la page au navigateur.

> [!TIP] 
> 
> **Razor etC#**
> 
> Jusqu’à présent, nous avons dit que vous travaillez avec syntaxe Razor. C’est vrai, mais ce n’est pas tout. Le langage de programmation que vous utilisez est appelé *C#* . C#a été créé par Microsoft il y a plus de dix ans et est devenu l’un des principaux langages de programmation pour la création d’applications Windows. Toutes les règles que vous avez vu sur la manière de nommer une variable et sur la manière de créer des instructions, etc., C# sont en fait toutes des règles du langage.
> 
> Razor fait plus spécifiquement référence au petit ensemble de conventions pour la façon dont vous incorporez ce code dans une page. Par exemple, la Convention de l’utilisation de @ pour marquer le code dans la page et l’utilisation de @ {} pour incorporer un bloc de code est l’aspect Razor d’une page. Les applications auxiliaires sont également considérées comme faisant partie de Razor. Syntaxe Razor est utilisé à d’autres endroits que dans pages Web ASP.NET. (Par exemple, il est également utilisé dans les vues MVC ASP.NET.)
> 
> Nous mentionnons cela, car si vous recherchez des informations sur la programmation pages Web ASP.NET, vous trouverez de nombreuses références à Razor. Toutefois, un grand nombre de ces références ne s’appliquent pas à ce que vous faites et peuvent donc prêter à confusion. Et en fait, la plupart de vos questions en matière de programmation vont vraiment utiliser C# ou travailler avec ASP.net. Par conséquent, si vous recherchez des informations spécifiques sur Razor, vous ne trouverez peut-être pas les réponses dont vous avez besoin.

## <a name="adding-some-conditional-logic"></a>Ajout d’une logique conditionnelle

L’une des fonctionnalités intéressantes de l’utilisation de code dans une page est que vous pouvez modifier ce qui se passe en fonction de différentes conditions. Dans cette partie du didacticiel, vous allez vous familiariser avec certaines méthodes pour modifier ce qui est affiché dans la page.

L’exemple sera simple et quelque peu fictif pour que nous puissions nous concentrer sur la logique conditionnelle. La page que vous allez créer effectuera cette opération :

- Affichez un texte différent sur la page selon qu’il s’agit de la première fois que la page est affichée ou si vous avez cliqué sur un bouton pour envoyer la page. Il s’agit du premier test conditionnel.
- Affichez le message uniquement si une certaine valeur est transmise dans la chaîne de requête de l’URL (http://... ? Show = true). Il s’agit du deuxième test conditionnel.

Dans WebMatrix, créez une page et nommez-la *TestRazorPart2. cshtml*. (Dans le ruban, cliquez sur **nouveau**, choisissez **cshtml**, nommez le fichier, puis cliquez sur **OK**.)

Remplacez le contenu de cette page par ce qui suit :

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample4.cshtml)]

Le bloc de code en haut Initialise une variable nommée message avec du texte. Dans le corps de la page, le contenu de la variable de message s’affiche à l’intérieur d’un élément &lt;p&gt;. Le balisage contient également un élément de&gt; d’entrée &lt;pour créer un bouton **Envoyer** .

Exécutez la page pour voir comment elle fonctionne maintenant. Pour le moment, il s’agit essentiellement d’une page statique, même si vous cliquez sur le bouton **Envoyer** .

Revenez à WebMatrix. Dans le bloc de code, ajoutez le code en surbrillance suivant *après* la ligne qui initialise le message :

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample5.cshtml?highlight=4-6)]

### <a name="the-if---block"></a>Bloc If {}

Ce que vous venez d’ajouter était une condition if. Dans le code, la condition If a une structure semblable à celle-ci :

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample6.cs)]

La condition à tester est entre parenthèses. Il doit s’agir d’une valeur ou d’une expression qui retourne true ou false. Si la condition est true, ASP.NET exécute l’instruction ou les instructions qui se trouvent à l’intérieur des accolades. (Il s’agit de la partie *Then* de la logique *si-Then* .) Si la condition est false, le bloc de code est ignoré.

Voici quelques exemples de conditions que vous pouvez tester dans une instruction if :

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample7.cs)]

Vous pouvez tester des variables par rapport à des valeurs ou par rapport à des expressions à l’aide d’un opérateur *logique* ou d’un *opérateur de comparaison*: égal à (= =), supérieur à (&gt;), inférieur à (&lt;), supérieur ou égal à (&gt;=) et inférieur ou égal à (&lt;=). L’opérateur ! = signifie que n’est pas égal à (par exemple, si (a ! = 0) signifie *si n’est pas égal à 0*.

> [!NOTE]
> Veillez à noter que l’opérateur de comparaison pour égal à (= =) n’est pas le même que =. L’opérateur = est utilisé uniquement pour assigner des valeurs (var a = 2). Si vous combinez ces opérateurs, vous obtenez une erreur ou vous obtenez des résultats étranges.

Pour déterminer si un événement est vrai, la syntaxe complète est si (IsDone = = true). Toutefois, vous pouvez également utiliser le raccourci if (IsDone). S’il n’existe aucun opérateur de comparaison, ASP.NET suppose que vous testez la valeur true.

L'opérateur « ! » l’opérateur seul signifie qu’il ne s’agit pas d’un NOT logique. Par exemple, la condition si ( ! IsPost) signifie *si IsPost n’a pas la valeur true*.

Vous pouvez combiner des conditions à l’aide d’un opérateur AND logique (&amp;&amp; opérateur) ou d’un opérateur OR logique (| | opérateur). Par exemple, la dernière des conditions if dans les exemples précédents signifie *si FileProcessingIsDone n’a pas la valeur true et que l’exemple de jeu d’aide est défini sur false*.

### <a name="the-else-block"></a>Le bloc Else

Une dernière chose à propos des blocs if : un bloc If peut être suivi d’un bloc Else. Un bloc Else est utile si vous devez exécuter un code différent lorsque la condition est false. Voici un exemple simple :

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample8.cs)]

Vous verrez quelques exemples dans les didacticiels ultérieurs de cette série, où l’utilisation d’un bloc Else est utile.

### <a name="testing-whether-the-request-is-a-submit-post"></a>Test de l’envoi de la demande (publication)

Il y a d’autres, mais revenons à l’exemple, qui a la condition if (IsPost) {...}. IsPost est en fait une propriété de la page actuelle. La première fois que la page est demandée, IsPost retourne false. Toutefois, si vous cliquez sur un bouton ou que vous envoyez la page, autrement dit, vous la publiez, IsPost retourne la valeur true. IsPost vous permet de déterminer si vous êtes confronté à une soumission de formulaire. (En termes de verbes HTTP, si la requête est une opération d’extraction, IsPost retourne false. Si la demande est une opération de publication, IsPost retourne true.) Dans un didacticiel ultérieur, vous travaillerez avec des formulaires d’entrée, où ce test devient particulièrement utile.

Exécutez la page. Comme c’est la première fois que vous demandez la page, vous voyez « c’est la première fois que vous avez demandé la page ». Cette chaîne correspond à la valeur à laquelle vous avez initialisé la variable de message. Il y a un test if (IsPost), mais qui retourne false à ce moment-là, le code à l’intérieur du bloc If ne s’exécute pas.

Cliquez sur le bouton **Envoyer** . La page est de nouveau demandée. Comme précédemment, la variable de message est définie sur « c’est la première fois... ». Mais cette fois-ci, le test si (IsPost) retourne la valeur true, le code à l’intérieur du bloc If s’exécute. Le code remplace la valeur de la variable de message par une valeur différente, ce qui est rendu dans le balisage.

Ajoutez maintenant une condition if dans le balisage. Sous l’élément &lt;p&gt; qui contient le bouton **Envoyer** , ajoutez le balisage suivant :

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample9.cshtml)]

Vous ajoutez du code à l’intérieur du balisage, vous devez donc commencer par @. Il y a ensuite un test IF similaire à celui que vous avez ajouté précédemment dans le bloc de code. Toutefois, à l’intérieur des accolades, vous ajoutez du code HTML ordinaire (au moins, il est ordinaire jusqu’à ce qu’il soit @DateTime.Now. Il s’agit d’un autre petit morceau de code Razor. vous devez donc encore ajouter @ devant.

Le point ici est que vous pouvez ajouter des conditions if dans le bloc de code en haut et dans le balisage. Si vous utilisez une condition if dans le corps de la page, les lignes à l’intérieur du bloc peuvent être des balises ou du code. Dans ce cas, et comme c’est vrai chaque fois que vous mélangez le balisage et le code, vous devez utiliser @ pour le rendre plus clair dans ASP.NET où le code est.

Exécutez la page et cliquez sur **Envoyer**. Cette fois, vous voyez un message différent lorsque vous envoyez (« vous avez envoyé... »), mais vous voyez un nouveau message qui indique la date et l’heure.

![Page « test Razor 2 » en cours d’exécution dans le navigateur avec horodateur indiquant après envoi](intro-to-web-pages-programming/_static/image4.png)

### <a name="testing-the-value-of-a-query-string"></a>Test de la valeur d’une chaîne de requête

Un test supplémentaire. Cette fois-ci, vous allez ajouter un bloc If qui teste une valeur nommée Show qui peut être transmise dans la chaîne de requête. (Comme ceci : `http://localhost:43097/TestRazorPart2.cshtml?show=true`) Vous allez modifier la page de sorte que le message que vous avez affiché (« c’est la première fois... », etc.) s’affiche uniquement si la valeur de Show est true.

En bas (mais à l’intérieur) du bloc de code en haut de la page, ajoutez ce qui suit :

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample10.cs)]

Le bloc de code complet ressemble maintenant à l’exemple suivant. (N’oubliez pas que lorsque vous copiez le code dans votre page, la mise en retrait peut paraître différente. Mais cela n’affecte pas la manière dont le code s’exécute.)

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample11.cshtml)]

Le nouveau code dans le bloc Initialise une variable nommée showMessage à la valeur false. Il effectue ensuite un test si pour rechercher une valeur dans la chaîne de requête. Lorsque vous demandez la page pour la première fois, elle contient une URL telle que celle-ci :

`http://localhost:43097/TestRazorPart2.cshtml`

Le code détermine si l’URL contient une variable nommée Show dans la chaîne de requête, comme cette version de l’URL :

`http://localhost:43097/TestRazorPart2.cshtml`? Show = true

Le test lui-même examine la propriété QueryString de l’objet Request. Si la chaîne de requête contient un élément nommé Show, et si cet élément a la valeur true, le bloc If s’exécute et définit la variable showMessage sur true.

Il y a une astuce ici, comme vous pouvez le voir. Comme le nom indique, la chaîne de requête est une chaîne. Toutefois, vous pouvez uniquement tester la valeur true et false si la valeur que vous testez est une valeur booléenne (true/false). Avant de pouvoir tester la valeur de la variable Show dans la chaîne de requête, vous devez la convertir en une valeur booléenne. C’est ce que fait la méthode AsBool, elle prend une chaîne comme entrée et la convertit en valeur booléenne. En clair, si la chaîne est « true », la méthode AsBool convertit cette valeur en true. Si la valeur de la chaîne est autre, AsBool retourne false.

> [!TIP] 
> 
> **Types de données et méthodes ()**
> 
> Nous n’avons dit que jusqu’à présent que lorsque vous créez une variable, vous utilisez le mot clé var. Mais ce n’est pas tout. Pour pouvoir manipuler des valeurs (pour ajouter des nombres ou concaténer des chaînes, ou pour comparer des dates, ou pour tester la C# valeur true/false), doit utiliser une représentation interne appropriée de la valeur. C#peut *généralement* déterminer ce que doit être la représentation (c’est-à-dire le *type* des données) en fonction de ce que vous faites avec les valeurs. À présent, il n’est pas possible de le faire. Si ce n’est pas le cas, vous devez vous aider C# en indiquant explicitement Comment doit représenter les données. La méthode AsBool indique C# qu’une valeur de chaîne « true » ou « false » doit être traitée comme une valeur booléenne. Des méthodes similaires existent pour représenter des chaînes comme d’autres types, comme AsInt (Treat comme un entier), AsDateTime (Treat comme date/heure), AsFloat (Treat comme nombre à virgule flottante), et ainsi de suite. Quand vous les utilisez en tant que méthodes () C# , si ne peut pas représenter la valeur de chaîne comme demandé, une erreur s’affiche.

Dans le balisage de la page, supprimez ou Commentez cet élément (ici, il est présenté en commentaire) :

[!code-html[Main](intro-to-web-pages-programming/samples/sample12.html)]

Juste là où vous avez supprimé ou commenté ce texte, ajoutez ce qui suit :

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample13.cshtml)]

Le test IF indique que si la variable showMessage a la valeur true, le rendu d’un élément &lt;p&gt; avec la valeur de la variable message.

### <a name="summary-of-your-conditional-logic"></a>Résumé de votre logique conditionnelle

Si vous n’êtes pas entièrement sûr de ce que vous venez de faire, voici un résumé.

- La variable de message est initialisée avec une chaîne par défaut (« c’est la première fois... »).
- Si la demande de page est le résultat d’un envoi (publication), la valeur du message est remplacée par « maintenant vous avez envoyé... »
- La variable showMessage est initialisée avec la valeur false.
- Si la chaîne de requête contient ? Show = true, la variable showMessage a la valeur true.
- Dans le balisage, si showMessage a la valeur true, un élément &lt;p&gt; est rendu et affiche la valeur du message. (Si showMessage a la valeur false, rien n’est rendu à ce stade dans le balisage.)
- Dans le balisage, si la requête est une publication, un élément &lt;p&gt; est affiché, qui affiche la date et l’heure.

Exécutez la page. Il n’y a aucun message, car showMessage a la valeur false, donc dans le balisage, le test if (showMessage) retourne la valeur false.

Cliquez sur **Envoyer**. Vous voyez la date et l’heure, mais vous n’avez toujours pas de message.

Dans votre navigateur, accédez à la zone URL et ajoutez le code suivant à la fin de l’URL :? Show = true, puis appuyez sur entrée.

![Page « test Razor 2 » dans le navigateur avec la chaîne de requête](intro-to-web-pages-programming/_static/image5.png)

La page s’affiche à nouveau. (Étant donné que vous avez modifié l’URL, il s’agit d’une nouvelle demande, et non d’une soumission.) Cliquez à nouveau sur **Envoyer** . Le message s’affiche à nouveau, comme c’est le cas de la date et de l’heure.

![Page « test Razor 2 » après envoi lorsqu’il existe une chaîne de requête](intro-to-web-pages-programming/_static/image6.png)

Dans l’URL, remplacez ? Show = true par ? Show = false et appuyez sur entrée. Soumettez à nouveau la page. La page revient à la façon dont vous avez démarré, aucun message.

Comme indiqué précédemment, la logique de cet exemple est un petit fictif. Toutefois, si doit apparaître dans un grand nombre de vos pages, cela prendra un ou plusieurs des formulaires que vous avez vus ici.

## <a name="installing-a-helper-displaying-a-gravatar-image"></a>Installation d’un programme d’assistance (affichage d’une image Gravatar)

Certaines tâches que les utilisateurs souhaitent souvent effectuer sur des pages Web nécessitent beaucoup de code ou nécessitent des connaissances supplémentaires. Exemples : affichage d’un graphique pour les données ; Ajout d’un bouton « like » Facebook sur une page ; envoi de courrier électronique à partir de votre site Web ; rognage ou redimensionnement d’images ; utilisation de PayPal pour votre site. Pour faciliter ces types d’opérations, pages Web ASP.NET vous permet d’utiliser des *applications auxiliaires*. Les programmes d’assistance sont des composants que vous installez pour un site et qui vous permettent d’effectuer des tâches courantes à l’aide de quelques lignes de code Razor.

Pages Web ASP.NET dispose de quelques applications d’assistance intégrées. Toutefois, de nombreuses applications d’assistance sont disponibles dans les packages (compléments) fournis à l’aide du gestionnaire de package NuGet. NuGet vous permet de sélectionner un package à installer, puis il prend en charge tous les détails de l’installation.

Dans cette partie du didacticiel, vous allez installer une application d’assistance qui vous permet d’afficher une image gravatar (« avatar internationalement reconnu »). Vous en apprendrez deux choses. La première consiste à rechercher et à installer une application d’assistance. Vous allez également découvrir comment une application d’assistance vous permet de faire quelque chose que vous auriez dû faire en utilisant beaucoup de code que vous auriez à écrire vous-même.

Vous pouvez inscrire votre propre Gravatar sur le site Web Gravatar au [http://www.gravatar.com/](http://www.gravatar.com/), mais il n’est pas essentiel de créer un compte gravatar pour effectuer cette partie du didacticiel.

Dans WebMatrix, cliquez sur le bouton **NuGet** .

![Boîte de dialogue galerie NuGet dans WebMatrix](intro-to-web-pages-programming/_static/image7.png)

Le gestionnaire de package NuGet est lancé et les packages disponibles s’affichent. (Tous les packages ne sont pas des programmes d’assistance ; certains ajoutent des fonctionnalités à WebMatrix, d’autres modèles, etc.). Vous pouvez recevoir un message d’erreur relatif à l’incompatibilité de version. Vous pouvez ignorer ce message d’erreur en cliquant sur **OK** et en poursuivant ce didacticiel.

![Boîte de dialogue galerie NuGet dans WebMatrix](intro-to-web-pages-programming/_static/image8.png)

Dans la zone de recherche, entrez « asp.net helpers ». NuGet affiche les packages qui correspondent aux termes de recherche.

![Galerie NuGet dans WebMatrix avec des packages](intro-to-web-pages-programming/_static/image9.png)

La bibliothèque d’applications auxiliaires Web ASP.NET contient le code permettant de simplifier de nombreuses tâches courantes, notamment l’utilisation d’images Gravatar. Sélectionnez le package de la **bibliothèque d’assistances Web ASP.net** , puis cliquez sur **installer** pour lancer le programme d’installation. Sélectionnez **Oui** lorsque vous êtes invité à installer le package et acceptez les termes pour terminer l’installation.

C'est tout ! NuGet télécharge et installe tout, y compris tous les composants supplémentaires éventuellement requis (*dépendances*).

Si, pour une raison quelconque, vous devez désinstaller une application d’assistance, le processus est très similaire. Cliquez sur le bouton **NuGet** , cliquez sur l’onglet **installé** , puis sélectionnez le package que vous souhaitez désinstaller.

## <a name="using-a-helper-in-a-page"></a>Utilisation d’un programme d’assistance dans une page

À présent, vous allez utiliser l’application auxiliaire que vous venez d’installer. Le processus d’ajout d’un programme d’assistance à une page est similaire pour la plupart des applications auxiliaires.

Dans WebMatrix, créez une page et nommez-la *GravatarTest. cshml*. (Vous créez une page spéciale pour tester l’application d’assistance, mais vous pouvez utiliser les applications d’assistance dans n’importe quelle page de votre site.)

À l’intérieur de l’élément &lt;Body&gt;, ajoutez un élément &lt;div&gt;. À l’intérieur de l’élément de&gt; &lt;div, tapez ce qui suit :

@Gravatar.

Le caractère @ est le même que celui que vous avez utilisé pour marquer le code Razor. **Gravatar** est l’objet d’assistance que vous utilisez.

Dès que vous tapez le point (.), WebMatrix affiche la liste des *méthodes* (fonctions) que le programme d’assistance Gravatar met à disposition :

![Liste déroulante IntelliSense d’assistance Gravatar](intro-to-web-pages-programming/_static/image10.png)

Cette fonctionnalité est appelée *IntelliSense*. Il vous aide à coder en fournissant des choix appropriés au contexte. IntelliSense fonctionne avec du code HTML, CSS, ASP.NET, JavaScript et d’autres langages pris en charge dans WebMatrix. Il s’agit d’une autre fonctionnalité qui facilite le développement de pages Web dans WebMatrix.

Appuyez sur G sur le clavier. vous constatez que IntelliSense trouve la méthode GetHtml. Appuyez sur la touche Tab. IntelliSense insère la méthode sélectionnée (GetHtml) pour vous. Tapez une parenthèse ouvrante et notez que la parenthèse fermante est automatiquement ajoutée. Tapez votre adresse de messagerie entre guillemets doubles entre parenthèses. Si vous avez un compte gravatar, votre image de profil est retournée. Si vous n’avez pas de compte gravatar, une image par défaut est retournée. Lorsque vous avez terminé, la ligne ressemble à ceci :

[!code-css[Main](intro-to-web-pages-programming/samples/sample14.css)]

À présent, affichez la page dans un navigateur. L’image ou l’image par défaut s’affiche, selon que vous disposez d’un compte gravatar.

![Gravatar](intro-to-web-pages-programming/_static/image11.png) ![image par défaut](intro-to-web-pages-programming/_static/image12.png)

Pour avoir une idée de ce que l’application auxiliaire fait pour vous, affichez la source de la page dans le navigateur. Avec le code HTML que vous aviez dans votre page, vous voyez un élément image qui comprend un identificateur. Il s’agit du code que le programme d’assistance a rendu dans la page à l’endroit où vous aviez @Gravatar.GetHtml. Le programme d’assistance a pris les informations que vous avez fournies et a généré le code qui communique directement avec Gravatar afin de récupérer l’image correcte pour le compte fourni.

La méthode GetHtml vous permet également de personnaliser l’image en fournissant d’autres paramètres. Le code suivant montre comment demander une image dont la largeur et la hauteur sont de 40 pixels et qui utilise une image par défaut spécifiée nommée **Wavatar** si le compte spécifié n’existe pas.

[!code-javascript[Main](intro-to-web-pages-programming/samples/sample15.js)]

Ce code produit un résultat semblable au suivant (l’image par défaut varie aléatoirement).

![](intro-to-web-pages-programming/_static/image13.png)

## <a name="coming-up-next"></a>À venir suivant

Pour que ce didacticiel reste concis, nous devons nous concentrer sur seulement quelques notions de base. Naturellement, il y en a *beaucoup* plus pour le C#rasoir et. Vous en apprendrez davantage au fur et à mesure de ces didacticiels. Si vous souhaitez en savoir plus sur les aspects de programmation de Razor et C# pour l’instant, vous pouvez lire une présentation plus complète ici : [Présentation de la programmation Web ASP.net à l’aide de la syntaxe Razor](https://go.microsoft.com/fwlink/?LinkID=202890).

Le didacticiel suivant vous présente l’utilisation d’une base de données. Dans ce didacticiel, vous allez commencer à créer l’exemple d’application qui vous permet de répertorier vos films préférés.

## <a name="complete-listing-for-testrazor-page"></a>Liste complète de la page TestRazor

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample16.cshtml)]

## <a name="complete-listing-for-testrazorpart2-page"></a>Liste complète de la page TestRazorPart2

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample17.cshtml)]

## <a name="complete-listing-for-gravatartest-page"></a>Liste complète de la page GravatarTest

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample18.cshtml)]

## <a name="additional-resources"></a>Ressources supplémentaires

- [Présentation de la programmation Web ASP.NET à l’aide de la syntaxe Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Assistance Twitter](../../ui-layouts-and-themes/twitter-helper.md)

> [!div class="step-by-step"]
> [Précédent](getting-started.md)
> [Suivant](displaying-data.md)
