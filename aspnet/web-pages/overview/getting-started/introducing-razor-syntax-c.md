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
# <a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a><span data-ttu-id="f9639-104">Présentation de la programmation Web ASP.NET à l’aide deC#la syntaxe Razor ()</span><span class="sxs-lookup"><span data-stu-id="f9639-104">Introduction to ASP.NET Web Programming Using the Razor Syntax (C#)</span></span>

<span data-ttu-id="f9639-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f9639-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="f9639-106">Cet article vous donne une vue d’ensemble de la programmation avec pages Web ASP.NET à l’aide de l’syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="f9639-106">This article gives you an overview of programming with ASP.NET Web Pages using the Razor syntax.</span></span> <span data-ttu-id="f9639-107">ASP.NET est la technologie de Microsoft pour l’exécution de pages Web dynamiques sur des serveurs Web.</span><span class="sxs-lookup"><span data-stu-id="f9639-107">ASP.NET is Microsoft's technology for running dynamic web pages on web servers.</span></span> <span data-ttu-id="f9639-108">Cet article se concentre sur C# l’utilisation du langage de programmation.</span><span class="sxs-lookup"><span data-stu-id="f9639-108">This articles focuses on using the C# programming language.</span></span>
> 
> <span data-ttu-id="f9639-109">**Ce que vous allez apprendre**:</span><span class="sxs-lookup"><span data-stu-id="f9639-109">**What you'll learn**:</span></span>
> 
> - <span data-ttu-id="f9639-110">Les 8 principales astuces de programmation pour la prise en main de la programmation pages Web ASP.NET à l’aide de syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="f9639-110">The top 8 programming tips for getting started with programming ASP.NET Web Pages using Razor syntax.</span></span>
> - <span data-ttu-id="f9639-111">Concepts de programmation de base dont vous aurez besoin.</span><span class="sxs-lookup"><span data-stu-id="f9639-111">Basic programming concepts you'll need.</span></span>
> - <span data-ttu-id="f9639-112">Ce que ASP.NET le code serveur et le syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="f9639-112">What ASP.NET server code and the Razor syntax is all about.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="f9639-113">Versions de logiciels</span><span class="sxs-lookup"><span data-stu-id="f9639-113">Software versions</span></span>
> 
> 
> - <span data-ttu-id="f9639-114">Pages Web ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="f9639-114">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="f9639-115">Ce didacticiel fonctionne également avec pages Web ASP.NET 2.</span><span class="sxs-lookup"><span data-stu-id="f9639-115">This tutorial also works with ASP.NET Web Pages 2.</span></span>

## <a name="the-top-8-programming-tips"></a><span data-ttu-id="f9639-116">Les 8 meilleurs conseils de programmation</span><span class="sxs-lookup"><span data-stu-id="f9639-116">The Top 8 Programming Tips</span></span>

<span data-ttu-id="f9639-117">Cette section répertorie quelques conseils que vous devez absolument savoir lorsque vous commencez à écrire du code serveur ASP.NET à l’aide de l’syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="f9639-117">This section lists a few tips that you absolutely need to know as you start writing ASP.NET server code using the Razor syntax.</span></span>

> [!NOTE]
> <span data-ttu-id="f9639-118">Le syntaxe Razor est basé sur le C# langage de programmation et c’est le langage le plus souvent utilisé avec pages Web ASP.net.</span><span class="sxs-lookup"><span data-stu-id="f9639-118">The Razor syntax is based on the C# programming language, and that's the language that's used most often with ASP.NET Web Pages.</span></span> <span data-ttu-id="f9639-119">Toutefois, le syntaxe Razor prend également en charge le langage de Visual Basic, et tout ce que vous voyez est également possible dans Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="f9639-119">However, the Razor syntax also supports the Visual Basic language, and everything you see you can also do in Visual Basic.</span></span> <span data-ttu-id="f9639-120">Pour plus d’informations, consultez l’annexe [Visual Basic langage et la syntaxe](https://go.microsoft.com/fwlink/?LinkId=202908).</span><span class="sxs-lookup"><span data-stu-id="f9639-120">For details, see the appendix [Visual Basic Language and Syntax](https://go.microsoft.com/fwlink/?LinkId=202908).</span></span>

<span data-ttu-id="f9639-121">Vous trouverez plus d’informations sur la plupart de ces techniques de programmation plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="f9639-121">You can find more details about most of these programming techniques later in the article.</span></span>

### <a name="1-you-add-code-to-a-page-using-the--character"></a><span data-ttu-id="f9639-122">1. vous ajoutez du code à une page à l’aide du caractère @</span><span class="sxs-lookup"><span data-stu-id="f9639-122">1. You add code to a page using the @ character</span></span>

<span data-ttu-id="f9639-123">Le caractère `@` démarre les expressions Inline, les blocs d’instructions uniques et les blocs à instructions multiples :</span><span class="sxs-lookup"><span data-stu-id="f9639-123">The `@` character starts inline expressions, single statement blocks, and multi-statement blocks:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

<span data-ttu-id="f9639-124">Voici à quoi ressemblent ces instructions lorsque la page s’exécute dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="f9639-124">This is what these statements look like when the page runs in a browser:</span></span>

![Rasoir-Img1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> <span data-ttu-id="f9639-126">**Encodage HTML**</span><span class="sxs-lookup"><span data-stu-id="f9639-126">**HTML Encoding**</span></span>
> 
> <span data-ttu-id="f9639-127">Lorsque vous affichez le contenu d’une page à l’aide du caractère `@`, comme dans les exemples précédents, ASP.NET code HTML de la sortie.</span><span class="sxs-lookup"><span data-stu-id="f9639-127">When you display content in a page using the `@` character, as in the preceding examples, ASP.NET HTML-encodes the output.</span></span> <span data-ttu-id="f9639-128">Cela remplace les caractères HTML réservés (tels que `<` et `>` et `&`) par des codes qui permettent d’afficher les caractères sous forme de caractères dans une page Web au lieu d’être interprétés comme des balises ou des entités HTML.</span><span class="sxs-lookup"><span data-stu-id="f9639-128">This replaces reserved HTML characters (such as `<` and `>` and `&`) with codes that enable the characters to be displayed as characters in a web page instead of being interpreted as HTML tags or entities.</span></span> <span data-ttu-id="f9639-129">Sans l’encodage HTML, la sortie de votre code serveur peut ne pas s’afficher correctement et exposer une page à des risques de sécurité.</span><span class="sxs-lookup"><span data-stu-id="f9639-129">Without HTML encoding, the output from your server code might not display correctly, and could expose a page to security risks.</span></span>
> 
> <span data-ttu-id="f9639-130">Si votre objectif est de générer un balisage HTML qui rend les balises sous forme de balisage (par exemple `<p></p>` pour un paragraphe ou `<em></em>` pour mettre en évidence du texte), consultez la section [combinaison de texte, de balises et de code dans les blocs de code](#BM_CombiningTextMarkupAndCode) plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="f9639-130">If your goal is to output HTML markup that renders tags as markup (for example `<p></p>` for a paragraph or `<em></em>` to emphasize text), see the section [Combining Text, Markup, and Code in Code Blocks](#BM_CombiningTextMarkupAndCode) later in this article.</span></span>
> 
> <span data-ttu-id="f9639-131">Vous pouvez en savoir plus sur l’encodage HTML dans [utilisation des formulaires](https://go.microsoft.com/fwlink/?LinkId=202892).</span><span class="sxs-lookup"><span data-stu-id="f9639-131">You can read more about HTML encoding in [Working with Forms](https://go.microsoft.com/fwlink/?LinkId=202892).</span></span>

### <a name="2-you-enclose-code-blocks-in-braces"></a><span data-ttu-id="f9639-132">2. Placez les blocs de code entre accolades</span><span class="sxs-lookup"><span data-stu-id="f9639-132">2. You enclose code blocks in braces</span></span>

<span data-ttu-id="f9639-133">Un *bloc de code* comprend une ou plusieurs instructions de code et est placé entre accolades.</span><span class="sxs-lookup"><span data-stu-id="f9639-133">A *code block* includes one or more code statements and is enclosed in braces.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

<span data-ttu-id="f9639-134">Résultat affiché dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="f9639-134">The result displayed in a browser:</span></span>

![Razor-Img2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a><span data-ttu-id="f9639-136">3. à l’intérieur d’un bloc, vous terminez chaque instruction de code avec un point-virgule</span><span class="sxs-lookup"><span data-stu-id="f9639-136">3. Inside a block, you end each code statement with a semicolon</span></span>

<span data-ttu-id="f9639-137">À l’intérieur d’un bloc de code, chaque instruction de code complète doit se terminer par un point-virgule.</span><span class="sxs-lookup"><span data-stu-id="f9639-137">Inside a code block, each complete code statement must end with a semicolon.</span></span> <span data-ttu-id="f9639-138">Les expressions Inline ne se terminent pas par un point-virgule.</span><span class="sxs-lookup"><span data-stu-id="f9639-138">Inline expressions don't end with a semicolon.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a><span data-ttu-id="f9639-139">4. vous utilisez des variables pour stocker des valeurs</span><span class="sxs-lookup"><span data-stu-id="f9639-139">4. You use variables to store values</span></span>

<span data-ttu-id="f9639-140">Vous pouvez stocker des valeurs dans une *variable*, y compris des chaînes, des nombres et des dates, etc. Vous créez une variable à l’aide du mot clé `var`.</span><span class="sxs-lookup"><span data-stu-id="f9639-140">You can store values in a *variable*, including strings, numbers, and dates, etc. You create a new variable using the `var` keyword.</span></span> <span data-ttu-id="f9639-141">Vous pouvez insérer des valeurs de variable directement dans une page à l’aide de `@`.</span><span class="sxs-lookup"><span data-stu-id="f9639-141">You can insert variable values directly in a page using `@`.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

<span data-ttu-id="f9639-142">Résultat affiché dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="f9639-142">The result displayed in a browser:</span></span>

![Rasoir-Img3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a><span data-ttu-id="f9639-144">5. vous placez les valeurs de chaîne littérale entre des guillemets doubles</span><span class="sxs-lookup"><span data-stu-id="f9639-144">5. You enclose literal string values in double quotation marks</span></span>

<span data-ttu-id="f9639-145">Une *chaîne* est une séquence de caractères qui sont traités comme du texte.</span><span class="sxs-lookup"><span data-stu-id="f9639-145">A *string* is a sequence of characters that are treated as text.</span></span> <span data-ttu-id="f9639-146">Pour spécifier une chaîne, vous devez la placer entre guillemets doubles :</span><span class="sxs-lookup"><span data-stu-id="f9639-146">To specify a string, you enclose it in double quotation marks:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

<span data-ttu-id="f9639-147">Si la chaîne que vous souhaitez afficher contient une barre oblique inverse (`\`) ou des guillemets doubles (`"`), utilisez un *littéral de chaîne Verbatim* préfixé avec l’opérateur `@`.</span><span class="sxs-lookup"><span data-stu-id="f9639-147">If the string that you want to display contains a backslash character ( `\` ) or double quotation marks ( `"` ), use a *verbatim string literal* that's prefixed with the `@` operator.</span></span> <span data-ttu-id="f9639-148">(Dans C#, le caractère \ a une signification spéciale, à moins que vous n’utilisiez un littéral de chaîne textuelle.)</span><span class="sxs-lookup"><span data-stu-id="f9639-148">(In C#, the \ character has special meaning unless you use a verbatim string literal.)</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

<span data-ttu-id="f9639-149">Pour incorporer des guillemets doubles, utilisez un littéral de chaîne Verbatim et répétez les guillemets :</span><span class="sxs-lookup"><span data-stu-id="f9639-149">To embed double quotation marks, use a verbatim string literal and repeat the quotation marks:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

<span data-ttu-id="f9639-150">Voici le résultat de l’utilisation de ces deux exemples dans une page :</span><span class="sxs-lookup"><span data-stu-id="f9639-150">Here's the result of using both of these examples in a page:</span></span>

![Rasoir-Img4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> <span data-ttu-id="f9639-152">Notez que le caractère `@` est utilisé à la fois pour marquer des littéraux C# de chaîne textuels dans et pour marquer le code dans les pages ASP.net.</span><span class="sxs-lookup"><span data-stu-id="f9639-152">Notice that the `@` character is used both to mark verbatim string literals in C# and to mark code in ASP.NET pages.</span></span>

### <a name="6-code-is-case-sensitive"></a><span data-ttu-id="f9639-153">6. le code respecte la casse</span><span class="sxs-lookup"><span data-stu-id="f9639-153">6. Code is case sensitive</span></span>

<span data-ttu-id="f9639-154">Dans C#, les mots clés (comme `var`, `true`et `if`) et les noms de variable respectent la casse.</span><span class="sxs-lookup"><span data-stu-id="f9639-154">In C#, keywords (like `var`, `true`, and `if`) and variable names are case sensitive.</span></span> <span data-ttu-id="f9639-155">Les lignes de code suivantes créent deux variables différentes, `lastName` et `LastName.`</span><span class="sxs-lookup"><span data-stu-id="f9639-155">The following lines of code create two different variables, `lastName` and `LastName.`</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

<span data-ttu-id="f9639-156">Si vous déclarez une variable en tant que `var lastName = "Smith";` et que vous tentez de référencer cette variable dans votre page comme `@LastName`, vous obtiendriez la valeur `"Jones"` au lieu de `"Smith"`.</span><span class="sxs-lookup"><span data-stu-id="f9639-156">If you declare a variable as `var lastName = "Smith";` and try to reference that variable in your page as `@LastName`, you would get the value `"Jones"` instead of `"Smith"`.</span></span>

> [!NOTE]
> <span data-ttu-id="f9639-157">Dans Visual Basic, les mots clés et les variables ne respectent *pas* la casse.</span><span class="sxs-lookup"><span data-stu-id="f9639-157">In Visual Basic, keywords and variables are *not* case sensitive.</span></span>

### <a name="7-much-of-your-coding-involves-objects"></a><span data-ttu-id="f9639-158">7. la majeure partie de votre codage implique des objets</span><span class="sxs-lookup"><span data-stu-id="f9639-158">7. Much of your coding involves objects</span></span>

<span data-ttu-id="f9639-159">Un *objet* représente un élément que vous pouvez programmer à &#8212; l’aide d’une page, d’une zone de texte, d’un fichier, d’une image, d’une requête Web, d’un message électronique, d’un enregistrement de client (ligne de base de données), etc. Les objets ont des propriétés qui décrivent leurs caractéristiques et que vous pouvez &#8212; lire ou modifier un objet de zone de texte a une propriété `Text` (entre autres), un objet de requête possède une propriété `Url`, un message électronique a une propriété `From` et un objet Customer possède une propriété `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="f9639-159">An *object* represents a thing that you can program with &#8212; a page, a text box, a file, an image, a web request, an email message, a customer record (database row), etc. Objects have properties that describe their characteristics and that you can read or change &#8212; a text box object has a `Text` property (among others), a request object has a `Url` property, an email message has a `From` property, and a customer object has a `FirstName` property.</span></span> <span data-ttu-id="f9639-160">Les objets ont également des méthodes qui sont les verbes de &quot;&quot; ils peuvent effectuer.</span><span class="sxs-lookup"><span data-stu-id="f9639-160">Objects also have methods that are the &quot;verbs&quot; they can perform.</span></span> <span data-ttu-id="f9639-161">Les exemples incluent la méthode `Save` d’un objet fichier, la méthode `Rotate` d’un objet image et la méthode `Send` d’un objet email.</span><span class="sxs-lookup"><span data-stu-id="f9639-161">Examples include a file object's `Save` method, an image object's `Rotate` method, and an email object's `Send` method.</span></span>

<span data-ttu-id="f9639-162">Vous travaillez souvent avec l’objet `Request`, qui vous donne des informations telles que les valeurs des zones de texte (champs de formulaire) sur la page, le type de navigateur qui a effectué la demande, l’URL de la page, l’identité de l’utilisateur, etc. L’exemple suivant montre comment accéder aux propriétés de l’objet `Request` et comment appeler la méthode `MapPath` de l’objet `Request`, qui vous donne le chemin d’accès absolu de la page sur le serveur :</span><span class="sxs-lookup"><span data-stu-id="f9639-162">You'll often work with the `Request` object, which gives you information like the values of text boxes (form fields) on the page, what type of browser made the request, the URL of the page, the user identity, etc. The following example shows how to access properties of the `Request` object and how to call the `MapPath` method of the `Request` object, which gives you the absolute path of the page on the server:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

<span data-ttu-id="f9639-163">Résultat affiché dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="f9639-163">The result displayed in a browser:</span></span>

![Rasoir-Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a><span data-ttu-id="f9639-165">8. vous pouvez écrire du code qui prend des décisions</span><span class="sxs-lookup"><span data-stu-id="f9639-165">8. You can write code that makes decisions</span></span>

<span data-ttu-id="f9639-166">Une fonctionnalité clé des pages Web dynamiques est que vous pouvez déterminer ce que vous devez faire en fonction des conditions.</span><span class="sxs-lookup"><span data-stu-id="f9639-166">A key feature of dynamic web pages is that you can determine what to do based on conditions.</span></span> <span data-ttu-id="f9639-167">La méthode la plus courante consiste à utiliser l’instruction `if` (et l’instruction facultative `else`).</span><span class="sxs-lookup"><span data-stu-id="f9639-167">The most common way to do this is with the `if` statement (and optional `else` statement).</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

<span data-ttu-id="f9639-168">L’instruction `if(IsPost)` est un moyen simple d’écrire des `if(IsPost == true)`.</span><span class="sxs-lookup"><span data-stu-id="f9639-168">The statement `if(IsPost)` is a shorthand way of writing `if(IsPost == true)`.</span></span> <span data-ttu-id="f9639-169">Outre les instructions `if`, il existe différentes façons de tester des conditions, de répéter des blocs de code, etc., qui sont décrits plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="f9639-169">Along with `if` statements, there are a variety of ways to test conditions, repeat blocks of code, and so on, which are described later in this article.</span></span>

<span data-ttu-id="f9639-170">Résultat affiché dans un navigateur (après avoir cliqué sur **Envoyer**) :</span><span class="sxs-lookup"><span data-stu-id="f9639-170">The result displayed in a browser (after clicking **Submit**):</span></span>

![Rasoir-Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a><span data-ttu-id="f9639-172">Méthodes HTTP d’extraction et de publication et propriété IsPost</span><span class="sxs-lookup"><span data-stu-id="f9639-172">HTTP GET and POST Methods and the IsPost Property</span></span>
> 
> <span data-ttu-id="f9639-173">Le protocole utilisé pour les pages Web (HTTP) prend en charge un nombre très limité de méthodes (verbes) utilisées pour effectuer des requêtes sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="f9639-173">The protocol used for web pages (HTTP) supports a very limited number of methods (verbs) that are used to make requests to the server.</span></span> <span data-ttu-id="f9639-174">Les deux types les plus courants sont obtenir, qui est utilisé pour lire une page, et la publication, qui est utilisée pour envoyer une page.</span><span class="sxs-lookup"><span data-stu-id="f9639-174">The two most common ones are GET, which is used to read a page, and POST, which is used to submit a page.</span></span> <span data-ttu-id="f9639-175">En général, la première fois qu’un utilisateur demande une page, la page est demandée à l’aide de la requête obtenir.</span><span class="sxs-lookup"><span data-stu-id="f9639-175">In general, the first time a user requests a page, the page is requested using GET.</span></span> <span data-ttu-id="f9639-176">Si l’utilisateur remplit un formulaire, puis clique sur un bouton envoyer, le navigateur envoie une demande de publication au serveur.</span><span class="sxs-lookup"><span data-stu-id="f9639-176">If the user fills in a form and then clicks a submit button, the browser makes a POST request to the server.</span></span>
> 
> <span data-ttu-id="f9639-177">Dans la programmation Web, il est souvent utile de savoir si une page est demandée en tant que « obtenir » ou « publication », afin que vous sachiez comment traiter la page.</span><span class="sxs-lookup"><span data-stu-id="f9639-177">In web programming, it's often useful to know whether a page is being requested as a GET or as a POST so that you know how to process the page.</span></span> <span data-ttu-id="f9639-178">Dans pages Web ASP.NET, vous pouvez utiliser la propriété `IsPost` pour déterminer si une requête est une requête d’extraction ou une publication.</span><span class="sxs-lookup"><span data-stu-id="f9639-178">In ASP.NET Web Pages, you can use the `IsPost` property to see whether a request is a GET or a POST.</span></span> <span data-ttu-id="f9639-179">Si la demande est une publication, la propriété `IsPost` retourne la valeur true et vous pouvez effectuer des opérations telles que lire les valeurs des zones de texte sur un formulaire.</span><span class="sxs-lookup"><span data-stu-id="f9639-179">If the request is a POST, the `IsPost` property will return true, and you can do things like read the values of text boxes on a form.</span></span> <span data-ttu-id="f9639-180">De nombreux exemples vous montrent comment traiter la page différemment en fonction de la valeur de `IsPost`.</span><span class="sxs-lookup"><span data-stu-id="f9639-180">Many examples you'll see show you how to process the page differently depending on the value of `IsPost`.</span></span>

## <a name="a-simple-code-example"></a><span data-ttu-id="f9639-181">Exemple de code simple</span><span class="sxs-lookup"><span data-stu-id="f9639-181">A Simple Code Example</span></span>

<span data-ttu-id="f9639-182">Cette procédure vous montre comment créer une page qui illustre des techniques de programmation de base.</span><span class="sxs-lookup"><span data-stu-id="f9639-182">This procedure shows you how to create a page that illustrates basic programming techniques.</span></span> <span data-ttu-id="f9639-183">Dans l’exemple, vous créez une page qui permet aux utilisateurs d’entrer deux nombres, puis de les ajouter et d’afficher le résultat.</span><span class="sxs-lookup"><span data-stu-id="f9639-183">In the example, you create a page that lets users enter two numbers, then it adds them and displays the result.</span></span>

1. <span data-ttu-id="f9639-184">Dans votre éditeur, créez un nouveau fichier et nommez-le *AddNumbers. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f9639-184">In your editor, create a new file and name it *AddNumbers.cshtml*.</span></span>
2. <span data-ttu-id="f9639-185">Copiez le code et le balisage suivants dans la page, en remplaçant tout ce qui se trouve déjà dans la page.</span><span class="sxs-lookup"><span data-stu-id="f9639-185">Copy the following code and markup into the page, replacing anything already in the page.</span></span>  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    <span data-ttu-id="f9639-186">Voici quelques points à noter :</span><span class="sxs-lookup"><span data-stu-id="f9639-186">Here are some things for you to note:</span></span>

    - <span data-ttu-id="f9639-187">Le caractère `@` démarre le premier bloc de code dans la page et précède la variable `totalMessage` qui est incorporée près du bas de la page.</span><span class="sxs-lookup"><span data-stu-id="f9639-187">The `@` character starts the first block of code in the page, and it precedes the `totalMessage` variable that's embedded near the bottom of the page.</span></span>
    - <span data-ttu-id="f9639-188">Le bloc en haut de la page est placé entre accolades.</span><span class="sxs-lookup"><span data-stu-id="f9639-188">The block at the top of the page is enclosed in braces.</span></span>
    - <span data-ttu-id="f9639-189">Dans le bloc en haut, toutes les lignes se terminent par un point-virgule.</span><span class="sxs-lookup"><span data-stu-id="f9639-189">In the block at the top, all lines end with a semicolon.</span></span>
    - <span data-ttu-id="f9639-190">Les variables `total`, `num1`, `num2`et `totalMessage` stockent plusieurs nombres et une chaîne.</span><span class="sxs-lookup"><span data-stu-id="f9639-190">The variables `total`, `num1`, `num2`, and `totalMessage` store several numbers and a string.</span></span>
    - <span data-ttu-id="f9639-191">La valeur de chaîne littérale assignée à la variable `totalMessage` est entre guillemets doubles.</span><span class="sxs-lookup"><span data-stu-id="f9639-191">The literal string value assigned to the `totalMessage` variable is in double quotation marks.</span></span>
    - <span data-ttu-id="f9639-192">Étant donné que le code respecte la casse, lorsque la variable `totalMessage` est utilisée vers le bas de la page, son nom doit correspondre exactement à la variable située en haut.</span><span class="sxs-lookup"><span data-stu-id="f9639-192">Because the code is case-sensitive, when the `totalMessage` variable is used near the bottom of the page, its name must match the variable at the top exactly.</span></span>
    - <span data-ttu-id="f9639-193">L' `num1.AsInt() + num2.AsInt()` d’expression montre comment utiliser des objets et des méthodes.</span><span class="sxs-lookup"><span data-stu-id="f9639-193">The expression `num1.AsInt() + num2.AsInt()` shows how to work with objects and methods.</span></span> <span data-ttu-id="f9639-194">La méthode `AsInt` sur chaque variable convertit la chaîne entrée par un utilisateur en nombre (un entier) afin que vous puissiez y effectuer une opération arithmétique.</span><span class="sxs-lookup"><span data-stu-id="f9639-194">The `AsInt` method on each variable converts the string entered by a user to a number (an integer) so that you can perform arithmetic on it.</span></span>
    - <span data-ttu-id="f9639-195">La balise `<form>` comprend un attribut `method="post"`.</span><span class="sxs-lookup"><span data-stu-id="f9639-195">The `<form>` tag includes a `method="post"` attribute.</span></span> <span data-ttu-id="f9639-196">Cela spécifie que lorsque l’utilisateur clique sur **Ajouter**, la page est envoyée au serveur à l’aide de la méthode http.</span><span class="sxs-lookup"><span data-stu-id="f9639-196">This specifies that when the user clicks **Add**, the page will be sent to the server using the HTTP POST method.</span></span> <span data-ttu-id="f9639-197">Lorsque la page est envoyée, le test de `if(IsPost)` prend la valeur true et le code conditionnel s’exécute et affiche le résultat de l’ajout des nombres.</span><span class="sxs-lookup"><span data-stu-id="f9639-197">When the page is submitted, the `if(IsPost)` test evaluates to true and the conditional code runs, displaying the result of adding the numbers.</span></span>
3. <span data-ttu-id="f9639-198">Enregistrez la page et exécutez-la dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="f9639-198">Save the page and run it in a browser.</span></span> <span data-ttu-id="f9639-199">(Assurez-vous que la page est sélectionnée dans l’espace de travail **fichiers** avant de l’exécuter.) Entrez deux nombres entiers, puis cliquez sur le bouton **Ajouter** .</span><span class="sxs-lookup"><span data-stu-id="f9639-199">(Make sure the page is selected in the **Files** workspace before you run it.) Enter two whole numbers and then click the **Add** button.</span></span> 

    ![Rasoir-Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a><span data-ttu-id="f9639-201">Concepts de programmation de base</span><span class="sxs-lookup"><span data-stu-id="f9639-201">Basic Programming Concepts</span></span>

<span data-ttu-id="f9639-202">Cet article vous fournit une vue d’ensemble de la programmation Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f9639-202">This article provides you with an overview of ASP.NET web programming.</span></span> <span data-ttu-id="f9639-203">Il ne s’agit pas d’un examen exhaustif, juste une brève présentation des concepts de programmation que vous utiliserez le plus souvent.</span><span class="sxs-lookup"><span data-stu-id="f9639-203">It isn't an exhaustive examination, just a quick tour through the programming concepts you'll use most often.</span></span> <span data-ttu-id="f9639-204">Même dans ce cas, il couvre presque tout ce dont vous avez besoin pour commencer à utiliser pages Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f9639-204">Even so, it covers almost everything you'll need to get started with ASP.NET Web Pages.</span></span>

<span data-ttu-id="f9639-205">Mais tout d’abord, un peu de support technique.</span><span class="sxs-lookup"><span data-stu-id="f9639-205">But first, a little technical background.</span></span>

### <a name="the-razor-syntax-server-code-and-aspnet"></a><span data-ttu-id="f9639-206">Syntaxe Razor, code du serveur et ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f9639-206">The Razor Syntax, Server Code, and ASP.NET</span></span>

<span data-ttu-id="f9639-207">Syntaxe Razor est une syntaxe de programmation simple pour incorporer du code serveur dans une page Web.</span><span class="sxs-lookup"><span data-stu-id="f9639-207">Razor syntax is a simple programming syntax for embedding server-based code in a web page.</span></span> <span data-ttu-id="f9639-208">Dans une page Web qui utilise la syntaxe Razor, il existe deux types de contenu : le contenu du client et le code du serveur.</span><span class="sxs-lookup"><span data-stu-id="f9639-208">In a web page that uses the Razor syntax, there are two kinds of content: client content and server code.</span></span> <span data-ttu-id="f9639-209">Le contenu client est le fichier que vous utilisez dans les pages Web : balisage HTML (éléments), informations de style, telles que CSS, peut-être un script client tel que JavaScript et du texte brut.</span><span class="sxs-lookup"><span data-stu-id="f9639-209">Client content is the stuff you're used to in web pages: HTML markup (elements), style information such as CSS, maybe some client script such as JavaScript, and plain text.</span></span>

<span data-ttu-id="f9639-210">Syntaxe Razor vous permet d’ajouter du code serveur au contenu de ce client.</span><span class="sxs-lookup"><span data-stu-id="f9639-210">Razor syntax lets you add server code to this client content.</span></span> <span data-ttu-id="f9639-211">Si la page contient du code serveur, le serveur exécute ce code en premier, avant d’envoyer la page au navigateur.</span><span class="sxs-lookup"><span data-stu-id="f9639-211">If there's server code in the page, the server runs that code first, before it sends the page to the browser.</span></span> <span data-ttu-id="f9639-212">En s’exécutant sur le serveur, le code peut effectuer des tâches qui peuvent être beaucoup plus complexes en utilisant uniquement le contenu du client, comme l’accès aux bases de données basées sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="f9639-212">By running on the server, the code can perform tasks that can be a lot more complex to do using client content alone, like accessing server-based databases.</span></span> <span data-ttu-id="f9639-213">Plus important encore, le code serveur peut créer dynamiquement du &#8212; contenu client, il peut générer un balisage HTML ou tout autre contenu à la volée, puis l’envoyer au navigateur avec tout code HTML statique que la page peut contenir.</span><span class="sxs-lookup"><span data-stu-id="f9639-213">Most importantly, server code can dynamically create client content &#8212; it can generate HTML markup or other content on the fly and then send it to the browser along with any static HTML that the page might contain.</span></span> <span data-ttu-id="f9639-214">Du point de vue du navigateur, le contenu client généré par votre code serveur n’est pas différent de tout autre contenu client.</span><span class="sxs-lookup"><span data-stu-id="f9639-214">From the browser's perspective, client content that's generated by your server code is no different than any other client content.</span></span> <span data-ttu-id="f9639-215">Comme vous l’avez déjà vu, le code serveur requis est assez simple.</span><span class="sxs-lookup"><span data-stu-id="f9639-215">As you've already seen, the server code that's required is quite simple.</span></span>

<span data-ttu-id="f9639-216">Les pages Web ASP.NET qui incluent les syntaxe Razor ont une extension de fichier spéciale ( *. cshtml* ou *. vbhtml*).</span><span class="sxs-lookup"><span data-stu-id="f9639-216">ASP.NET web pages that include the Razor syntax have a special file extension (*.cshtml* or *.vbhtml*).</span></span> <span data-ttu-id="f9639-217">Le serveur reconnaît ces extensions, exécute le code marqué avec syntaxe Razor, puis envoie la page au navigateur.</span><span class="sxs-lookup"><span data-stu-id="f9639-217">The server recognizes these extensions, runs the code that's marked with Razor syntax, and then sends the page to the browser.</span></span>

### <a name="where-does-aspnet-fit-in"></a><span data-ttu-id="f9639-218">Où ASP.NET s’intègre-t-il ?</span><span class="sxs-lookup"><span data-stu-id="f9639-218">Where does ASP.NET fit in?</span></span>

<span data-ttu-id="f9639-219">Syntaxe Razor est basé sur une technologie de Microsoft appelée ASP.NET, qui est à son tour basée sur le Microsoft .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f9639-219">Razor syntax is based on a technology from Microsoft called ASP.NET, which in turn is based on the Microsoft .NET Framework.</span></span> <span data-ttu-id="f9639-220">The.NET Framework est une infrastructure de programmation volumineuse et complète de Microsoft pour le développement de pratiquement n’importe quel type d’application informatique.</span><span class="sxs-lookup"><span data-stu-id="f9639-220">The.NET Framework is a big, comprehensive programming framework from Microsoft for developing virtually any type of computer application.</span></span> <span data-ttu-id="f9639-221">ASP.NET est la partie du .NET Framework spécifiquement conçue pour la création d’applications Web.</span><span class="sxs-lookup"><span data-stu-id="f9639-221">ASP.NET is the part of the .NET Framework that's specifically designed for creating web applications.</span></span> <span data-ttu-id="f9639-222">Les développeurs ont utilisé ASP.NET pour créer un grand nombre des sites Web les plus volumineux et les plus fréquents dans le monde.</span><span class="sxs-lookup"><span data-stu-id="f9639-222">Developers have used ASP.NET to create many of the largest and highest-traffic websites in the world.</span></span> <span data-ttu-id="f9639-223">(Chaque fois que vous voyez l’extension de nom de fichier *. aspx* dans le cadre de l’URL d’un site, vous savez que le site a été écrit à l’aide de ASP.net.)</span><span class="sxs-lookup"><span data-stu-id="f9639-223">(Any time you see the file-name extension *.aspx* as part of the URL in a site, you'll know that the site was written using ASP.NET.)</span></span>

<span data-ttu-id="f9639-224">Le syntaxe Razor vous donne toute la puissance de ASP.NET, mais à l’aide d’une syntaxe simplifiée qui est plus facile à apprendre si vous êtes débutant et qui vous permet de vous rendre plus productif si vous êtes un expert.</span><span class="sxs-lookup"><span data-stu-id="f9639-224">The Razor syntax gives you all the power of ASP.NET, but using a simplified syntax that's easier to learn if you're a beginner and that makes you more productive if you're an expert.</span></span> <span data-ttu-id="f9639-225">Même si cette syntaxe est simple à utiliser, sa relation de famille avec ASP.NET et la .NET Framework signifie que lorsque vos sites Web sont plus sophistiqués, vous bénéficiez de la puissance des infrastructures plus grandes disponibles.</span><span class="sxs-lookup"><span data-stu-id="f9639-225">Even though this syntax is simple to use, its family relationship to ASP.NET and the .NET Framework means that as your websites become more sophisticated, you have the power of the larger frameworks available to you.</span></span>

![Razor-Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> <span data-ttu-id="f9639-227">**Classes et instances**</span><span class="sxs-lookup"><span data-stu-id="f9639-227">**Classes and Instances**</span></span>
> 
> <span data-ttu-id="f9639-228">ASP.NET Server Code utilise des objets, qui sont à leur tour basés sur l’idée des classes.</span><span class="sxs-lookup"><span data-stu-id="f9639-228">ASP.NET server code uses objects, which are in turn built on the idea of classes.</span></span> <span data-ttu-id="f9639-229">La classe est la définition ou le modèle d’un objet.</span><span class="sxs-lookup"><span data-stu-id="f9639-229">The class is the definition or template for an object.</span></span> <span data-ttu-id="f9639-230">Par exemple, une application peut contenir une classe `Customer` qui définit les propriétés et les méthodes dont tout objet client a besoin.</span><span class="sxs-lookup"><span data-stu-id="f9639-230">For example, an application might contain a `Customer` class that defines the properties and methods that any customer object needs.</span></span>
> 
> <span data-ttu-id="f9639-231">Lorsque l’application doit utiliser des informations client réelles, elle crée une instance de (ou *instancie*) un objet client.</span><span class="sxs-lookup"><span data-stu-id="f9639-231">When the application needs to work with actual customer information, it creates an instance of (or *instantiates*) a customer object.</span></span> <span data-ttu-id="f9639-232">Chaque client individuel est une instance distincte de la classe `Customer`.</span><span class="sxs-lookup"><span data-stu-id="f9639-232">Each individual customer is a separate instance of the `Customer` class.</span></span> <span data-ttu-id="f9639-233">Chaque instance prend en charge les mêmes propriétés et méthodes, mais les valeurs de propriété pour chaque instance sont généralement différentes, car chaque objet Customer est unique.</span><span class="sxs-lookup"><span data-stu-id="f9639-233">Every instance supports the same properties and methods, but the property values for each instance are typically different, because each customer object is unique.</span></span> <span data-ttu-id="f9639-234">Dans un objet client, la propriété `LastName` peut être « Smith ». dans un autre objet client, la propriété `LastName` peut être « Jones ».</span><span class="sxs-lookup"><span data-stu-id="f9639-234">In one customer object, the `LastName` property might be "Smith"; in another customer object, the `LastName` property might be "Jones."</span></span>
> 
> <span data-ttu-id="f9639-235">De même, toute page Web individuelle de votre site est un objet `Page` qui est une instance de la classe `Page`.</span><span class="sxs-lookup"><span data-stu-id="f9639-235">Similarly, any individual web page in your site is a `Page` object that's an instance of the `Page` class.</span></span> <span data-ttu-id="f9639-236">Un bouton sur la page est un objet `Button` qui est une instance de la classe `Button`, et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="f9639-236">A button on the page is a `Button` object that is an instance of the `Button` class, and so on.</span></span> <span data-ttu-id="f9639-237">Chaque instance a ses propres caractéristiques, mais elles sont toutes basées sur ce qui est spécifié dans la définition de classe de l’objet.</span><span class="sxs-lookup"><span data-stu-id="f9639-237">Each instance has its own characteristics, but they all are based on what's specified in the object's class definition.</span></span>

## <a name="basic-syntax"></a><span data-ttu-id="f9639-238">Syntaxe de base</span><span class="sxs-lookup"><span data-stu-id="f9639-238">Basic Syntax</span></span>

<span data-ttu-id="f9639-239">Vous avez vu précédemment un exemple de base de la création d’une page de pages Web ASP.NET, ainsi que la façon dont vous pouvez ajouter du code serveur au balisage HTML.</span><span class="sxs-lookup"><span data-stu-id="f9639-239">Earlier you saw a basic example of how to create an ASP.NET Web Pages page, and how you can add server code to HTML markup.</span></span> <span data-ttu-id="f9639-240">Ici, vous allez apprendre les principes fondamentaux de l’écriture du code serveur &#8212; ASP.net à l’aide de la syntaxe Razor qui correspond aux règles du langage de programmation.</span><span class="sxs-lookup"><span data-stu-id="f9639-240">Here you'll learn the basics of writing ASP.NET server code using the Razor syntax &#8212; that is, the programming language rules.</span></span>

<span data-ttu-id="f9639-241">Si vous êtes familiarisé avec la programmation (surtout si vous avez utilisé C C++, C#,, Visual Basic ou JavaScript), la plupart des éléments que vous avez lus ici vous seront familiers.</span><span class="sxs-lookup"><span data-stu-id="f9639-241">If you're experienced with programming (especially if you've used C, C++, C#, Visual Basic, or JavaScript), much of what you read here will be familiar.</span></span> <span data-ttu-id="f9639-242">Vous devrez probablement vous familiariser avec la façon dont le code du serveur est ajouté au balisage dans les fichiers *. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="f9639-242">You'll probably need to familiarize yourself only with how server code is added to markup in *.cshtml* files.</span></span>

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a><span data-ttu-id="f9639-243">Combinaison de texte, de balisage et de code dans des blocs de code</span><span class="sxs-lookup"><span data-stu-id="f9639-243">Combining Text, Markup, and Code in Code Blocks</span></span>

<span data-ttu-id="f9639-244">Dans les blocs de code serveur, vous souhaitez souvent afficher du texte ou des balises (ou les deux) sur la page.</span><span class="sxs-lookup"><span data-stu-id="f9639-244">In server code blocks, you often want to output text or markup (or both) to the page.</span></span> <span data-ttu-id="f9639-245">Si un bloc de code serveur contient du texte qui n’est pas du code et qu’il doit à la place être rendu tel quel, ASP.NET doit être en mesure de distinguer ce texte du code.</span><span class="sxs-lookup"><span data-stu-id="f9639-245">If a server code block contains text that's not code and that instead should be rendered as is, ASP.NET needs to be able to distinguish that text from code.</span></span> <span data-ttu-id="f9639-246">Plusieurs méthodes sont possibles.</span><span class="sxs-lookup"><span data-stu-id="f9639-246">There are several ways to do this.</span></span>

- <span data-ttu-id="f9639-247">Placez le texte dans un élément HTML comme `<p></p>` ou `<em></em>`:</span><span class="sxs-lookup"><span data-stu-id="f9639-247">Enclose the text in an HTML element like `<p></p>` or `<em></em>`:</span></span>   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    <span data-ttu-id="f9639-248">L’élément HTML peut inclure du texte, des éléments HTML supplémentaires et des expressions de code de serveur.</span><span class="sxs-lookup"><span data-stu-id="f9639-248">The HTML element can include text, additional HTML elements, and server-code expressions.</span></span> <span data-ttu-id="f9639-249">Quand ASP.NET voit la balise HTML d’ouverture (par exemple, `<p>`), elle affiche tout ce qui inclut l’élément et son contenu tel quel dans le navigateur, en résolvant les expressions de code serveur au fur et à mesure.</span><span class="sxs-lookup"><span data-stu-id="f9639-249">When ASP.NET sees the opening HTML tag (for example, `<p>`), it renders everything including the element and its content as is to the browser, resolving server-code expressions as it goes.</span></span>
- <span data-ttu-id="f9639-250">Utilisez l’opérateur `@:` ou l’élément `<text>`.</span><span class="sxs-lookup"><span data-stu-id="f9639-250">Use the `@:` operator or the `<text>` element.</span></span> <span data-ttu-id="f9639-251">Le `@:` génère une seule ligne de contenu contenant du texte brut ou des balises HTML sans correspondance ; l’élément `<text>` encadre plusieurs lignes à la sortie.</span><span class="sxs-lookup"><span data-stu-id="f9639-251">The `@:` outputs a single line of content containing plain text or unmatched HTML tags; the `<text>` element encloses multiple lines to output.</span></span> <span data-ttu-id="f9639-252">Ces options sont utiles lorsque vous ne souhaitez pas restituer un élément HTML dans le cadre de la sortie.</span><span class="sxs-lookup"><span data-stu-id="f9639-252">These options are useful when you don't want to render an HTML element as part of the output.</span></span>  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    <span data-ttu-id="f9639-253">Si vous souhaitez générer plusieurs lignes de texte ou des balises HTML sans correspondance, vous pouvez faire précéder chaque ligne d' `@:`, ou vous pouvez placer la ligne dans un élément `<text>`.</span><span class="sxs-lookup"><span data-stu-id="f9639-253">If you want to output multiple lines of text or unmatched HTML tags, you can precede each line with `@:`, or you can enclose the line in a `<text>` element.</span></span> <span data-ttu-id="f9639-254">À l’instar de l’opérateur `@:`, les balises`<text>` sont utilisées par ASP.NET pour identifier le contenu de texte et ne sont jamais rendues dans la sortie de page.</span><span class="sxs-lookup"><span data-stu-id="f9639-254">Like the `@:` operator,`<text>` tags are used by ASP.NET to identify text content and are never rendered in the page output.</span></span>

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    <span data-ttu-id="f9639-255">Le premier exemple répète l’exemple précédent, mais utilise une seule paire de balises `<text>` pour encadrer le texte à afficher.</span><span class="sxs-lookup"><span data-stu-id="f9639-255">The first example repeats the previous example but uses a single pair of `<text>` tags to enclose the text to render.</span></span> <span data-ttu-id="f9639-256">Dans le deuxième exemple, les balises `<text>` et `</text>` encadrent trois lignes, qui ont toutes des textes sans relation contenant-contenu et des balises HTML non appariées (`<br />`), ainsi que du code serveur et des balises HTML correspondantes.</span><span class="sxs-lookup"><span data-stu-id="f9639-256">In the second example, the `<text>` and `</text>` tags enclose three lines, all of which have some uncontained text and unmatched HTML tags (`<br />`), along with server code and matched HTML tags.</span></span> <span data-ttu-id="f9639-257">Là encore, vous pouvez également faire précéder chaque ligne d’un opérateur `@:` ; les deux méthodes fonctionnent.</span><span class="sxs-lookup"><span data-stu-id="f9639-257">Again, you could also precede each line individually with the `@:` operator; either way works.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f9639-258">Lorsque vous affichez le texte comme indiqué dans &#8212; cette section à l’aide d’un élément HTML, l’opérateur `@:` &#8212; ou l’élément `<text>` ASP.net n’encode pas la sortie au format html.</span><span class="sxs-lookup"><span data-stu-id="f9639-258">When you output text as shown in this section &#8212; using an HTML element, the `@:` operator, or the `<text>` element &#8212; ASP.NET doesn't HTML-encode the output.</span></span> <span data-ttu-id="f9639-259">(Comme indiqué précédemment, ASP.NET encode la sortie des expressions de code serveur et des blocs de code serveur précédés de `@`, sauf dans les cas particuliers indiqués dans cette section.)</span><span class="sxs-lookup"><span data-stu-id="f9639-259">(As noted earlier, ASP.NET does encode the output of server code expressions and server code blocks that are preceded by `@`, except in the special cases noted in this section.)</span></span>

### <a name="whitespace"></a><span data-ttu-id="f9639-260">Whitespace</span><span class="sxs-lookup"><span data-stu-id="f9639-260">Whitespace</span></span>

<span data-ttu-id="f9639-261">Les espaces supplémentaires dans une instruction (et en dehors d’un littéral de chaîne) n’affectent pas l’instruction :</span><span class="sxs-lookup"><span data-stu-id="f9639-261">Extra spaces in a statement (and outside of a string literal) don't affect the statement:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

<span data-ttu-id="f9639-262">Un saut de ligne dans une instruction n’a aucun effet sur l’instruction et vous pouvez encapsuler des instructions pour améliorer la lisibilité.</span><span class="sxs-lookup"><span data-stu-id="f9639-262">A line break in a statement has no effect on the statement, and you can wrap statements for readability.</span></span> <span data-ttu-id="f9639-263">Les instructions suivantes sont les mêmes :</span><span class="sxs-lookup"><span data-stu-id="f9639-263">The following statements are the same:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

<span data-ttu-id="f9639-264">Toutefois, vous ne pouvez pas encapsuler une ligne au milieu d’un littéral de chaîne.</span><span class="sxs-lookup"><span data-stu-id="f9639-264">However, you can't wrap a line in the middle of a string literal.</span></span> <span data-ttu-id="f9639-265">L’exemple suivant ne fonctionne pas :</span><span class="sxs-lookup"><span data-stu-id="f9639-265">The following example doesn't work:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

<span data-ttu-id="f9639-266">Pour combiner une longue chaîne encapsulée sur plusieurs lignes, comme le code ci-dessus, il existe deux options.</span><span class="sxs-lookup"><span data-stu-id="f9639-266">To combine a long string that wraps to multiple lines like the above code, there are two options.</span></span> <span data-ttu-id="f9639-267">Vous pouvez utiliser l’opérateur de concaténation (`+`), que vous verrez plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="f9639-267">You can use the concatenation operator (`+`), which you'll see later in this article.</span></span> <span data-ttu-id="f9639-268">Vous pouvez également utiliser le caractère `@` pour créer un littéral de chaîne textuelle, comme vous l’avez vu précédemment dans cet article.</span><span class="sxs-lookup"><span data-stu-id="f9639-268">You can also use the `@` character to create a verbatim string literal, as you saw earlier in this article.</span></span> <span data-ttu-id="f9639-269">Vous pouvez rompre les littéraux de chaîne Verbatim sur les lignes :</span><span class="sxs-lookup"><span data-stu-id="f9639-269">You can break verbatim string literals across lines:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a><span data-ttu-id="f9639-270">Commentaires sur le code (et le balisage)</span><span class="sxs-lookup"><span data-stu-id="f9639-270">Code (and Markup) Comments</span></span>

<span data-ttu-id="f9639-271">Les commentaires vous permettent de laisser des notes pour vous-même ou d’autres.</span><span class="sxs-lookup"><span data-stu-id="f9639-271">Comments let you leave notes for yourself or others.</span></span> <span data-ttu-id="f9639-272">Ils vous permettent également de désactiver (*commenter*) une section de code ou de balisage que vous ne souhaitez pas exécuter, mais que vous souhaitez conserver dans votre page pour le moment.</span><span class="sxs-lookup"><span data-stu-id="f9639-272">They also allow you to disable (*comment out*) a section of code or markup that you don't want to run but want to keep in your page for the time being.</span></span>

<span data-ttu-id="f9639-273">Il existe une syntaxe de commentaire différente pour le code Razor et pour le balisage HTML.</span><span class="sxs-lookup"><span data-stu-id="f9639-273">There's different commenting syntax for Razor code and for HTML markup.</span></span> <span data-ttu-id="f9639-274">Comme avec tout le code Razor, les commentaires Razor sont traités (puis supprimés) sur le serveur avant que la page ne soit envoyée au navigateur.</span><span class="sxs-lookup"><span data-stu-id="f9639-274">As with all Razor code, Razor comments are processed (and then removed) on the server before the page is sent to the browser.</span></span> <span data-ttu-id="f9639-275">Par conséquent, la syntaxe de commentaire Razor vous permet de placer des commentaires dans le code (ou même dans le balisage) que vous pouvez voir lorsque vous modifiez le fichier, mais que les utilisateurs ne voient pas, même dans la source de la page.</span><span class="sxs-lookup"><span data-stu-id="f9639-275">Therefore, the Razor commenting syntax lets you put comments into the code (or even into the markup) that you can see when you edit the file, but that users don't see, even in the page source.</span></span>

<span data-ttu-id="f9639-276">Pour les commentaires Razor ASP.NET, vous démarrez le commentaire avec `@*` et vous le Terminez avec `*@`.</span><span class="sxs-lookup"><span data-stu-id="f9639-276">For ASP.NET Razor comments, you start the comment with `@*` and end it with `*@`.</span></span> <span data-ttu-id="f9639-277">Le commentaire peut être sur une ou plusieurs lignes :</span><span class="sxs-lookup"><span data-stu-id="f9639-277">The comment can be on one line or multiple lines:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

<span data-ttu-id="f9639-278">Voici un commentaire dans un bloc de code :</span><span class="sxs-lookup"><span data-stu-id="f9639-278">Here is a comment within a code block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

<span data-ttu-id="f9639-279">Voici le même bloc de code, avec la ligne de code commentée afin qu’il ne s’exécute pas :</span><span class="sxs-lookup"><span data-stu-id="f9639-279">Here is the same block of code, with the line of code commented out so that it won't run:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

<span data-ttu-id="f9639-280">À l’intérieur d’un bloc de code, comme alternative à l’utilisation de la syntaxe des commentaires Razor, vous pouvez utiliser la syntaxe de commentaire du langage de C#programmation que vous utilisez, par exemple :</span><span class="sxs-lookup"><span data-stu-id="f9639-280">Inside a code block, as an alternative to using Razor comment syntax, you can use the commenting syntax of the programming language you're using, such as C#:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

<span data-ttu-id="f9639-281">Dans C#, les commentaires sur une seule ligne sont précédés des caractères `//` et les commentaires sur plusieurs lignes commencent par `/*` et se terminent par `*/`.</span><span class="sxs-lookup"><span data-stu-id="f9639-281">In C#, single-line comments are preceded by the `//` characters, and multi-line comments begin with `/*` and end with `*/`.</span></span> <span data-ttu-id="f9639-282">(Comme avec les commentaires Razor C# , les commentaires ne sont pas rendus dans le navigateur.)</span><span class="sxs-lookup"><span data-stu-id="f9639-282">(As with Razor comments, C# comments are not rendered to the browser.)</span></span>

<span data-ttu-id="f9639-283">Pour le balisage, comme vous le savez probablement, vous pouvez créer un commentaire HTML :</span><span class="sxs-lookup"><span data-stu-id="f9639-283">For markup, as you probably know, you can create an HTML comment:</span></span>

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

<span data-ttu-id="f9639-284">Les commentaires HTML commencent par `<!--` caractères et se terminent par `-->`.</span><span class="sxs-lookup"><span data-stu-id="f9639-284">HTML comments start with `<!--` characters and end with `-->`.</span></span> <span data-ttu-id="f9639-285">Vous pouvez utiliser des commentaires HTML pour entourer non seulement du texte, mais également tout balisage HTML que vous pouvez conserver dans la page, mais que vous ne souhaitez pas afficher.</span><span class="sxs-lookup"><span data-stu-id="f9639-285">You can use HTML comments to surround not only text, but also any HTML markup that you may want to keep in the page but don't want to render.</span></span> <span data-ttu-id="f9639-286">Ce commentaire HTML masque l’intégralité du contenu des balises et le texte qu’ils contiennent :</span><span class="sxs-lookup"><span data-stu-id="f9639-286">This HTML comment will hide the entire content of the tags and the text they contain:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

<span data-ttu-id="f9639-287">Contrairement aux commentaires Razor, les commentaires HTML *sont* rendus dans la page et l’utilisateur peut les voir en affichant la source de la page.</span><span class="sxs-lookup"><span data-stu-id="f9639-287">Unlike Razor comments, HTML comments *are* rendered to the page and the user can see them by viewing the page source.</span></span>

<span data-ttu-id="f9639-288">Razor présente des limitations sur les blocs imbriqués de C#.</span><span class="sxs-lookup"><span data-stu-id="f9639-288">Razor has limitations on nested blocks of C#.</span></span> <span data-ttu-id="f9639-289">Pour plus d’informations [, C# consultez variables nommées et blocs imbriqués générer du code endommagé](http://aspnetwebstack.codeplex.com/workitem/1914)</span><span class="sxs-lookup"><span data-stu-id="f9639-289">For more information see [Named C# Variables and Nested Blocks Generate Broken Code](http://aspnetwebstack.codeplex.com/workitem/1914)</span></span>

## <a name="variables"></a><span data-ttu-id="f9639-290">Variables</span><span class="sxs-lookup"><span data-stu-id="f9639-290">Variables</span></span>

<span data-ttu-id="f9639-291">Une variable est un objet nommé que vous utilisez pour stocker des données.</span><span class="sxs-lookup"><span data-stu-id="f9639-291">A variable is a named object that you use to store data.</span></span> <span data-ttu-id="f9639-292">Vous pouvez nommer toutes les variables, mais le nom doit commencer par un caractère alphabétique et ne peut pas contenir d’espaces blancs ou de caractères réservés.</span><span class="sxs-lookup"><span data-stu-id="f9639-292">You can name variables anything, but the name must begin with an alphabetic character and it cannot contain whitespace or reserved characters.</span></span>

### <a name="variables-and-data-types"></a><span data-ttu-id="f9639-293">Variables et types de données</span><span class="sxs-lookup"><span data-stu-id="f9639-293">Variables and Data Types</span></span>

<span data-ttu-id="f9639-294">Une variable peut avoir un type de données spécifique, qui indique le type de données qui est stocké dans la variable.</span><span class="sxs-lookup"><span data-stu-id="f9639-294">A variable can have a specific data type, which indicates what kind of data is stored in the variable.</span></span> <span data-ttu-id="f9639-295">Vous pouvez avoir des variables de chaîne qui stockent des valeurs de chaîne (comme &quot;Hello World&quot;), des variables de type entier qui stockent des valeurs de nombres entiers (comme 3 ou 79) et des variables de date qui stockent des valeurs de date dans divers formats (comme 4/12/2012 ou 2009).</span><span class="sxs-lookup"><span data-stu-id="f9639-295">You can have string variables that store string values (like &quot;Hello world&quot;), integer variables that store whole-number values (like 3 or 79), and date variables that store date values in a variety of formats (like 4/12/2012 or March 2009).</span></span> <span data-ttu-id="f9639-296">Et il existe de nombreux autres types de données que vous pouvez utiliser.</span><span class="sxs-lookup"><span data-stu-id="f9639-296">And there are many other data types you can use.</span></span>

<span data-ttu-id="f9639-297">Toutefois, vous n’êtes généralement pas obligé de spécifier un type pour une variable.</span><span class="sxs-lookup"><span data-stu-id="f9639-297">However, you generally don't have to specify a type for a variable.</span></span> <span data-ttu-id="f9639-298">La plupart du temps, ASP.NET peut déterminer le type en fonction de la façon dont les données de la variable sont utilisées.</span><span class="sxs-lookup"><span data-stu-id="f9639-298">Most of the time, ASP.NET can figure out the type based on how the data in the variable is being used.</span></span> <span data-ttu-id="f9639-299">(Parfois, vous devez spécifier un type ; vous verrez des exemples où cela est vrai.)</span><span class="sxs-lookup"><span data-stu-id="f9639-299">(Occasionally you must specify a type; you'll see examples where this is true.)</span></span>

<span data-ttu-id="f9639-300">Vous déclarez une variable à l’aide du mot clé `var` (si vous ne souhaitez pas spécifier un type) ou en utilisant le nom du type :</span><span class="sxs-lookup"><span data-stu-id="f9639-300">You declare a variable using the `var` keyword (if you don't want to specify a type) or by using the name of the type:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

<span data-ttu-id="f9639-301">L’exemple suivant montre des utilisations typiques de variables dans une page Web :</span><span class="sxs-lookup"><span data-stu-id="f9639-301">The following example shows some typical uses of variables in a web page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

<span data-ttu-id="f9639-302">Si vous combinez les exemples précédents dans une page, vous voyez que cela s’affiche dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="f9639-302">If you combine the previous examples in a page, you see this displayed in a browser:</span></span>

![Rasoir-Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a><span data-ttu-id="f9639-304">Conversion et test des types de données</span><span class="sxs-lookup"><span data-stu-id="f9639-304">Converting and Testing Data Types</span></span>

<span data-ttu-id="f9639-305">Bien que ASP.NET puisse généralement déterminer automatiquement un type de données, il est parfois impossible de le faire.</span><span class="sxs-lookup"><span data-stu-id="f9639-305">Although ASP.NET can usually determine a data type automatically, sometimes it can't.</span></span> <span data-ttu-id="f9639-306">Par conséquent, vous devrez peut-être aider ASP.NET en effectuant une conversion explicite.</span><span class="sxs-lookup"><span data-stu-id="f9639-306">Therefore, you might need to help ASP.NET out by performing an explicit conversion.</span></span> <span data-ttu-id="f9639-307">Même si vous n’avez pas besoin de convertir les types, il est parfois utile de tester le type de données que vous pouvez utiliser.</span><span class="sxs-lookup"><span data-stu-id="f9639-307">Even if you don't have to convert types, sometimes it's helpful to test to see what type of data you might be working with.</span></span>

<span data-ttu-id="f9639-308">Le cas le plus courant est que vous devez convertir une chaîne en un autre type, tel qu’un entier ou une date.</span><span class="sxs-lookup"><span data-stu-id="f9639-308">The most common case is that you have to convert a string to another type, such as to an integer or date.</span></span> <span data-ttu-id="f9639-309">L’exemple suivant montre un cas typique dans lequel vous devez convertir une chaîne en nombre.</span><span class="sxs-lookup"><span data-stu-id="f9639-309">The following example shows a typical case where you must convert a string to a number.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

<span data-ttu-id="f9639-310">En règle générale, l’entrée utilisateur vous est fournie sous forme de chaînes.</span><span class="sxs-lookup"><span data-stu-id="f9639-310">As a rule, user input comes to you as strings.</span></span> <span data-ttu-id="f9639-311">Même si vous avez demandé aux utilisateurs d’entrer un nombre, et même s’ils ont entré un chiffre, lorsque l’entrée de l’utilisateur est envoyée et que vous la lisez dans le code, les données sont au format de chaîne.</span><span class="sxs-lookup"><span data-stu-id="f9639-311">Even if you've prompted users to enter a number, and even if they've entered a digit, when user input is submitted and you read it in code, the data is in string format.</span></span> <span data-ttu-id="f9639-312">Par conséquent, vous devez convertir la chaîne en nombre.</span><span class="sxs-lookup"><span data-stu-id="f9639-312">Therefore, you must convert the string to a number.</span></span> <span data-ttu-id="f9639-313">Dans l’exemple, si vous essayez d’effectuer des opérations arithmétiques sur les valeurs sans les convertir, l’erreur suivante se produit, car ASP.NET ne peut pas ajouter deux chaînes :</span><span class="sxs-lookup"><span data-stu-id="f9639-313">In the example, if you try to perform arithmetic on the values without converting them, the following error results, because ASP.NET cannot add two strings:</span></span>

<span data-ttu-id="f9639-314">*Impossible de convertir implicitement le type’String’en’int'.*</span><span class="sxs-lookup"><span data-stu-id="f9639-314">*Cannot implicitly convert type 'string' to 'int'.*</span></span>

<span data-ttu-id="f9639-315">Pour convertir les valeurs en entiers, vous appelez la méthode `AsInt`.</span><span class="sxs-lookup"><span data-stu-id="f9639-315">To convert the values to integers, you call the `AsInt` method.</span></span> <span data-ttu-id="f9639-316">Si la conversion réussit, vous pouvez ajouter les nombres.</span><span class="sxs-lookup"><span data-stu-id="f9639-316">If the conversion is successful, you can then add the numbers.</span></span>

<span data-ttu-id="f9639-317">Le tableau suivant répertorie quelques méthodes de conversion et de test courantes pour les variables.</span><span class="sxs-lookup"><span data-stu-id="f9639-317">The following table lists some common conversion and test methods for variables.</span></span>

:::row:::
    :::column:::
    <span data-ttu-id="f9639-318"><strong>Méthode</strong></span><span class="sxs-lookup"><span data-stu-id="f9639-318"><strong>Method</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="f9639-319"><strong>Description</strong></span><span class="sxs-lookup"><span data-stu-id="f9639-319"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="f9639-320"><strong>Exemple</strong></span><span class="sxs-lookup"><span data-stu-id="f9639-320"><strong>Example</strong></span></span>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="f9639-321">Convertit une chaîne qui représente un nombre entier (comme « 593 ») en entier.</span><span class="sxs-lookup"><span data-stu-id="f9639-321">Converts a string that represents a whole number (like "593") to an integer.</span></span>
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
    <span data-ttu-id="f9639-322">Convertit une chaîne comme &quot;true&quot; ou &quot;false&quot; en un type booléen.</span><span class="sxs-lookup"><span data-stu-id="f9639-322">Converts a string like &quot;true&quot; or &quot;false&quot; to a Boolean type.</span></span>
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
    <span data-ttu-id="f9639-323">Convertit une chaîne qui a une valeur décimale comme &quot;1,3&quot; ou &quot;7,439&quot; en un nombre à virgule flottante.</span><span class="sxs-lookup"><span data-stu-id="f9639-323">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a floating-point number.</span></span>
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
    <span data-ttu-id="f9639-324">Convertit une chaîne qui a une valeur décimale comme &quot;1,3&quot; ou &quot;7,439&quot; en nombre décimal.</span><span class="sxs-lookup"><span data-stu-id="f9639-324">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a decimal number.</span></span> <span data-ttu-id="f9639-325">(Dans ASP.NET, un nombre décimal est plus précis qu’un nombre à virgule flottante.)</span><span class="sxs-lookup"><span data-stu-id="f9639-325">(In ASP.NET, a decimal number is more precise than a floating-point number.)</span></span>
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
    <span data-ttu-id="f9639-326">Convertit une chaîne qui représente une valeur de date et d’heure dans le type de `DateTime` ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f9639-326">Converts a string that represents a date and time value to the ASP.NET `DateTime` type.</span></span>
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
    <span data-ttu-id="f9639-327">Convertit tout autre type de données en chaîne.</span><span class="sxs-lookup"><span data-stu-id="f9639-327">Converts any other data type to a string.</span></span>
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)]
    :::column-end:::
:::row-end:::

## <a name="operators"></a><span data-ttu-id="f9639-328">Opérateurs</span><span class="sxs-lookup"><span data-stu-id="f9639-328">Operators</span></span>

<span data-ttu-id="f9639-329">Un opérateur est un mot clé ou un caractère qui indique à ASP.NET le type de commande à effectuer dans une expression.</span><span class="sxs-lookup"><span data-stu-id="f9639-329">An operator is a keyword or character that tells ASP.NET what kind of command to perform in an expression.</span></span> <span data-ttu-id="f9639-330">La C# langue (et le syntaxe Razor basé sur celle-ci) prend en charge de nombreux opérateurs, mais vous ne devez en reconnaître que quelques-uns pour commencer.</span><span class="sxs-lookup"><span data-stu-id="f9639-330">The C# language (and the Razor syntax that's based on it) supports many operators, but you only need to recognize a few to get started.</span></span> <span data-ttu-id="f9639-331">Le tableau suivant récapitule les opérateurs les plus courants.</span><span class="sxs-lookup"><span data-stu-id="f9639-331">The following table summarizes the most common operators.</span></span>

:::row:::
    :::column:::
    <span data-ttu-id="f9639-332"><strong>Operator</strong></span><span class="sxs-lookup"><span data-stu-id="f9639-332"><strong>Operator</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="f9639-333"><strong>Description</strong></span><span class="sxs-lookup"><span data-stu-id="f9639-333"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="f9639-334"><strong>Exemples</strong></span><span class="sxs-lookup"><span data-stu-id="f9639-334"><strong>Examples</strong></span></span>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        <span data-ttu-id="f9639-335">`+` `-` `*` `/`</span><span class="sxs-lookup"><span data-stu-id="f9639-335">`+` `-` `*` `/`</span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="f9639-336">Opérateurs mathématiques utilisés dans les expressions numériques.</span><span class="sxs-lookup"><span data-stu-id="f9639-336">Math operators used in numerical expressions.</span></span>
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
    <span data-ttu-id="f9639-337">Assignation.</span><span class="sxs-lookup"><span data-stu-id="f9639-337">Assignment.</span></span> <span data-ttu-id="f9639-338">Assigne la valeur du côté droit d’une instruction à l’objet situé à gauche.</span><span class="sxs-lookup"><span data-stu-id="f9639-338">Assigns the value on the right side of a statement to the object on the left side.</span></span>
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
    <span data-ttu-id="f9639-339">Égalité</span><span class="sxs-lookup"><span data-stu-id="f9639-339">Equality.</span></span> <span data-ttu-id="f9639-340">Retourne `true` si les valeurs sont égales.</span><span class="sxs-lookup"><span data-stu-id="f9639-340">Returns `true` if the values are equal.</span></span> <span data-ttu-id="f9639-341">(Notez la distinction entre l’opérateur `=` et l’opérateur `==`.)</span><span class="sxs-lookup"><span data-stu-id="f9639-341">(Notice the distinction between the `=` operator and the `==` operator.)</span></span>
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
    <span data-ttu-id="f9639-342">Inégalité</span><span class="sxs-lookup"><span data-stu-id="f9639-342">Inequality.</span></span> <span data-ttu-id="f9639-343">Retourne `true` si les valeurs ne sont pas égales.</span><span class="sxs-lookup"><span data-stu-id="f9639-343">Returns `true` if the values are not equal.</span></span>
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
    <span data-ttu-id="f9639-344">Inférieur à, supérieur à, inférieur ou égal à, et supérieur à ou égal à égal.</span><span class="sxs-lookup"><span data-stu-id="f9639-344">Less-than, greater-than, less-than-or-equal, and greater-than-or-equal.</span></span>
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
    <span data-ttu-id="f9639-345">Concaténation, qui est utilisée pour joindre des chaînes.</span><span class="sxs-lookup"><span data-stu-id="f9639-345">Concatenation, which is used to join strings.</span></span> <span data-ttu-id="f9639-346">ASP.NET sait la différence entre cet opérateur et l’opérateur d’addition en fonction du type de données de l’expression.</span><span class="sxs-lookup"><span data-stu-id="f9639-346">ASP.NET knows the difference between this operator and the addition operator based on the data type of the expression.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        <span data-ttu-id="f9639-347">`+=` `-=`</span><span class="sxs-lookup"><span data-stu-id="f9639-347">`+=` `-=`</span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="f9639-348">Les opérateurs d’incrémentation et de décrémentation, qui ajoutent et soustraient 1 (respectivement) d’une variable.</span><span class="sxs-lookup"><span data-stu-id="f9639-348">The increment and decrement operators, which add and subtract 1 (respectively) from a variable.</span></span>
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
    <span data-ttu-id="f9639-349">Cédé.</span><span class="sxs-lookup"><span data-stu-id="f9639-349">Dot.</span></span> <span data-ttu-id="f9639-350">Utilisé pour distinguer les objets et leurs propriétés et méthodes.</span><span class="sxs-lookup"><span data-stu-id="f9639-350">Used to distinguish objects and their properties and methods.</span></span>
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
    <span data-ttu-id="f9639-351">Parenthèses.</span><span class="sxs-lookup"><span data-stu-id="f9639-351">Parentheses.</span></span> <span data-ttu-id="f9639-352">Utilisé pour regrouper des expressions et passer des paramètres à des méthodes.</span><span class="sxs-lookup"><span data-stu-id="f9639-352">Used to group expressions and to pass parameters to methods.</span></span>
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
    <span data-ttu-id="f9639-353">Crochets.</span><span class="sxs-lookup"><span data-stu-id="f9639-353">Brackets.</span></span> <span data-ttu-id="f9639-354">Utilisé pour accéder aux valeurs dans les tableaux ou les collections.</span><span class="sxs-lookup"><span data-stu-id="f9639-354">Used for accessing values in arrays or collections.</span></span>
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
    <span data-ttu-id="f9639-355">Pas.</span><span class="sxs-lookup"><span data-stu-id="f9639-355">Not.</span></span> <span data-ttu-id="f9639-356">Inverse une valeur `true` pour `false` et vice versa.</span><span class="sxs-lookup"><span data-stu-id="f9639-356">Reverses a `true` value to `false` and vice versa.</span></span> <span data-ttu-id="f9639-357">Généralement utilisé comme un moyen simple de tester `false` (autrement dit, pour ne pas `true`).</span><span class="sxs-lookup"><span data-stu-id="f9639-357">Typically used as a shorthand way to test for `false` (that is, for not `true`).</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        <span data-ttu-id="f9639-358">`&&` `||`</span><span class="sxs-lookup"><span data-stu-id="f9639-358">`&&` `||`</span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="f9639-359">AND logique and ou, qui sont utilisés pour lier des conditions ensemble.</span><span class="sxs-lookup"><span data-stu-id="f9639-359">Logical AND and OR, which are used to link conditions together.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)]
    :::column-end:::
:::row-end:::

<a id="ID_WorkingWithFileAndFolderPaths"></a>
## <a name="working-with-file-and-folder-paths-in-code"></a><span data-ttu-id="f9639-360">Utilisation des chemins d’accès aux fichiers et aux dossiers dans le code</span><span class="sxs-lookup"><span data-stu-id="f9639-360">Working with File and Folder Paths in Code</span></span>

<span data-ttu-id="f9639-361">Vous travaillez souvent avec les chemins d’accès de fichier et de dossier dans votre code.</span><span class="sxs-lookup"><span data-stu-id="f9639-361">You'll often work with file and folder paths in your code.</span></span> <span data-ttu-id="f9639-362">Voici un exemple de structure de dossiers physiques pour un site Web tel qu’il peut apparaître sur votre ordinateur de développement :</span><span class="sxs-lookup"><span data-stu-id="f9639-362">Here is an example of physical folder structure for a website as it might appear on your development computer:</span></span>

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

<span data-ttu-id="f9639-363">Voici quelques détails essentiels sur les URL et les chemins d’accès :</span><span class="sxs-lookup"><span data-stu-id="f9639-363">Here are some essential details about URLs and paths:</span></span>

- <span data-ttu-id="f9639-364">Une URL commence par un nom de domaine (`http://www.example.com`) ou un nom de serveur (`http://localhost`, `http://mycomputer`).</span><span class="sxs-lookup"><span data-stu-id="f9639-364">A URL begins with either a domain name (`http://www.example.com`) or a server name (`http://localhost`, `http://mycomputer`).</span></span>
- <span data-ttu-id="f9639-365">Une URL correspond à un chemin d’accès physique sur un ordinateur hôte.</span><span class="sxs-lookup"><span data-stu-id="f9639-365">A URL corresponds to a physical path on a host computer.</span></span> <span data-ttu-id="f9639-366">Par exemple, `http://myserver` peut correspondre au dossier *C:\websites\mywebsite* sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="f9639-366">For example, `http://myserver` might correspond to the folder *C:\websites\mywebsite* on the server.</span></span>
- <span data-ttu-id="f9639-367">Un chemin d’accès virtuel est raccourci pour représenter les chemins d’accès dans le code sans avoir à spécifier le chemin d’accès complet.</span><span class="sxs-lookup"><span data-stu-id="f9639-367">A virtual path is shorthand to represent paths in code without having to specify the full path.</span></span> <span data-ttu-id="f9639-368">Il comprend la partie d’une URL qui suit le nom de domaine ou de serveur.</span><span class="sxs-lookup"><span data-stu-id="f9639-368">It includes the portion of a URL that follows the domain or server name.</span></span> <span data-ttu-id="f9639-369">Lorsque vous utilisez des chemins d’accès virtuels, vous pouvez déplacer votre code vers un autre domaine ou serveur sans avoir à mettre à jour les chemins d’accès.</span><span class="sxs-lookup"><span data-stu-id="f9639-369">When you use virtual paths, you can move your code to a different domain or server without having to update the paths.</span></span>

<span data-ttu-id="f9639-370">Voici un exemple pour vous aider à comprendre les différences :</span><span class="sxs-lookup"><span data-stu-id="f9639-370">Here's an example to help you understand the differences:</span></span>

| <span data-ttu-id="f9639-371">URL complète</span><span class="sxs-lookup"><span data-stu-id="f9639-371">Complete URL</span></span> | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| <span data-ttu-id="f9639-372">Nom du serveur</span><span class="sxs-lookup"><span data-stu-id="f9639-372">Server name</span></span> | <span data-ttu-id="f9639-373">*mycompanyserver*</span><span class="sxs-lookup"><span data-stu-id="f9639-373">*mycompanyserver*</span></span> |
| <span data-ttu-id="f9639-374">Chemin d'accès virtuel</span><span class="sxs-lookup"><span data-stu-id="f9639-374">Virtual path</span></span> | <span data-ttu-id="f9639-375">*/humanresources/CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="f9639-375">*/humanresources/CompanyPolicy.htm*</span></span> |
| <span data-ttu-id="f9639-376">Chemin d’accès physique</span><span class="sxs-lookup"><span data-stu-id="f9639-376">Physical path</span></span> | <span data-ttu-id="f9639-377">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="f9639-377">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span></span> |

<span data-ttu-id="f9639-378">La racine virtuelle est/, tout comme la racine de votre lecteur C :.</span><span class="sxs-lookup"><span data-stu-id="f9639-378">The virtual root is /, just like the root of your C: drive is \.</span></span> <span data-ttu-id="f9639-379">(Les chemins d’accès aux dossiers virtuels utilisent toujours des barres obliques.) Le chemin d’accès virtuel d’un dossier ne doit pas nécessairement avoir le même nom que le dossier physique ; Il peut s’agir d’un alias.</span><span class="sxs-lookup"><span data-stu-id="f9639-379">(Virtual folder paths always use forward slashes.) The virtual path of a folder doesn't have to have the same name as the physical folder; it can be an alias.</span></span> <span data-ttu-id="f9639-380">(Sur les serveurs de production, le chemin d’accès virtuel correspond rarement à un chemin d’accès physique exact.)</span><span class="sxs-lookup"><span data-stu-id="f9639-380">(On production servers, the virtual path rarely matches an exact physical path.)</span></span>

<span data-ttu-id="f9639-381">Lorsque vous travaillez avec des fichiers et des dossiers dans le code, vous devez parfois faire référence au chemin d’accès physique et parfois à un chemin d’accès virtuel, selon les objets que vous utilisez.</span><span class="sxs-lookup"><span data-stu-id="f9639-381">When you work with files and folders in code, sometimes you need to reference the physical path and sometimes a virtual path, depending on what objects you're working with.</span></span> <span data-ttu-id="f9639-382">ASP.NET vous offre les outils nécessaires pour utiliser les chemins d’accès aux fichiers et aux dossiers dans le code : la méthode `Server.MapPath`, ainsi que l’opérateur `~` et la méthode `Href`.</span><span class="sxs-lookup"><span data-stu-id="f9639-382">ASP.NET gives you these tools for working with file and folder paths in code: the `Server.MapPath` method, and the `~` operator and `Href` method.</span></span>

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a><span data-ttu-id="f9639-383">Conversion de chemins d’accès virtuels en éléments physiques : méthode Server. MapPath</span><span class="sxs-lookup"><span data-stu-id="f9639-383">Converting virtual to physical paths: the Server.MapPath method</span></span>

<span data-ttu-id="f9639-384">La méthode `Server.MapPath` convertit un chemin d’accès virtuel (par exemple, */default.cshtml*) en chemin d’accès physique absolu (comme *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="f9639-384">The `Server.MapPath` method converts a virtual path (like */default.cshtml*) to an absolute physical path (like *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span></span> <span data-ttu-id="f9639-385">Vous utilisez cette méthode chaque fois que vous avez besoin d’un chemin d’accès physique complet.</span><span class="sxs-lookup"><span data-stu-id="f9639-385">You use this method any time you need a complete physical path.</span></span> <span data-ttu-id="f9639-386">Voici un exemple typique de la lecture ou de l’écriture d’un fichier texte ou d’un fichier image sur le serveur Web.</span><span class="sxs-lookup"><span data-stu-id="f9639-386">A typical example is when you're reading or writing a text file or image file on the web server.</span></span>

<span data-ttu-id="f9639-387">En général, vous ne connaissez pas le chemin d’accès physique absolu de votre site sur le serveur d’un site d’hébergement. cette méthode peut donc convertir le chemin d’accès virtuel (le chemin d’accès virtuel) vers le chemin d’accès correspondant sur le serveur pour vous.</span><span class="sxs-lookup"><span data-stu-id="f9639-387">You typically don't know the absolute physical path of your site on a hosting site's server, so this method can convert the path you do know — the virtual path — to the corresponding path on the server for you.</span></span> <span data-ttu-id="f9639-388">Vous transmettez le chemin d’accès virtuel d’un fichier ou d’un dossier à la méthode et il retourne le chemin d’accès physique :</span><span class="sxs-lookup"><span data-stu-id="f9639-388">You pass the virtual path to a file or folder to the method, and it returns the physical path:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a><span data-ttu-id="f9639-389">Référencement de la racine virtuelle : l’opérateur ~ et la méthode href</span><span class="sxs-lookup"><span data-stu-id="f9639-389">Referencing the virtual root: the ~ operator and Href method</span></span>

<span data-ttu-id="f9639-390">Dans un fichier *. cshtml* ou *. vbhtml* , vous pouvez référencer le chemin d’accès de la racine virtuelle à l’aide de l’opérateur `~`.</span><span class="sxs-lookup"><span data-stu-id="f9639-390">In a *.cshtml* or *.vbhtml* file, you can reference the virtual root path using the `~` operator.</span></span> <span data-ttu-id="f9639-391">C’est très pratique, car vous pouvez déplacer des pages dans un site et tous les liens qu’ils contiennent vers d’autres pages ne sont pas rompus.</span><span class="sxs-lookup"><span data-stu-id="f9639-391">This is very handy because you can move pages around in a site, and any links they contain to other pages won't be broken.</span></span> <span data-ttu-id="f9639-392">C’est également pratique si vous déplacez votre site Web vers un autre emplacement.</span><span class="sxs-lookup"><span data-stu-id="f9639-392">It's also handy in case you ever move your website to a different location.</span></span> <span data-ttu-id="f9639-393">Voici quelques exemples :</span><span class="sxs-lookup"><span data-stu-id="f9639-393">Here are some examples:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

<span data-ttu-id="f9639-394">Si le site Web est `http://myserver/myapp`, voici comment ASP.NET traite ces chemins lors de l’exécution de la page :</span><span class="sxs-lookup"><span data-stu-id="f9639-394">If the website is `http://myserver/myapp`, here's how ASP.NET will treat these paths when the page runs:</span></span>

- <span data-ttu-id="f9639-395">`myImagesFolder`: `http://myserver/myapp/images`</span><span class="sxs-lookup"><span data-stu-id="f9639-395">`myImagesFolder`: `http://myserver/myapp/images`</span></span>
- <span data-ttu-id="f9639-396">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span><span class="sxs-lookup"><span data-stu-id="f9639-396">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span></span>

<span data-ttu-id="f9639-397">(Vous ne verrez pas ces chemins comme valeurs de la variable, mais ASP.NET traitera les chemins comme s’ils étaient.)</span><span class="sxs-lookup"><span data-stu-id="f9639-397">(You won't actually see these paths as the values of the variable, but ASP.NET will treat the paths as if that's what they were.)</span></span>

<span data-ttu-id="f9639-398">Vous pouvez utiliser l’opérateur `~` dans le code serveur (comme ci-dessus) et dans le balisage, comme suit :</span><span class="sxs-lookup"><span data-stu-id="f9639-398">You can use the `~` operator both in server code (as above) and in markup, like this:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

<span data-ttu-id="f9639-399">Dans le balisage, vous utilisez l’opérateur `~` pour créer des chemins d’accès à des ressources telles que des fichiers image, d’autres pages Web et des fichiers CSS.</span><span class="sxs-lookup"><span data-stu-id="f9639-399">In markup, you use the `~` operator to create paths to resources like image files, other web pages, and CSS files.</span></span> <span data-ttu-id="f9639-400">Lorsque la page s’exécute, ASP.NET parcourt la page (code et balisage) et résout toutes les références `~` au chemin d’accès approprié.</span><span class="sxs-lookup"><span data-stu-id="f9639-400">When the page runs, ASP.NET looks through the page (both code and markup) and resolves all the `~` references to the appropriate path.</span></span>

## <a name="conditional-logic-and-loops"></a><span data-ttu-id="f9639-401">Logique conditionnelle et boucles</span><span class="sxs-lookup"><span data-stu-id="f9639-401">Conditional Logic and Loops</span></span>

<span data-ttu-id="f9639-402">ASP.NET Server Code vous permet d’effectuer des tâches en fonction de conditions et d’écrire du code qui répète des instructions un nombre spécifique de fois (autrement dit, le code qui exécute une boucle).</span><span class="sxs-lookup"><span data-stu-id="f9639-402">ASP.NET server code lets you perform tasks based on conditions and write code that repeats statements a specific number of times (that is, code that runs a loop).</span></span>

### <a name="testing-conditions"></a><span data-ttu-id="f9639-403">Conditions de test</span><span class="sxs-lookup"><span data-stu-id="f9639-403">Testing Conditions</span></span>

<span data-ttu-id="f9639-404">Pour tester une condition simple, vous utilisez l’instruction `if`, qui retourne true ou false en fonction d’un test que vous spécifiez :</span><span class="sxs-lookup"><span data-stu-id="f9639-404">To test a simple condition you use the `if` statement, which returns true or false based on a test you specify:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

<span data-ttu-id="f9639-405">Le mot clé `if` démarre un bloc.</span><span class="sxs-lookup"><span data-stu-id="f9639-405">The `if` keyword starts a block.</span></span> <span data-ttu-id="f9639-406">Le test réel (condition) est entre parenthèses et retourne true ou false.</span><span class="sxs-lookup"><span data-stu-id="f9639-406">The actual test (condition) is in parentheses and returns true or false.</span></span> <span data-ttu-id="f9639-407">Les instructions qui s’exécutent si le test a la valeur true sont placées entre accolades.</span><span class="sxs-lookup"><span data-stu-id="f9639-407">The statements that run if the test is true are enclosed in braces.</span></span> <span data-ttu-id="f9639-408">Une instruction `if` peut inclure un bloc `else` qui spécifie les instructions à exécuter si la condition a la valeur false :</span><span class="sxs-lookup"><span data-stu-id="f9639-408">An `if` statement can include an `else` block that specifies statements to run if the condition is false:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

<span data-ttu-id="f9639-409">Vous pouvez ajouter plusieurs conditions à l’aide d’un bloc de `else if` :</span><span class="sxs-lookup"><span data-stu-id="f9639-409">You can add multiple conditions using an `else if` block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

<span data-ttu-id="f9639-410">Dans cet exemple, si la première condition du bloc If n’est pas true, la condition `else if` est cochée.</span><span class="sxs-lookup"><span data-stu-id="f9639-410">In this example, if the first condition in the if block is not true, the `else if` condition is checked.</span></span> <span data-ttu-id="f9639-411">Si cette condition est remplie, les instructions du bloc `else if` sont exécutées.</span><span class="sxs-lookup"><span data-stu-id="f9639-411">If that condition is met, the statements in the `else if` block are executed.</span></span> <span data-ttu-id="f9639-412">Si aucune des conditions n’est remplie, les instructions du bloc `else` sont exécutées.</span><span class="sxs-lookup"><span data-stu-id="f9639-412">If none of the conditions are met, the statements in the `else` block are executed.</span></span> <span data-ttu-id="f9639-413">Vous pouvez ajouter un nombre quelconque d’autres blocs if, puis fermer avec un bloc `else` comme &quot;tout le reste&quot; condition.</span><span class="sxs-lookup"><span data-stu-id="f9639-413">You can add any number of else if blocks, and then close with an `else` block as the &quot;everything else&quot; condition.</span></span>

<span data-ttu-id="f9639-414">Pour tester un grand nombre de conditions, utilisez un bloc `switch` :</span><span class="sxs-lookup"><span data-stu-id="f9639-414">To test a large number of conditions, use a `switch` block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

<span data-ttu-id="f9639-415">La valeur à tester est entre parenthèses (dans l’exemple, la variable `weekday`).</span><span class="sxs-lookup"><span data-stu-id="f9639-415">The value to test is in parentheses (in the example, the `weekday` variable).</span></span> <span data-ttu-id="f9639-416">Chaque test individuel utilise une instruction `case` qui se termine par un signe deux-points ( :).</span><span class="sxs-lookup"><span data-stu-id="f9639-416">Each individual test uses a `case` statement that ends with a colon (:).</span></span> <span data-ttu-id="f9639-417">Si la valeur d’une instruction `case` correspond à la valeur de test, le code dans ce bloc case est exécuté.</span><span class="sxs-lookup"><span data-stu-id="f9639-417">If the value of a `case` statement matches the test value, the code in that case block is executed.</span></span> <span data-ttu-id="f9639-418">Vous fermez chaque instruction case avec une instruction `break`.</span><span class="sxs-lookup"><span data-stu-id="f9639-418">You close each case statement with a `break` statement.</span></span> <span data-ttu-id="f9639-419">(Si vous oubliez d’inclure Break dans chaque bloc `case`, le code de l’instruction `case` suivante s’exécutera également.) Un bloc de `switch` a souvent une instruction `default` comme dernier cas pour une option &quot;tout le reste&quot; qui s’exécute si aucun des autres cas n’a la valeur true.</span><span class="sxs-lookup"><span data-stu-id="f9639-419">(If you forget to include break in each `case` block, the code from the next `case` statement will run also.) A `switch` block often has a `default` statement as the last case for an &quot;everything else&quot; option that runs if none of the other cases are true.</span></span>

<span data-ttu-id="f9639-420">Résultat des deux derniers blocs conditionnels affichés dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="f9639-420">The result of the last two conditional blocks displayed in a browser:</span></span>

![Rasoir-Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a><span data-ttu-id="f9639-422">Code en boucle</span><span class="sxs-lookup"><span data-stu-id="f9639-422">Looping Code</span></span>

<span data-ttu-id="f9639-423">Vous devez souvent exécuter les mêmes instructions à plusieurs reprises.</span><span class="sxs-lookup"><span data-stu-id="f9639-423">You often need to run the same statements repeatedly.</span></span> <span data-ttu-id="f9639-424">Pour ce faire, vous devez effectuer une boucle.</span><span class="sxs-lookup"><span data-stu-id="f9639-424">You do this by looping.</span></span> <span data-ttu-id="f9639-425">Par exemple, vous exécutez souvent les mêmes instructions pour chaque élément d’une collection de données.</span><span class="sxs-lookup"><span data-stu-id="f9639-425">For example, you often run the same statements for each item in a collection of data.</span></span> <span data-ttu-id="f9639-426">Si vous savez exactement combien de fois vous souhaitez effectuer une boucle, vous pouvez utiliser une boucle `for`.</span><span class="sxs-lookup"><span data-stu-id="f9639-426">If you know exactly how many times you want to loop, you can use a `for` loop.</span></span> <span data-ttu-id="f9639-427">Ce type de boucle est particulièrement utile pour le comptage ou le comptage :</span><span class="sxs-lookup"><span data-stu-id="f9639-427">This kind of loop is especially useful for counting up or counting down:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

<span data-ttu-id="f9639-428">La boucle commence par le mot clé `for`, suivi de trois instructions entre parenthèses, chacune se terminant par un point-virgule.</span><span class="sxs-lookup"><span data-stu-id="f9639-428">The loop begins with the `for` keyword, followed by three statements in parentheses, each terminated with a semicolon.</span></span>

- <span data-ttu-id="f9639-429">À l’intérieur des parenthèses, la première instruction (`var i=10;`) crée un compteur et l’initialise à 10.</span><span class="sxs-lookup"><span data-stu-id="f9639-429">Inside the parentheses, the first statement (`var i=10;`) creates a counter and initializes it to 10.</span></span> <span data-ttu-id="f9639-430">Vous n’avez pas besoin de nommer le &#8212; compteur `i` vous pouvez utiliser n’importe quelle variable.</span><span class="sxs-lookup"><span data-stu-id="f9639-430">You don't have to name the counter `i` &#8212; you can use any variable.</span></span> <span data-ttu-id="f9639-431">Lors de l’exécution de la boucle `for`, le compteur est incrémenté automatiquement.</span><span class="sxs-lookup"><span data-stu-id="f9639-431">When the `for` loop runs, the counter is automatically incremented.</span></span>
- <span data-ttu-id="f9639-432">La deuxième instruction (`i < 21;`) définit la condition pour le nombre de fois que vous souhaitez compter.</span><span class="sxs-lookup"><span data-stu-id="f9639-432">The second statement (`i < 21;`) sets the condition for how far you want to count.</span></span> <span data-ttu-id="f9639-433">Dans ce cas, vous souhaitez qu’il passe à un maximum de 20 (autrement dit, continuez alors que le compteur est inférieur à 21).</span><span class="sxs-lookup"><span data-stu-id="f9639-433">In this case, you want it to go to a maximum of 20 (that is, keep going while the counter is less than 21).</span></span>
- <span data-ttu-id="f9639-434">La troisième instruction (`i++`) utilise un opérateur d’incrément, qui spécifie simplement que la valeur 1 a été ajoutée au compteur chaque fois que la boucle s’exécute.</span><span class="sxs-lookup"><span data-stu-id="f9639-434">The third statement (`i++` ) uses an increment operator, which simply specifies that the counter should have 1 added to it each time the loop runs.</span></span>

<span data-ttu-id="f9639-435">À l’intérieur des accolades se trouve le code qui s’exécute pour chaque itération de la boucle.</span><span class="sxs-lookup"><span data-stu-id="f9639-435">Inside the braces is the code that will run for each iteration of the loop.</span></span> <span data-ttu-id="f9639-436">Le balisage crée chaque fois un nouveau paragraphe (`<p>` élément) et ajoute une ligne à la sortie, en affichant la valeur de `i` (le compteur).</span><span class="sxs-lookup"><span data-stu-id="f9639-436">The markup creates a new paragraph (`<p>` element) each time and adds a line to the output, displaying the value of `i` (the counter).</span></span> <span data-ttu-id="f9639-437">Lorsque vous exécutez cette page, l’exemple crée 11 lignes affichant la sortie, avec le texte de chaque ligne indiquant le numéro de l’élément.</span><span class="sxs-lookup"><span data-stu-id="f9639-437">When you run this page, the example creates 11 lines displaying the output, with the text in each line indicating the item number.</span></span>

![Razor-Img11](introducing-razor-syntax-c/_static/image11.jpg)

<span data-ttu-id="f9639-439">Si vous utilisez une collection ou un tableau, vous utilisez souvent une boucle `foreach`.</span><span class="sxs-lookup"><span data-stu-id="f9639-439">If you're working with a collection or array, you often use a `foreach` loop.</span></span> <span data-ttu-id="f9639-440">Une collection est un groupe d’objets similaires, et la boucle `foreach` vous permet d’effectuer une tâche sur chaque élément de la collection.</span><span class="sxs-lookup"><span data-stu-id="f9639-440">A collection is a group of similar objects, and the `foreach` loop lets you carry out a task on each item in the collection.</span></span> <span data-ttu-id="f9639-441">Ce type de boucle est pratique pour les collections, car contrairement à une boucle `for`, il n’est pas nécessaire d’incrémenter le compteur ou de définir une limite.</span><span class="sxs-lookup"><span data-stu-id="f9639-441">This type of loop is convenient for collections, because unlike a `for` loop, you don't have to increment the counter or set a limit.</span></span> <span data-ttu-id="f9639-442">Au lieu de cela, le `foreach` code de boucle passe simplement par la collection jusqu’à ce qu’il soit terminé.</span><span class="sxs-lookup"><span data-stu-id="f9639-442">Instead, the `foreach` loop code simply proceeds through the collection until it's finished.</span></span>

<span data-ttu-id="f9639-443">Par exemple, le code suivant retourne les éléments de la collection `Request.ServerVariables`, qui est un objet qui contient des informations sur votre serveur Web.</span><span class="sxs-lookup"><span data-stu-id="f9639-443">For example, the following code returns the items in the `Request.ServerVariables` collection, which is an object that contains information about your web server.</span></span> <span data-ttu-id="f9639-444">Elle utilise une boucle `foreac` h pour afficher le nom de chaque élément en créant un nouvel élément `<li>` dans une liste à puces HTML.</span><span class="sxs-lookup"><span data-stu-id="f9639-444">It uses a `foreac` h loop to display the name of each item by creating a new `<li>` element in an HTML bulleted list.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

<span data-ttu-id="f9639-445">Le mot clé `foreach` est suivi de parenthèses où vous déclarez une variable qui représente un élément unique dans la collection (dans l’exemple, `var item`), suivi du mot clé `in`, suivi de la collection dans laquelle vous souhaitez effectuer une boucle.</span><span class="sxs-lookup"><span data-stu-id="f9639-445">The `foreach` keyword is followed by parentheses where you declare a variable that represents a single item in the collection (in the example, `var item`), followed by the `in` keyword, followed by the collection you want to loop through.</span></span> <span data-ttu-id="f9639-446">Dans le corps de la boucle `foreach`, vous pouvez accéder à l’élément actuel à l’aide de la variable que vous avez déclarée précédemment.</span><span class="sxs-lookup"><span data-stu-id="f9639-446">In the body of the `foreach` loop, you can access the current item using the variable that you declared earlier.</span></span>

![Rasoir-Img12](introducing-razor-syntax-c/_static/image12.jpg)

<span data-ttu-id="f9639-448">Pour créer une boucle plus générale, utilisez l’instruction `while` :</span><span class="sxs-lookup"><span data-stu-id="f9639-448">To create a more general-purpose loop, use the `while` statement:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

<span data-ttu-id="f9639-449">Une boucle `while` commence par le mot clé `while`, suivi de parenthèses où vous spécifiez la durée de la boucle (ici, pendant tant que `countNum` est inférieure à 50), puis le bloc à répéter.</span><span class="sxs-lookup"><span data-stu-id="f9639-449">A `while` loop begins with the `while` keyword, followed by parentheses where you specify how long the loop continues (here, for as long as `countNum` is less than 50), then the block to repeat.</span></span> <span data-ttu-id="f9639-450">En général, les boucles incrémentent (ajoutent à) ou décrémente (soustrait de) une variable ou un objet utilisé pour le comptage.</span><span class="sxs-lookup"><span data-stu-id="f9639-450">Loops typically increment (add to) or decrement (subtract from) a variable or object used for counting.</span></span> <span data-ttu-id="f9639-451">Dans l’exemple, l’opérateur `+=` ajoute 1 à `countNum` à chaque exécution de la boucle.</span><span class="sxs-lookup"><span data-stu-id="f9639-451">In the example, the `+=` operator adds 1 to `countNum` each time the loop runs.</span></span> <span data-ttu-id="f9639-452">(Pour décrémenter une variable dans une boucle qui est décomptée, vous devez utiliser l’opérateur de décrémentation `-=`).</span><span class="sxs-lookup"><span data-stu-id="f9639-452">(To decrement a variable in a loop that counts down, you would use the decrement operator `-=`).</span></span>

## <a name="objects-and-collections"></a><span data-ttu-id="f9639-453">Objets et collections</span><span class="sxs-lookup"><span data-stu-id="f9639-453">Objects and Collections</span></span>

<span data-ttu-id="f9639-454">Presque tout ce qui se trouve dans un site Web ASP.NET est un objet, y compris la page Web elle-même.</span><span class="sxs-lookup"><span data-stu-id="f9639-454">Nearly everything in an ASP.NET website is an object, including the web page itself.</span></span> <span data-ttu-id="f9639-455">Cette section décrit certains objets importants que vous allez utiliser fréquemment dans votre code.</span><span class="sxs-lookup"><span data-stu-id="f9639-455">This section discusses some important objects you'll work with frequently in your code.</span></span>

### <a name="page-objects"></a><span data-ttu-id="f9639-456">Objets page</span><span class="sxs-lookup"><span data-stu-id="f9639-456">Page Objects</span></span>

<span data-ttu-id="f9639-457">L’objet le plus basique dans ASP.NET est la page.</span><span class="sxs-lookup"><span data-stu-id="f9639-457">The most basic object in ASP.NET is the page.</span></span> <span data-ttu-id="f9639-458">Vous pouvez accéder aux propriétés de l’objet page directement sans aucun objet éligible.</span><span class="sxs-lookup"><span data-stu-id="f9639-458">You can access properties of the page object directly without any qualifying object.</span></span> <span data-ttu-id="f9639-459">Le code suivant obtient le chemin d’accès au fichier de la page, à l’aide de l’objet `Request` de la page :</span><span class="sxs-lookup"><span data-stu-id="f9639-459">The following code gets the page's file path, using the `Request` object of the page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

<span data-ttu-id="f9639-460">Pour indiquer clairement que vous référencez des propriétés et des méthodes sur l’objet de page actuel, vous pouvez éventuellement utiliser le mot clé `this` pour représenter l’objet page dans votre code.</span><span class="sxs-lookup"><span data-stu-id="f9639-460">To make it clear that you're referencing properties and methods on the current page object, you can optionally use the keyword `this` to represent the page object in your code.</span></span> <span data-ttu-id="f9639-461">Voici l’exemple de code précédent, avec `this` ajoutée pour représenter la page :</span><span class="sxs-lookup"><span data-stu-id="f9639-461">Here is the previous code example, with `this` added to represent the page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

<span data-ttu-id="f9639-462">Vous pouvez utiliser les propriétés de l’objet `Page` pour obtenir un grand nombre d’informations, telles que :</span><span class="sxs-lookup"><span data-stu-id="f9639-462">You can use properties of the `Page` object to get a lot of information, such as:</span></span>

- <span data-ttu-id="f9639-463">`Request`.</span><span class="sxs-lookup"><span data-stu-id="f9639-463">`Request`.</span></span> <span data-ttu-id="f9639-464">Comme vous l’avez déjà vu, il s’agit d’une collection d’informations sur la requête actuelle, notamment le type de navigateur qui a effectué la demande, l’URL de la page, l’identité de l’utilisateur, etc.</span><span class="sxs-lookup"><span data-stu-id="f9639-464">As you've already seen, this is a collection of information about the current request, including what type of browser made the request, the URL of the page, the user identity, etc.</span></span>
- <span data-ttu-id="f9639-465">`Response`.</span><span class="sxs-lookup"><span data-stu-id="f9639-465">`Response`.</span></span> <span data-ttu-id="f9639-466">Il s’agit d’une collection d’informations sur la réponse (page) qui seront envoyées au navigateur une fois l’exécution du code serveur terminée.</span><span class="sxs-lookup"><span data-stu-id="f9639-466">This is a collection of information about the response (page) that will be sent to the browser when the server code has finished running.</span></span> <span data-ttu-id="f9639-467">Par exemple, vous pouvez utiliser cette propriété pour écrire des informations dans la réponse.</span><span class="sxs-lookup"><span data-stu-id="f9639-467">For example, you can use this property to write information into the response.</span></span> 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a><span data-ttu-id="f9639-468">Objets de collection (tableaux et dictionnaires)</span><span class="sxs-lookup"><span data-stu-id="f9639-468">Collection Objects (Arrays and Dictionaries)</span></span>

<span data-ttu-id="f9639-469">Une *collection* est un groupe d’objets du même type, par exemple une collection d’objets `Customer` à partir d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="f9639-469">A *collection* is a group of objects of the same type, such as a collection of `Customer` objects from a database.</span></span> <span data-ttu-id="f9639-470">ASP.NET contient de nombreuses collections intégrées, telles que la collection `Request.Files`.</span><span class="sxs-lookup"><span data-stu-id="f9639-470">ASP.NET contains many built-in collections, like the `Request.Files` collection.</span></span>

<span data-ttu-id="f9639-471">Vous utiliserez souvent des données dans des collections.</span><span class="sxs-lookup"><span data-stu-id="f9639-471">You'll often work with data in collections.</span></span> <span data-ttu-id="f9639-472">Deux types de collections courants sont le *tableau* et le *dictionnaire*.</span><span class="sxs-lookup"><span data-stu-id="f9639-472">Two common collection types are the *array* and the *dictionary*.</span></span> <span data-ttu-id="f9639-473">Un tableau est utile lorsque vous souhaitez stocker une collection d’éléments similaires, mais que vous ne voulez pas créer une variable distincte pour contenir chaque élément :</span><span class="sxs-lookup"><span data-stu-id="f9639-473">An array is useful when you want to store a collection of similar items but don't want to create a separate variable to hold each item:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

<span data-ttu-id="f9639-474">Avec les tableaux, vous déclarez un type de données spécifique, tel que `string`, `int`ou `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="f9639-474">With arrays, you declare a specific data type, such as `string`, `int`, or `DateTime`.</span></span> <span data-ttu-id="f9639-475">Pour indiquer que la variable peut contenir un tableau, vous ajoutez des crochets à la déclaration (par exemple, `string[]` ou `int[]`).</span><span class="sxs-lookup"><span data-stu-id="f9639-475">To indicate that the variable can contain an array, you add brackets to the declaration (such as `string[]` or `int[]`).</span></span> <span data-ttu-id="f9639-476">Vous pouvez accéder aux éléments d’un tableau à l’aide de leur position (index) ou à l’aide de l’instruction `foreach`.</span><span class="sxs-lookup"><span data-stu-id="f9639-476">You can access items in an array using their position (index) or by using the `foreach` statement.</span></span> <span data-ttu-id="f9639-477">Les index de tableau sont de base &#8212; zéro, c’est-à-dire que le premier élément est à la position 0, le deuxième à la position 1, et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="f9639-477">Array indexes are zero-based &#8212; that is, the first item is at position 0, the second item is at position 1, and so on.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

<span data-ttu-id="f9639-478">Vous pouvez déterminer le nombre d’éléments d’un tableau en obtenant sa propriété `Length`.</span><span class="sxs-lookup"><span data-stu-id="f9639-478">You can determine the number of items in an array by getting its `Length` property.</span></span> <span data-ttu-id="f9639-479">Pour obtenir la position d’un élément spécifique dans le tableau (pour effectuer une recherche dans le tableau), utilisez la méthode `Array.IndexOf`.</span><span class="sxs-lookup"><span data-stu-id="f9639-479">To get the position of a specific item in the array (to search the array), use the `Array.IndexOf` method.</span></span> <span data-ttu-id="f9639-480">Vous pouvez également effectuer des opérations telles que inverser le contenu d’un tableau (méthode `Array.Reverse`) ou trier le contenu (méthode `Array.Sort`).</span><span class="sxs-lookup"><span data-stu-id="f9639-480">You can also do things like reverse the contents of an array (the `Array.Reverse` method) or sort the contents (the `Array.Sort` method).</span></span>

<span data-ttu-id="f9639-481">Sortie du code du tableau de chaînes affiché dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="f9639-481">The output of the string array code displayed in a browser:</span></span>

![Rasoir-Img13](introducing-razor-syntax-c/_static/image13.jpg)

<span data-ttu-id="f9639-483">Un dictionnaire est une collection de paires clé/valeur, dans laquelle vous fournissez la clé (ou le nom) pour définir ou récupérer la valeur correspondante :</span><span class="sxs-lookup"><span data-stu-id="f9639-483">A dictionary is a collection of key/value pairs, where you provide the key (or name) to set or retrieve the corresponding value:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

<span data-ttu-id="f9639-484">Pour créer un dictionnaire, vous utilisez le mot clé `new` pour indiquer que vous créez un nouvel objet dictionnaire.</span><span class="sxs-lookup"><span data-stu-id="f9639-484">To create a dictionary, you use the `new` keyword to indicate that you're creating a new dictionary object.</span></span> <span data-ttu-id="f9639-485">Vous pouvez assigner un dictionnaire à une variable à l’aide du mot clé `var`.</span><span class="sxs-lookup"><span data-stu-id="f9639-485">You can assign a dictionary to a variable using the `var` keyword.</span></span> <span data-ttu-id="f9639-486">Vous indiquez les types de données des éléments dans le dictionnaire à l’aide de crochets pointus (`< >`).</span><span class="sxs-lookup"><span data-stu-id="f9639-486">You indicate the data types of the items in the dictionary using angle brackets ( `< >` ).</span></span> <span data-ttu-id="f9639-487">À la fin de la déclaration, vous devez ajouter une paire de parenthèses, car il s’agit en fait d’une méthode qui crée un nouveau dictionnaire.</span><span class="sxs-lookup"><span data-stu-id="f9639-487">At the end of the declaration, you must add a pair of parentheses, because this is actually a method that creates a new dictionary.</span></span>

<span data-ttu-id="f9639-488">Pour ajouter des éléments au dictionnaire, vous pouvez appeler la méthode `Add` de la variable dictionnaire (`myScores` dans ce cas), puis spécifier une clé et une valeur.</span><span class="sxs-lookup"><span data-stu-id="f9639-488">To add items to the dictionary, you can call the `Add` method of the dictionary variable (`myScores` in this case), and then specify a key and a value.</span></span> <span data-ttu-id="f9639-489">Vous pouvez également utiliser des crochets pour indiquer la clé et effectuer une assignation simple, comme dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="f9639-489">Alternatively, you can use square brackets to indicate the key and do a simple assignment, as in the following example:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

<span data-ttu-id="f9639-490">Pour obtenir une valeur à partir du dictionnaire, vous spécifiez la clé entre crochets :</span><span class="sxs-lookup"><span data-stu-id="f9639-490">To get a value from the dictionary, you specify the key in brackets:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a><span data-ttu-id="f9639-491">Appel de méthodes avec des paramètres</span><span class="sxs-lookup"><span data-stu-id="f9639-491">Calling Methods with Parameters</span></span>

<span data-ttu-id="f9639-492">Comme vous l’avez lu plus haut dans cet article, les objets que vous programmez peuvent avoir des méthodes.</span><span class="sxs-lookup"><span data-stu-id="f9639-492">As you read earlier in this article, the objects that you program with can have methods.</span></span> <span data-ttu-id="f9639-493">Par exemple, un objet `Database` peut avoir une méthode `Database.Connect`.</span><span class="sxs-lookup"><span data-stu-id="f9639-493">For example, a `Database` object might have a `Database.Connect` method.</span></span> <span data-ttu-id="f9639-494">De nombreuses méthodes possèdent également un ou plusieurs paramètres.</span><span class="sxs-lookup"><span data-stu-id="f9639-494">Many methods also have one or more parameters.</span></span> <span data-ttu-id="f9639-495">Un *paramètre* est une valeur que vous transmettez à une méthode pour permettre à la méthode d’effectuer sa tâche.</span><span class="sxs-lookup"><span data-stu-id="f9639-495">A *parameter* is a value that you pass to a method to enable the method to complete its task.</span></span> <span data-ttu-id="f9639-496">Par exemple, examinez une déclaration pour la méthode `Request.MapPath`, qui accepte trois paramètres :</span><span class="sxs-lookup"><span data-stu-id="f9639-496">For example, look at a declaration for the `Request.MapPath` method, which takes three parameters:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

<span data-ttu-id="f9639-497">(La ligne a été encapsulée pour le rendre plus lisible.</span><span class="sxs-lookup"><span data-stu-id="f9639-497">(The line has been wrapped to make it more readable.</span></span> <span data-ttu-id="f9639-498">N’oubliez pas que vous pouvez placer des sauts de ligne presque n’importe quel endroit, sauf dans les chaînes placées entre guillemets.)</span><span class="sxs-lookup"><span data-stu-id="f9639-498">Remember that you can put line breaks almost any place except inside strings that are enclosed in quotation marks.)</span></span>

<span data-ttu-id="f9639-499">Cette méthode retourne le chemin d’accès physique sur le serveur qui correspond à un chemin d’accès virtuel spécifié.</span><span class="sxs-lookup"><span data-stu-id="f9639-499">This method returns the physical path on the server that corresponds to a specified virtual path.</span></span> <span data-ttu-id="f9639-500">Les trois paramètres de la méthode sont `virtualPath`, `baseVirtualDir`et `allowCrossAppMapping`.</span><span class="sxs-lookup"><span data-stu-id="f9639-500">The three parameters for the method are `virtualPath`, `baseVirtualDir`, and `allowCrossAppMapping`.</span></span> <span data-ttu-id="f9639-501">(Notez que dans la déclaration, les paramètres sont répertoriés avec les types de données des données qu’ils accepteront.) Lorsque vous appelez cette méthode, vous devez fournir des valeurs pour les trois paramètres.</span><span class="sxs-lookup"><span data-stu-id="f9639-501">(Notice that in the declaration, the parameters are listed with the data types of the data that they'll accept.) When you call this method, you must supply values for all three parameters.</span></span>

<span data-ttu-id="f9639-502">L’syntaxe Razor vous offre deux options pour passer des paramètres à une méthode : les *paramètres positionnels* et les *paramètres nommés*.</span><span class="sxs-lookup"><span data-stu-id="f9639-502">The Razor syntax gives you two options for passing parameters to a method: *positional parameters* and *named parameters*.</span></span> <span data-ttu-id="f9639-503">Pour appeler une méthode à l’aide de paramètres positionnels, vous devez passer les paramètres dans un ordre strict spécifié dans la déclaration de méthode.</span><span class="sxs-lookup"><span data-stu-id="f9639-503">To call a method using positional parameters, you pass the parameters in a strict order that's specified in the method declaration.</span></span> <span data-ttu-id="f9639-504">(Vous connaissez généralement cet ordre en lisant la documentation de la méthode.) Vous devez suivre l’ordre et vous ne pouvez pas ignorer les paramètres &#8212; si nécessaire, vous transmettez une chaîne vide (`""`) ou `null` pour un paramètre positionnel dont vous n’avez pas de valeur.</span><span class="sxs-lookup"><span data-stu-id="f9639-504">(You would typically know this order by reading documentation for the method.) You must follow the order, and you can't skip any of the parameters &#8212; if necessary, you pass an empty string (`""`) or `null` for a positional parameter that you don't have a value for.</span></span>

<span data-ttu-id="f9639-505">L’exemple suivant suppose que vous avez un dossier nommé *scripts* sur votre site Web.</span><span class="sxs-lookup"><span data-stu-id="f9639-505">The following example assumes you have a folder named *scripts* on your website.</span></span> <span data-ttu-id="f9639-506">Le code appelle la méthode `Request.MapPath` et passe des valeurs pour les trois paramètres dans le bon ordre.</span><span class="sxs-lookup"><span data-stu-id="f9639-506">The code calls the `Request.MapPath` method and passes values for the three parameters in the correct order.</span></span> <span data-ttu-id="f9639-507">Il affiche ensuite le chemin d’accès mappé résultant.</span><span class="sxs-lookup"><span data-stu-id="f9639-507">It then displays the resulting mapped path.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

<span data-ttu-id="f9639-508">Quand une méthode a de nombreux paramètres, vous pouvez rendre votre code plus lisible en utilisant des paramètres nommés.</span><span class="sxs-lookup"><span data-stu-id="f9639-508">When a method has many parameters, you can keep your code more readable by using named parameters.</span></span> <span data-ttu-id="f9639-509">Pour appeler une méthode à l’aide de paramètres nommés, vous spécifiez le nom du paramètre suivi d’un signe deux-points ( :), puis de la valeur.</span><span class="sxs-lookup"><span data-stu-id="f9639-509">To call a method using named parameters, you specify the parameter name followed by a colon (:), and then the value.</span></span> <span data-ttu-id="f9639-510">L’avantage des paramètres nommés est que vous pouvez les passer dans l’ordre de votre choix.</span><span class="sxs-lookup"><span data-stu-id="f9639-510">The advantage of named parameters is that you can pass them in any order you want.</span></span> <span data-ttu-id="f9639-511">(Un inconvénient est que l’appel de la méthode n’est pas aussi compact.)</span><span class="sxs-lookup"><span data-stu-id="f9639-511">(A disadvantage is that the method call is not as compact.)</span></span>

<span data-ttu-id="f9639-512">L’exemple suivant appelle la même méthode que ci-dessus, mais utilise des paramètres nommés pour fournir les valeurs :</span><span class="sxs-lookup"><span data-stu-id="f9639-512">The following example calls the same method as above, but uses named parameters to supply the values:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

<span data-ttu-id="f9639-513">Comme vous pouvez le voir, les paramètres sont passés dans un ordre différent.</span><span class="sxs-lookup"><span data-stu-id="f9639-513">As you can see, the parameters are passed in a different order.</span></span> <span data-ttu-id="f9639-514">Toutefois, si vous exécutez l’exemple précédent et cet exemple, ils retournent la même valeur.</span><span class="sxs-lookup"><span data-stu-id="f9639-514">However, if you run the previous example and this example, they'll return the same value.</span></span>

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a><span data-ttu-id="f9639-515">Gestion des erreurs</span><span class="sxs-lookup"><span data-stu-id="f9639-515">Handling Errors</span></span>

### <a name="try-catch-statements"></a><span data-ttu-id="f9639-516">Instructions Try-Catch</span><span class="sxs-lookup"><span data-stu-id="f9639-516">Try-Catch Statements</span></span>

<span data-ttu-id="f9639-517">Dans votre code, vous aurez souvent des instructions qui peuvent échouer pour des raisons extérieures à votre contrôle.</span><span class="sxs-lookup"><span data-stu-id="f9639-517">You'll often have statements in your code that might fail for reasons outside your control.</span></span> <span data-ttu-id="f9639-518">Exemple :</span><span class="sxs-lookup"><span data-stu-id="f9639-518">For example:</span></span>

- <span data-ttu-id="f9639-519">Si votre code essaie de créer ou d’accéder à un fichier, toutes sortes d’erreurs peuvent se produire.</span><span class="sxs-lookup"><span data-stu-id="f9639-519">If your code tries to create or access a file, all sorts of errors might occur.</span></span> <span data-ttu-id="f9639-520">Le fichier que vous souhaitez peut-être n’existe pas, il est peut-être verrouillé, le code n’a peut-être pas d’autorisations, et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="f9639-520">The file you want might not exist, it might be locked, the code might not have permissions, and so on.</span></span>
- <span data-ttu-id="f9639-521">De même, si votre code tente de mettre à jour des enregistrements dans une base de données, il peut y avoir des problèmes d’autorisations, la connexion à la base de données peut être supprimée, les données à enregistrer peuvent ne pas être valides, etc.</span><span class="sxs-lookup"><span data-stu-id="f9639-521">Similarly, if your code tries to update records in a database, there can be permissions issues, the connection to the database might be dropped, the data to save might be invalid, and so on.</span></span>

<span data-ttu-id="f9639-522">En termes de programmation, ces situations sont appelées *exceptions*.</span><span class="sxs-lookup"><span data-stu-id="f9639-522">In programming terms, these situations are called *exceptions*.</span></span> <span data-ttu-id="f9639-523">Si votre code rencontre une exception, il génère (lève) un message d’erreur qui est, au mieux, ennuyeux pour les utilisateurs :</span><span class="sxs-lookup"><span data-stu-id="f9639-523">If your code encounters an exception, it generates (throws) an error message that's, at best, annoying to users:</span></span>

![Razor-Img14](introducing-razor-syntax-c/_static/image14.jpg)

<span data-ttu-id="f9639-525">Dans les cas où votre code risque de rencontrer des exceptions et afin d’éviter les messages d’erreur de ce type, vous pouvez utiliser des instructions `try/catch`.</span><span class="sxs-lookup"><span data-stu-id="f9639-525">In situations where your code might encounter exceptions, and in order to avoid error messages of this type, you can use `try/catch` statements.</span></span> <span data-ttu-id="f9639-526">Dans l’instruction `try`, vous exécutez le code que vous êtes en train de vérifier.</span><span class="sxs-lookup"><span data-stu-id="f9639-526">In the `try` statement, you run the code that you're checking.</span></span> <span data-ttu-id="f9639-527">Dans une ou plusieurs instructions `catch`, vous pouvez rechercher des erreurs spécifiques (types spécifiques d’exceptions) qui ont pu se produire.</span><span class="sxs-lookup"><span data-stu-id="f9639-527">In one or more `catch` statements, you can look for specific errors (specific types of exceptions) that might have occurred.</span></span> <span data-ttu-id="f9639-528">Vous pouvez inclure autant d’instructions `catch` que vous le souhaitez pour rechercher les erreurs que vous anticipez.</span><span class="sxs-lookup"><span data-stu-id="f9639-528">You can include as many `catch` statements as you need to look for errors that you are anticipating.</span></span>

> [!NOTE]
> <span data-ttu-id="f9639-529">Nous vous recommandons d’éviter d’utiliser la méthode `Response.Redirect` dans les instructions `try/catch`, car cela peut provoquer une exception dans votre page.</span><span class="sxs-lookup"><span data-stu-id="f9639-529">We recommend that you avoid using the `Response.Redirect` method in `try/catch` statements, because it can cause an exception in your page.</span></span>

<span data-ttu-id="f9639-530">L’exemple suivant montre une page qui crée un fichier texte lors de la première demande, puis affiche un bouton qui permet à l’utilisateur d’ouvrir le fichier.</span><span class="sxs-lookup"><span data-stu-id="f9639-530">The following example shows a page that creates a text file on the first request and then displays a button that lets the user open the file.</span></span> <span data-ttu-id="f9639-531">L’exemple utilise délibérément un nom de fichier incorrect afin qu’il génère une exception.</span><span class="sxs-lookup"><span data-stu-id="f9639-531">The example deliberately uses a bad file name so that it will cause an exception.</span></span> <span data-ttu-id="f9639-532">Le code comprend des instructions `catch` pour deux exceptions possibles : `FileNotFoundException`, qui se produit si le nom de fichier est incorrect, et `DirectoryNotFoundException`, qui se produit si ASP.NET ne peut même pas trouver le dossier.</span><span class="sxs-lookup"><span data-stu-id="f9639-532">The code includes `catch` statements for two possible exceptions: `FileNotFoundException`, which occurs if the file name is bad, and `DirectoryNotFoundException`, which occurs if ASP.NET can't even find the folder.</span></span> <span data-ttu-id="f9639-533">(Vous pouvez supprimer les marques de commentaire d’une instruction dans l’exemple afin de voir comment elle s’exécute quand tout fonctionne correctement.)</span><span class="sxs-lookup"><span data-stu-id="f9639-533">(You can uncomment a statement in the example in order to see how it runs when everything works properly.)</span></span>

<span data-ttu-id="f9639-534">Si votre code n’a pas géré l’exception, vous verrez une page d’erreur telle que la capture d’écran précédente.</span><span class="sxs-lookup"><span data-stu-id="f9639-534">If your code didn't handle the exception, you would see an error page like the previous screen shot.</span></span> <span data-ttu-id="f9639-535">Toutefois, la section `try/catch` permet d’empêcher l’utilisateur de voir ces types d’erreurs.</span><span class="sxs-lookup"><span data-stu-id="f9639-535">However, the `try/catch` section helps prevent the user from seeing these types of errors.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="f9639-536">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="f9639-536">Additional Resources</span></span>

<span data-ttu-id="f9639-537">**Programmation avec Visual Basic**</span><span class="sxs-lookup"><span data-stu-id="f9639-537">**Programming with Visual Basic**</span></span>

[<span data-ttu-id="f9639-538">Annexe : langage et syntaxe de Visual Basic</span><span class="sxs-lookup"><span data-stu-id="f9639-538">Appendix: Visual Basic Language and Syntax</span></span>](https://go.microsoft.com/fwlink/?LinkId=202908)

<span data-ttu-id="f9639-539">**Documentation de référence**</span><span class="sxs-lookup"><span data-stu-id="f9639-539">**Reference Documentation**</span></span>

[<span data-ttu-id="f9639-540">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f9639-540">ASP.NET</span></span>](https://msdn.microsoft.com/library/ee532866.aspx)

[<span data-ttu-id="f9639-541">C#Sous</span><span class="sxs-lookup"><span data-stu-id="f9639-541">C# Language</span></span>](https://msdn.microsoft.com/library/kx37x362.aspx)
