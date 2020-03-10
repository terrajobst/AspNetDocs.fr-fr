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
# <a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a><span data-ttu-id="3b50e-103">Introduction to ASP.NET Web Programming Using the Razor Syntax (Visual Basic) (Présentation de la programmation Web ASP.NET à l'aide de la syntaxe Razor (Visual Basic)).</span><span class="sxs-lookup"><span data-stu-id="3b50e-103">Introduction to ASP.NET Web Programming Using the Razor Syntax (Visual Basic)</span></span>

<span data-ttu-id="3b50e-104">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="3b50e-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="3b50e-105">Cet article vous donne une vue d’ensemble de la programmation avec pages Web ASP.NET à l’aide du syntaxe Razor et de Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="3b50e-105">This article gives you an overview of programming with ASP.NET Web Pages using the Razor syntax and Visual Basic.</span></span> <span data-ttu-id="3b50e-106">ASP.NET est la technologie de Microsoft pour l’exécution de pages Web dynamiques sur des serveurs Web.</span><span class="sxs-lookup"><span data-stu-id="3b50e-106">ASP.NET is Microsoft's technology for running dynamic web pages on web servers.</span></span>
> 
> <span data-ttu-id="3b50e-107">**Ce que vous allez apprendre**:</span><span class="sxs-lookup"><span data-stu-id="3b50e-107">**What you'll learn**:</span></span>
> 
> - <span data-ttu-id="3b50e-108">Les 8 principales astuces de programmation pour la prise en main de la programmation pages Web ASP.NET à l’aide de syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="3b50e-108">The top 8 programming tips for getting started with programming ASP.NET Web Pages using Razor syntax.</span></span>
> - <span data-ttu-id="3b50e-109">Concepts de programmation de base dont vous aurez besoin.</span><span class="sxs-lookup"><span data-stu-id="3b50e-109">Basic programming concepts you'll need.</span></span>
> - <span data-ttu-id="3b50e-110">Ce que ASP.NET le code serveur et le syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="3b50e-110">What ASP.NET server code and the Razor syntax is all about.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="3b50e-111">Versions de logiciels</span><span class="sxs-lookup"><span data-stu-id="3b50e-111">Software versions</span></span>
> 
> 
> - <span data-ttu-id="3b50e-112">Pages Web ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="3b50e-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="3b50e-113">Ce didacticiel fonctionne également avec pages Web ASP.NET 2.</span><span class="sxs-lookup"><span data-stu-id="3b50e-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>

<span data-ttu-id="3b50e-114">La plupart des exemples d’utilisation de pages Web ASP.NET C#avec syntaxe Razor utilisent.</span><span class="sxs-lookup"><span data-stu-id="3b50e-114">Most examples of using ASP.NET Web Pages with Razor syntax use C#.</span></span> <span data-ttu-id="3b50e-115">Mais le syntaxe Razor prend également en charge les Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="3b50e-115">But the Razor syntax also supports Visual Basic.</span></span> <span data-ttu-id="3b50e-116">Pour programmer une page Web ASP.NET dans Visual Basic, vous créez une page Web avec une extension de nom de fichier *. vbhtml* , puis vous ajoutez Visual Basic code.</span><span class="sxs-lookup"><span data-stu-id="3b50e-116">To program an ASP.NET web page in Visual Basic, you create a web page with a *.vbhtml* filename extension, and then add Visual Basic code.</span></span> <span data-ttu-id="3b50e-117">Cet article vous donne une vue d’ensemble de l’utilisation du langage et de la syntaxe Visual Basic pour créer des pages Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3b50e-117">This article gives you an overview of working with the Visual Basic language and syntax to create ASP.NET Webpages.</span></span>

> [!NOTE]
> <span data-ttu-id="3b50e-118">Les modèles de site Web par défaut pour Microsoft WebMatrix (**boulangerie**, **Galerie Photo**et **site de démarrage**, etc.) C# sont disponibles dans et les versions Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="3b50e-118">The default website templates for Microsoft WebMatrix (**Bakery**, **Photo Gallery**, and **Starter Site**, etc.) are available in C# and Visual Basic versions.</span></span> <span data-ttu-id="3b50e-119">Vous pouvez installer les modèles Visual Basic en tant que packages NuGet.</span><span class="sxs-lookup"><span data-stu-id="3b50e-119">You can install the Visual Basic templates by as NuGet packages.</span></span> <span data-ttu-id="3b50e-120">Les modèles de site Web sont installés dans le dossier racine de votre site, dans un dossier nommé *Microsoft templates*.</span><span class="sxs-lookup"><span data-stu-id="3b50e-120">Website templates are installed in the root folder of your site in a folder named *Microsoft Templates*.</span></span>

## <a name="the-top-8-programming-tips"></a><span data-ttu-id="3b50e-121">Les 8 meilleurs conseils de programmation</span><span class="sxs-lookup"><span data-stu-id="3b50e-121">The Top 8 Programming Tips</span></span>

<span data-ttu-id="3b50e-122">Cette section répertorie quelques conseils que vous devez absolument savoir lorsque vous commencez à écrire du code serveur ASP.NET à l’aide de l’syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="3b50e-122">This section lists a few tips that you absolutely need to know as you start writing ASP.NET server code using the Razor syntax.</span></span>

### <a name="1-you-add-code-to-a-page-using-the--character"></a><span data-ttu-id="3b50e-123">1. vous ajoutez du code à une page à l’aide du caractère @</span><span class="sxs-lookup"><span data-stu-id="3b50e-123">1. You add code to a page using the @ character</span></span>

<span data-ttu-id="3b50e-124">Le caractère `@` démarre les expressions Inline, les blocs à instruction unique et les blocs à instructions multiples :</span><span class="sxs-lookup"><span data-stu-id="3b50e-124">The `@` character starts inline expressions, single-statement blocks, and multi-statement blocks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

<span data-ttu-id="3b50e-125">Résultat affiché dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="3b50e-125">The result displayed in a browser:</span></span>

![Rasoir-Img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> <span data-ttu-id="3b50e-127">**Encodage HTML**</span><span class="sxs-lookup"><span data-stu-id="3b50e-127">**HTML Encoding**</span></span>
> 
> <span data-ttu-id="3b50e-128">Lorsque vous affichez le contenu d’une page à l’aide du caractère `@`, comme dans les exemples précédents, ASP.NET code HTML de la sortie.</span><span class="sxs-lookup"><span data-stu-id="3b50e-128">When you display content in a page using the `@` character, as in the preceding examples, ASP.NET HTML-encodes the output.</span></span> <span data-ttu-id="3b50e-129">Cela remplace les caractères HTML réservés (tels que `<` et `>` et `&`) par des codes qui permettent d’afficher les caractères sous forme de caractères dans une page Web au lieu d’être interprétés comme des balises ou des entités HTML.</span><span class="sxs-lookup"><span data-stu-id="3b50e-129">This replaces reserved HTML characters (such as `<` and `>` and `&`) with codes that enable the characters to be displayed as characters in a web page instead of being interpreted as HTML tags or entities.</span></span> <span data-ttu-id="3b50e-130">Sans l’encodage HTML, la sortie de votre code serveur peut ne pas s’afficher correctement et exposer une page à des risques de sécurité.</span><span class="sxs-lookup"><span data-stu-id="3b50e-130">Without HTML encoding, the output from your server code might not display correctly, and could expose a page to security risks.</span></span>
> 
> <span data-ttu-id="3b50e-131">Si votre objectif est de générer un balisage HTML qui rend les balises sous forme de balisage (par exemple `<p></p>` pour un paragraphe ou `<em></em>` pour mettre en évidence du texte), consultez la section [combinaison de texte, de balises et de code dans les blocs de code](#BM_CombiningTextMarkupAndCode) plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="3b50e-131">If your goal is to output HTML markup that renders tags as markup (for example `<p></p>` for a paragraph or `<em></em>` to emphasize text), see the section [Combining Text, Markup, and Code in Code Blocks](#BM_CombiningTextMarkupAndCode) later in this article.</span></span>
> 
> <span data-ttu-id="3b50e-132">Pour plus d’informations sur l’encodage HTML, consultez [utilisation des formulaires HTML dans les Sites pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=202892).</span><span class="sxs-lookup"><span data-stu-id="3b50e-132">You can read more about HTML encoding in [Working with HTML Forms in ASP.NET Web Pages Sites](https://go.microsoft.com/fwlink/?LinkId=202892).</span></span>

### <a name="2-you-enclose-code-blocks-with-codeend-code"></a><span data-ttu-id="3b50e-133">2. Placez les blocs de code avec le code... Code de fin</span><span class="sxs-lookup"><span data-stu-id="3b50e-133">2. You enclose code blocks with Code...End Code</span></span>

<span data-ttu-id="3b50e-134">Un bloc de code comprend une ou plusieurs instructions de code et est inclus avec les mots clés `Code` et `End Code`.</span><span class="sxs-lookup"><span data-stu-id="3b50e-134">A code block includes one or more code statements and is enclosed with the keywords `Code` and `End Code`.</span></span> <span data-ttu-id="3b50e-135">Placez le mot clé `Code` d’ouverture immédiatement après le &#8212; `@` caractère où il ne peut pas y avoir d’espace blanc.</span><span class="sxs-lookup"><span data-stu-id="3b50e-135">Place the opening `Code` keyword immediately after the `@` character &#8212; there can't be whitespace between them.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

<span data-ttu-id="3b50e-136">Résultat affiché dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="3b50e-136">The result displayed in a browser:</span></span>

![Razor-Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a><span data-ttu-id="3b50e-138">3. à l’intérieur d’un bloc, vous terminez chaque instruction de code par un saut de ligne</span><span class="sxs-lookup"><span data-stu-id="3b50e-138">3. Inside a block, you end each code statement with a line break</span></span>

<span data-ttu-id="3b50e-139">Dans un bloc de code Visual Basic, chaque instruction se termine par un saut de ligne.</span><span class="sxs-lookup"><span data-stu-id="3b50e-139">In a Visual Basic code block, each statement ends with a line break.</span></span> <span data-ttu-id="3b50e-140">(Plus loin dans cet article, vous verrez un moyen d’encapsuler une longue instruction de code sur plusieurs lignes, si nécessaire.)</span><span class="sxs-lookup"><span data-stu-id="3b50e-140">(Later in the article you'll see a way to wrap a long code statement into multiple lines if needed.)</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a><span data-ttu-id="3b50e-141">4. vous utilisez des variables pour stocker des valeurs</span><span class="sxs-lookup"><span data-stu-id="3b50e-141">4. You use variables to store values</span></span>

<span data-ttu-id="3b50e-142">Vous pouvez stocker des valeurs dans une *variable*, y compris des chaînes, des nombres et des dates, etc. Vous créez une variable à l’aide du mot clé `Dim`.</span><span class="sxs-lookup"><span data-stu-id="3b50e-142">You can store values in a *variable*, including strings, numbers, and dates, etc. You create a new variable using the `Dim` keyword.</span></span> <span data-ttu-id="3b50e-143">Vous pouvez insérer des valeurs de variable directement dans une page à l’aide de `@`.</span><span class="sxs-lookup"><span data-stu-id="3b50e-143">You can insert variable values directly in a page using `@`.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

<span data-ttu-id="3b50e-144">Résultat affiché dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="3b50e-144">The result displayed in a browser:</span></span>

![Rasoir-Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a><span data-ttu-id="3b50e-146">5. vous placez les valeurs de chaîne littérale entre des guillemets doubles</span><span class="sxs-lookup"><span data-stu-id="3b50e-146">5. You enclose literal string values in double quotation marks</span></span>

<span data-ttu-id="3b50e-147">Une *chaîne* est une séquence de caractères qui sont traités comme du texte.</span><span class="sxs-lookup"><span data-stu-id="3b50e-147">A *string* is a sequence of characters that are treated as text.</span></span> <span data-ttu-id="3b50e-148">Pour spécifier une chaîne, vous devez la placer entre guillemets doubles :</span><span class="sxs-lookup"><span data-stu-id="3b50e-148">To specify a string, you enclose it in double quotation marks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

<span data-ttu-id="3b50e-149">Pour incorporer des guillemets doubles dans une valeur de chaîne, insérez deux guillemets doubles.</span><span class="sxs-lookup"><span data-stu-id="3b50e-149">To embed double quotation marks within a string value, insert two double quotation mark characters.</span></span> <span data-ttu-id="3b50e-150">Si vous souhaitez que le caractère de guillemet double apparaisse une fois dans la sortie de la page, entrez-le en tant que `""` dans la chaîne entre guillemets, et si vous souhaitez qu’il apparaisse deux fois, entrez-le comme `""""` dans la chaîne entre guillemets.</span><span class="sxs-lookup"><span data-stu-id="3b50e-150">If you want the double quotation character to appear once in the page output, enter it as `""` within the quoted string, and if you want it to appear twice, enter it as `""""` within the quoted string.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

<span data-ttu-id="3b50e-151">Résultat affiché dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="3b50e-151">The result displayed in a browser:</span></span>

![Rasoir-Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a><span data-ttu-id="3b50e-153">6. Visual Basic code ne respecte pas la casse</span><span class="sxs-lookup"><span data-stu-id="3b50e-153">6. Visual Basic code is not case sensitive</span></span>

<span data-ttu-id="3b50e-154">Le langage de Visual Basic ne respecte pas la casse.</span><span class="sxs-lookup"><span data-stu-id="3b50e-154">The Visual Basic language is not case sensitive.</span></span> <span data-ttu-id="3b50e-155">Les mots clés de programmation (comme `Dim`, `If`et `True`) et les noms de variables (comme `myString`ou `subTotal`) peuvent être écrits dans tous les cas.</span><span class="sxs-lookup"><span data-stu-id="3b50e-155">Programming keywords (like `Dim`, `If`, and `True`) and variable names (like `myString`, or `subTotal`) can be written in any case.</span></span>

<span data-ttu-id="3b50e-156">Les lignes de code suivantes affectent une valeur à la variable `lastname` à l’aide d’un nom en minuscules, puis génèrent la valeur de la variable sur la page à l’aide d’un nom en majuscules.</span><span class="sxs-lookup"><span data-stu-id="3b50e-156">The following lines of code assign a value to the variable `lastname` using a lowercase name, and then output the variable value to the page using an uppercase name.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

<span data-ttu-id="3b50e-157">Résultat affiché dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="3b50e-157">The result displayed in a browser:</span></span>

![VB-syntaxe-5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a><span data-ttu-id="3b50e-159">7. la majeure partie de votre code implique l’utilisation d’objets</span><span class="sxs-lookup"><span data-stu-id="3b50e-159">7. Much of your coding involves working with objects</span></span>

<span data-ttu-id="3b50e-160">Un objet représente un élément que vous pouvez programmer à &#8212; l’aide d’une page, d’une zone de texte, d’un fichier, d’une image, d’une requête Web, d’un message électronique, d’un enregistrement de client (ligne de base de données), etc. Les objets ont des propriétés qui décrivent leurs caractéristiques &#8212; . un objet de zone de texte a une propriété `Text`, un objet Request a une propriété `Url`, un message électronique a une propriété `From` et un objet Customer a une propriété `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="3b50e-160">An object represents a thing that you can program with &#8212; a page, a text box, a file, an image, a web request, an email message, a customer record (database row), etc. Objects have properties that describe their characteristics &#8212; a text box object has a `Text` property, a request object has a `Url` property, an email message has a `From` property, and a customer object has a `FirstName` property.</span></span> <span data-ttu-id="3b50e-161">Les objets ont également des méthodes qui sont les verbes de &quot;&quot; ils peuvent effectuer.</span><span class="sxs-lookup"><span data-stu-id="3b50e-161">Objects also have methods that are the &quot;verbs&quot; they can perform.</span></span> <span data-ttu-id="3b50e-162">Les exemples incluent la méthode `Save` d’un objet fichier, la méthode `Rotate` d’un objet image et la méthode `Send` d’un objet email.</span><span class="sxs-lookup"><span data-stu-id="3b50e-162">Examples include a file object's `Save` method, an image object's `Rotate` method, and an email object's `Send` method.</span></span>

<span data-ttu-id="3b50e-163">Vous travaillez souvent avec l’objet `Request`, qui donne des informations telles que les valeurs des champs de formulaire sur la page (zones de texte, etc.), le type de navigateur qui a effectué la demande, l’URL de la page, l’identité de l’utilisateur, etc. Cet exemple montre comment accéder aux propriétés de l’objet `Request` et comment appeler la méthode `MapPath` de l’objet `Request`, qui vous donne le chemin d’accès absolu de la page sur le serveur :</span><span class="sxs-lookup"><span data-stu-id="3b50e-163">You'll often work with the `Request` object, which gives you information like the values of form fields on the page (text boxes, etc.), what type of browser made the request, the URL of the page, the user identity, etc. This example shows how to access properties of the `Request` object and how to call the `MapPath` method of the `Request` object, which gives you the absolute path of the page on the server:</span></span>

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

<span data-ttu-id="3b50e-164">Résultat affiché dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="3b50e-164">The result displayed in a browser:</span></span>

![Rasoir-Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a><span data-ttu-id="3b50e-166">8. vous pouvez écrire du code qui prend des décisions</span><span class="sxs-lookup"><span data-stu-id="3b50e-166">8. You can write code that makes decisions</span></span>

<span data-ttu-id="3b50e-167">Une fonctionnalité clé des pages Web dynamiques est que vous pouvez déterminer ce que vous devez faire en fonction des conditions.</span><span class="sxs-lookup"><span data-stu-id="3b50e-167">A key feature of dynamic web pages is that you can determine what to do based on conditions.</span></span> <span data-ttu-id="3b50e-168">La méthode la plus courante consiste à utiliser l’instruction `If` (et l’instruction facultative `Else`).</span><span class="sxs-lookup"><span data-stu-id="3b50e-168">The most common way to do this is with the `If` statement (and optional `Else` statement).</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

<span data-ttu-id="3b50e-169">L’instruction `If IsPost` est un moyen simple d’écrire des `If IsPost = True`.</span><span class="sxs-lookup"><span data-stu-id="3b50e-169">The statement `If IsPost` is a shorthand way of writing `If IsPost = True`.</span></span> <span data-ttu-id="3b50e-170">Outre les instructions `If`, il existe différentes façons de tester des conditions, de répéter des blocs de code, etc., qui sont décrits plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="3b50e-170">Along with `If` statements, there are a variety of ways to test conditions, repeat blocks of code, and so on, which are described later in this article.</span></span>

<span data-ttu-id="3b50e-171">Résultat affiché dans un navigateur (après avoir cliqué sur **Envoyer**) :</span><span class="sxs-lookup"><span data-stu-id="3b50e-171">The result displayed in a browser (after clicking **Submit**):</span></span>

![Rasoir-Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> <span data-ttu-id="3b50e-173">**Méthodes HTTP d’extraction et de publication et propriété IsPost**</span><span class="sxs-lookup"><span data-stu-id="3b50e-173">**HTTP GET and POST Methods and the IsPost Property**</span></span>
> 
> <span data-ttu-id="3b50e-174">Le protocole utilisé pour les pages Web (HTTP) prend en charge un nombre très limité de méthodes (&quot;verbes&quot;) utilisées pour effectuer des requêtes sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="3b50e-174">The protocol used for web pages (HTTP) supports a very limited number of methods (&quot;verbs&quot;) that are used to make requests to the server.</span></span> <span data-ttu-id="3b50e-175">Les deux types les plus courants sont obtenir, qui est utilisé pour lire une page, et la publication, qui est utilisée pour envoyer une page.</span><span class="sxs-lookup"><span data-stu-id="3b50e-175">The two most common ones are GET, which is used to read a page, and POST, which is used to submit a page.</span></span> <span data-ttu-id="3b50e-176">En général, la première fois qu’un utilisateur demande une page, la page est demandée à l’aide de la requête obtenir.</span><span class="sxs-lookup"><span data-stu-id="3b50e-176">In general, the first time a user requests a page, the page is requested using GET.</span></span> <span data-ttu-id="3b50e-177">Si l’utilisateur remplit un formulaire, puis clique sur **Envoyer**, le navigateur envoie une demande de publication au serveur.</span><span class="sxs-lookup"><span data-stu-id="3b50e-177">If the user fills in a form and then clicks **Submit**, the browser makes a POST request to the server.</span></span>
> 
> <span data-ttu-id="3b50e-178">Dans la programmation Web, il est souvent utile de savoir si une page est demandée en tant que « obtenir » ou « publication », afin que vous sachiez comment traiter la page.</span><span class="sxs-lookup"><span data-stu-id="3b50e-178">In web programming, it's often useful to know whether a page is being requested as a GET or as a POST so that you know how to process the page.</span></span> <span data-ttu-id="3b50e-179">Dans pages Web ASP.NET, vous pouvez utiliser la propriété `IsPost` pour déterminer si une requête est une requête d’extraction ou une publication.</span><span class="sxs-lookup"><span data-stu-id="3b50e-179">In ASP.NET Web Pages, you can use the `IsPost` property to see whether a request is a GET or a POST.</span></span> <span data-ttu-id="3b50e-180">Si la demande est une publication, la propriété `IsPost` retourne la valeur true et vous pouvez effectuer des opérations telles que lire les valeurs des zones de texte sur un formulaire.</span><span class="sxs-lookup"><span data-stu-id="3b50e-180">If the request is a POST, the `IsPost` property will return true, and you can do things like read the values of text boxes on a form.</span></span> <span data-ttu-id="3b50e-181">De nombreux exemples vous montrent comment traiter la page différemment en fonction de la valeur de `IsPost`.</span><span class="sxs-lookup"><span data-stu-id="3b50e-181">Many examples you'll see show you how to process the page differently depending on the value of `IsPost`.</span></span>

## <a name="a-simple-code-example"></a><span data-ttu-id="3b50e-182">Exemple de code simple</span><span class="sxs-lookup"><span data-stu-id="3b50e-182">A Simple Code Example</span></span>

<span data-ttu-id="3b50e-183">Cette procédure vous montre comment créer une page qui illustre des techniques de programmation de base.</span><span class="sxs-lookup"><span data-stu-id="3b50e-183">This procedure shows you how to create a page that illustrates basic programming techniques.</span></span> <span data-ttu-id="3b50e-184">Dans l’exemple, vous créez une page qui permet aux utilisateurs d’entrer deux nombres, puis de les ajouter et d’afficher le résultat.</span><span class="sxs-lookup"><span data-stu-id="3b50e-184">In the example, you create a page that lets users enter two numbers, then it adds them and displays the result.</span></span>

1. <span data-ttu-id="3b50e-185">Dans votre éditeur, créez un nouveau fichier et nommez-le *AddNumbers. vbhtml*.</span><span class="sxs-lookup"><span data-stu-id="3b50e-185">In your editor, create a new file and name it *AddNumbers.vbhtml*.</span></span>
2. <span data-ttu-id="3b50e-186">Copiez le code et le balisage suivants dans la page, en remplaçant tout ce qui se trouve déjà dans la page.</span><span class="sxs-lookup"><span data-stu-id="3b50e-186">Copy the following code and markup into the page, replacing anything already in the page.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    <span data-ttu-id="3b50e-187">Voici quelques points à noter :</span><span class="sxs-lookup"><span data-stu-id="3b50e-187">Here are some things for you to note:</span></span>

    - <span data-ttu-id="3b50e-188">Le caractère `@` démarre le premier bloc de code dans la page et précède la variable `totalMessage` incorporée dans la partie inférieure.</span><span class="sxs-lookup"><span data-stu-id="3b50e-188">The `@` character starts the first block of code in the page, and it precedes the `totalMessage` variable embedded near the bottom.</span></span>
    - <span data-ttu-id="3b50e-189">Le bloc en haut de la page est placé dans `Code...End Code`.</span><span class="sxs-lookup"><span data-stu-id="3b50e-189">The block at the top of the page is enclosed in `Code...End Code`.</span></span>
    - <span data-ttu-id="3b50e-190">Les variables `total`, `num1`, `num2`et `totalMessage` stockent plusieurs nombres et une chaîne.</span><span class="sxs-lookup"><span data-stu-id="3b50e-190">The variables `total`, `num1`, `num2`, and `totalMessage` store several numbers and a string.</span></span>
    - <span data-ttu-id="3b50e-191">La valeur de chaîne littérale assignée à la variable `totalMessage` est entre guillemets doubles.</span><span class="sxs-lookup"><span data-stu-id="3b50e-191">The literal string value assigned to the `totalMessage` variable is in double quotation marks.</span></span>
    - <span data-ttu-id="3b50e-192">Étant donné que Visual Basic code ne respecte pas la casse, lorsque la variable `totalMessage` est utilisée vers le bas de la page, son nom ne doit correspondre qu’à l’orthographe de la déclaration de variable en haut de la page.</span><span class="sxs-lookup"><span data-stu-id="3b50e-192">Because Visual Basic code is not case sensitive, when the `totalMessage` variable is used near the bottom of the page, its name only needs to match the spelling of the variable declaration at the top of the page.</span></span> <span data-ttu-id="3b50e-193">La casse n’a pas d’importance.</span><span class="sxs-lookup"><span data-stu-id="3b50e-193">The casing doesn't matter.</span></span>
    - <span data-ttu-id="3b50e-194">L’expression `num1.AsInt()` + `num2.AsInt()` montre comment utiliser des objets et des méthodes.</span><span class="sxs-lookup"><span data-stu-id="3b50e-194">The expression `num1.AsInt()` + `num2.AsInt()` shows how to work with objects and methods.</span></span> <span data-ttu-id="3b50e-195">La méthode `AsInt` sur chaque variable convertit la chaîne entrée par un utilisateur en un nombre entier (entier) qui peut être ajouté.</span><span class="sxs-lookup"><span data-stu-id="3b50e-195">The `AsInt` method on each variable converts the string entered by a user to a whole number (an integer) that can be added.</span></span>
    - <span data-ttu-id="3b50e-196">La balise `<form>` comprend un attribut `method="post"`.</span><span class="sxs-lookup"><span data-stu-id="3b50e-196">The `<form>` tag includes a `method="post"` attribute.</span></span> <span data-ttu-id="3b50e-197">Cela spécifie que lorsque l’utilisateur clique sur **Ajouter**, la page est envoyée au serveur à l’aide de la méthode http.</span><span class="sxs-lookup"><span data-stu-id="3b50e-197">This specifies that when the user clicks **Add**, the page will be sent to the server using the HTTP POST method.</span></span> <span data-ttu-id="3b50e-198">Lorsque la page est envoyée, le code `If IsPost` prend la valeur true et le code conditionnel s’exécute et affiche le résultat de l’ajout des nombres.</span><span class="sxs-lookup"><span data-stu-id="3b50e-198">When the page is submitted, the code `If IsPost` evaluates to true and the conditional code runs, displaying the result of adding the numbers.</span></span>
3. <span data-ttu-id="3b50e-199">Enregistrez la page et exécutez-la dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="3b50e-199">Save the page and run it in a browser.</span></span> <span data-ttu-id="3b50e-200">(Assurez-vous que la page est sélectionnée dans l’espace de travail **fichiers** avant de l’exécuter.) Entrez deux nombres entiers, puis cliquez sur le bouton **Ajouter** .</span><span class="sxs-lookup"><span data-stu-id="3b50e-200">(Make sure the page is selected in the **Files** workspace before you run it.) Enter two whole numbers and then click the **Add** button.</span></span>

    ![Rasoir-Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a><span data-ttu-id="3b50e-202">Langage et syntaxe de Visual Basic</span><span class="sxs-lookup"><span data-stu-id="3b50e-202">Visual Basic Language and Syntax</span></span>

<span data-ttu-id="3b50e-203">Vous avez vu précédemment un exemple de base de la création d’une page Web ASP.NET et la façon dont vous pouvez ajouter du code serveur au balisage HTML.</span><span class="sxs-lookup"><span data-stu-id="3b50e-203">Earlier you saw a basic example of how to create an ASP.NET web page, and how you can add server code to HTML markup.</span></span> <span data-ttu-id="3b50e-204">Vous découvrirez ici les principes fondamentaux de l’utilisation de Visual Basic pour écrire du code serveur &#8212; ASP.net à l’aide de la syntaxe Razor, c’est-à-dire des règles du langage de programmation.</span><span class="sxs-lookup"><span data-stu-id="3b50e-204">Here you'll learn the basics of using Visual Basic to write ASP.NET server code using the Razor syntax &#8212; that is, the programming language rules.</span></span>

<span data-ttu-id="3b50e-205">Si vous êtes familiarisé avec la programmation (surtout si vous avez utilisé C C++, C#,, Visual Basic ou JavaScript), la plupart des éléments que vous avez lus ici vous seront familiers.</span><span class="sxs-lookup"><span data-stu-id="3b50e-205">If you're experienced with programming (especially if you've used C, C++, C#, Visual Basic, or JavaScript), much of what you read here will be familiar.</span></span> <span data-ttu-id="3b50e-206">Vous devrez probablement vous familiariser avec la façon dont le code WebMatrix est ajouté au balisage dans les fichiers *. vbhtml* .</span><span class="sxs-lookup"><span data-stu-id="3b50e-206">You'll probably need to familiarize yourself only with how WebMatrix code is added to markup in *.vbhtml* files.</span></span>

### <a id="BM_CombiningTextMarkupAndCode"></a><span data-ttu-id="3b50e-207">Combinaison de texte, de balisage et de code dans des blocs de code</span><span class="sxs-lookup"><span data-stu-id="3b50e-207">Combining text, markup, and code in code blocks</span></span>

<span data-ttu-id="3b50e-208">Dans les blocs de code serveur, vous souhaiterez souvent sortir le texte et le balisage de la page.</span><span class="sxs-lookup"><span data-stu-id="3b50e-208">In server code blocks, you'll often want to output text and markup to the page.</span></span> <span data-ttu-id="3b50e-209">Si un bloc de code serveur contient du texte qui n’est pas du code et qu’il doit à la place être rendu tel quel, ASP.NET doit être en mesure de distinguer ce texte du code.</span><span class="sxs-lookup"><span data-stu-id="3b50e-209">If a server code block contains text that's not code and that instead should be rendered as is, ASP.NET needs to be able to distinguish that text from code.</span></span> <span data-ttu-id="3b50e-210">Plusieurs méthodes sont possibles.</span><span class="sxs-lookup"><span data-stu-id="3b50e-210">There are several ways to do this.</span></span>

- <span data-ttu-id="3b50e-211">Placez le texte dans un élément de bloc HTML comme `<p></p>` ou `<em></em>`:</span><span class="sxs-lookup"><span data-stu-id="3b50e-211">Enclose the text in an HTML block element like `<p></p>` or `<em></em>`:</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    <span data-ttu-id="3b50e-212">L’élément HTML peut inclure du texte, des éléments HTML supplémentaires et des expressions de code de serveur.</span><span class="sxs-lookup"><span data-stu-id="3b50e-212">The HTML element can include text, additional HTML elements, and server-code expressions.</span></span> <span data-ttu-id="3b50e-213">Quand ASP.NET voit la balise HTML d’ouverture (par exemple, `<p>`), il restitue tout l’élément et son contenu tel quel dans le navigateur (et résout les expressions de code serveur).</span><span class="sxs-lookup"><span data-stu-id="3b50e-213">When ASP.NET sees the opening HTML tag (for example, `<p>`), it renders everything the element and its content as is to the browser (and resolves the server-code expressions).</span></span>

- <span data-ttu-id="3b50e-214">Utilisez l’opérateur `@:` ou l’élément `<text>`.</span><span class="sxs-lookup"><span data-stu-id="3b50e-214">Use the `@:` operator or the `<text>` element.</span></span> <span data-ttu-id="3b50e-215">Le `@:` génère une seule ligne de contenu contenant du texte brut ou des balises HTML sans correspondance ; l’élément `<text>` encadre plusieurs lignes à la sortie.</span><span class="sxs-lookup"><span data-stu-id="3b50e-215">The `@:` outputs a single line of content containing plain text or unmatched HTML tags; the `<text>` element encloses multiple lines to output.</span></span> <span data-ttu-id="3b50e-216">Ces options sont utiles lorsque vous ne souhaitez pas restituer un élément HTML dans le cadre de la sortie.</span><span class="sxs-lookup"><span data-stu-id="3b50e-216">These options are useful when you don't want to render an HTML element as part of the output.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    <span data-ttu-id="3b50e-217">L’exemple suivant répète l’exemple précédent, mais utilise une seule paire de balises `<text>` pour encadrer le texte à afficher.</span><span class="sxs-lookup"><span data-stu-id="3b50e-217">The following example repeats the previous example but uses a single pair of `<text>` tags to enclose the text to render.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    <span data-ttu-id="3b50e-218">Dans l’exemple suivant, les balises `<text>` et `</text>` encadrent trois lignes, qui ont toutes un texte sans relation et des balises HTML non appariées (`<br />`), ainsi que du code serveur et des balises HTML correspondantes.</span><span class="sxs-lookup"><span data-stu-id="3b50e-218">In the following example, the `<text>` and `</text>` tags enclose three lines, all of which have some uncontained text and unmatched HTML tags (`<br />`), along with server code and matched HTML tags.</span></span> <span data-ttu-id="3b50e-219">Là encore, vous pouvez également faire précéder chaque ligne d’un opérateur `@:` ; les deux méthodes fonctionnent.</span><span class="sxs-lookup"><span data-stu-id="3b50e-219">Again, you could also precede each line individually with the `@:` operator; either way works.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > <span data-ttu-id="3b50e-220">Lorsque vous affichez le texte comme indiqué dans &#8212; cette section à l’aide d’un élément HTML, l’opérateur `@:` &#8212; ou l’élément `<text>` ASP.net n’encode pas la sortie au format html.</span><span class="sxs-lookup"><span data-stu-id="3b50e-220">When you output text as shown in this section &#8212; using an HTML element, the `@:` operator, or the `<text>` element &#8212; ASP.NET doesn't HTML-encode the output.</span></span> <span data-ttu-id="3b50e-221">(Comme indiqué précédemment, ASP.NET encode la sortie des expressions de code serveur et des blocs de code serveur précédés de `@`, sauf dans les cas particuliers indiqués dans cette section.)</span><span class="sxs-lookup"><span data-stu-id="3b50e-221">(As noted earlier, ASP.NET does encode the output of server code expressions and server code blocks that are preceded by `@`, except in the special cases noted in this section.)</span></span>

### <a name="whitespace"></a><span data-ttu-id="3b50e-222">Whitespace</span><span class="sxs-lookup"><span data-stu-id="3b50e-222">Whitespace</span></span>

<span data-ttu-id="3b50e-223">Les espaces supplémentaires dans une instruction (et en dehors d’un littéral de chaîne) n’affectent pas l’instruction :</span><span class="sxs-lookup"><span data-stu-id="3b50e-223">Extra spaces in a statement (and outside of a string literal) don't affect the statement:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a><span data-ttu-id="3b50e-224">Fractionnement des instructions longues en plusieurs lignes</span><span class="sxs-lookup"><span data-stu-id="3b50e-224">Breaking long statements into multiple lines</span></span>

<span data-ttu-id="3b50e-225">Vous pouvez scinder une longue instruction de code en plusieurs lignes à l’aide du caractère de soulignement `_` (qui, dans Visual Basic, est appelé *caractère de continuation*) après chaque ligne de code.</span><span class="sxs-lookup"><span data-stu-id="3b50e-225">You can break a long code statement into multiple lines by using the underscore character `_` (which in Visual Basic is called the *continuation character*) after each line of code.</span></span> <span data-ttu-id="3b50e-226">Pour arrêter une instruction sur la ligne suivante, à la fin de la ligne, ajoutez un espace, puis le caractère de continuation.</span><span class="sxs-lookup"><span data-stu-id="3b50e-226">To break a statement onto the next line, at the end of the line add a space and then the continuation character.</span></span> <span data-ttu-id="3b50e-227">Poursuivez l’instruction sur la ligne suivante.</span><span class="sxs-lookup"><span data-stu-id="3b50e-227">Continue the statement on the next line.</span></span> <span data-ttu-id="3b50e-228">Vous pouvez encapsuler des instructions sur autant de lignes que nécessaire pour améliorer la lisibilité.</span><span class="sxs-lookup"><span data-stu-id="3b50e-228">You can wrap statements onto as many lines as you need to improve readability.</span></span> <span data-ttu-id="3b50e-229">Les instructions suivantes sont les mêmes :</span><span class="sxs-lookup"><span data-stu-id="3b50e-229">The following statements are the same:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

<span data-ttu-id="3b50e-230">Toutefois, vous ne pouvez pas encapsuler une ligne au milieu d’un littéral de chaîne.</span><span class="sxs-lookup"><span data-stu-id="3b50e-230">However, you can't wrap a line in the middle of a string literal.</span></span> <span data-ttu-id="3b50e-231">L’exemple suivant ne fonctionne pas :</span><span class="sxs-lookup"><span data-stu-id="3b50e-231">The following example doesn't work:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

<span data-ttu-id="3b50e-232">Pour combiner une longue chaîne encapsulée sur plusieurs lignes comme le code ci-dessus, vous devez utiliser l' *opérateur de concaténation* (`&`), que vous verrez plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="3b50e-232">To combine a long string that wraps to multiple lines like the above code, you would need to use the *concatenation operator* (`&`), which you'll see later in this article.</span></span>

### <a name="code-comments"></a><span data-ttu-id="3b50e-233">Commentaires de code</span><span class="sxs-lookup"><span data-stu-id="3b50e-233">Code comments</span></span>

<span data-ttu-id="3b50e-234">Les commentaires vous permettent de laisser des notes pour vous-même ou d’autres.</span><span class="sxs-lookup"><span data-stu-id="3b50e-234">Comments let you leave notes for yourself or others.</span></span> <span data-ttu-id="3b50e-235">Syntaxe Razor commentaires sont précédés de `@*` et se terminent par `*@`.</span><span class="sxs-lookup"><span data-stu-id="3b50e-235">Razor syntax comments are prefixed with `@*` and end with `*@`.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

<span data-ttu-id="3b50e-236">Dans les blocs de code, vous pouvez utiliser les commentaires syntaxe Razor, ou vous pouvez utiliser le caractère de commentaire Visual Basic ordinaire, qui est un guillemet simple (`'`) préfixé pour chaque ligne.</span><span class="sxs-lookup"><span data-stu-id="3b50e-236">Within code blocks you can use the Razor syntax comments, or you can use ordinary Visual Basic comment character, which is a single quote (`'`) prefixed to each line.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a><span data-ttu-id="3b50e-237">Variables</span><span class="sxs-lookup"><span data-stu-id="3b50e-237">Variables</span></span>

<span data-ttu-id="3b50e-238">Une variable est un objet nommé que vous utilisez pour stocker des données.</span><span class="sxs-lookup"><span data-stu-id="3b50e-238">A variable is a named object that you use to store data.</span></span> <span data-ttu-id="3b50e-239">Vous pouvez nommer toutes les variables, mais le nom doit commencer par un caractère alphabétique et ne peut pas contenir d’espaces blancs ou de caractères réservés.</span><span class="sxs-lookup"><span data-stu-id="3b50e-239">You can name variables anything, but the name must begin with an alphabetic character and it cannot contain whitespace or reserved characters.</span></span> <span data-ttu-id="3b50e-240">Dans Visual Basic, comme vous l’avez vu précédemment, la casse des lettres d’un nom de variable n’a pas d’importance.</span><span class="sxs-lookup"><span data-stu-id="3b50e-240">In Visual Basic, as you saw earlier, the case of the letters in a variable name doesn't matter.</span></span>

### <a name="variables-and-data-types"></a><span data-ttu-id="3b50e-241">Variables et types de données</span><span class="sxs-lookup"><span data-stu-id="3b50e-241">Variables and data types</span></span>

<span data-ttu-id="3b50e-242">Une variable peut avoir un type de données spécifique, qui indique le type de données qui est stocké dans la variable.</span><span class="sxs-lookup"><span data-stu-id="3b50e-242">A variable can have a specific data type, which indicates what kind of data is stored in the variable.</span></span> <span data-ttu-id="3b50e-243">Vous pouvez avoir des variables de chaîne qui stockent des valeurs de chaîne (comme &quot;Hello World&quot;), des variables de type entier qui stockent des valeurs de nombres entiers (comme 3 ou 79) et des variables de date qui stockent des valeurs de date dans divers formats (comme 4/12/2012 ou 2009).</span><span class="sxs-lookup"><span data-stu-id="3b50e-243">You can have string variables that store string values (like &quot;Hello world&quot;), integer variables that store whole-number values (like 3 or 79), and date variables that store date values in a variety of formats (like 4/12/2012 or March 2009).</span></span> <span data-ttu-id="3b50e-244">Et il existe de nombreux autres types de données que vous pouvez utiliser.</span><span class="sxs-lookup"><span data-stu-id="3b50e-244">And there are many other data types you can use.</span></span>

<span data-ttu-id="3b50e-245">Toutefois, vous n’êtes pas obligé de spécifier un type pour une variable.</span><span class="sxs-lookup"><span data-stu-id="3b50e-245">However, you don't have to specify a type for a variable.</span></span> <span data-ttu-id="3b50e-246">Dans la plupart des cas, ASP.NET peut déterminer le type en fonction de la façon dont les données de la variable sont utilisées.</span><span class="sxs-lookup"><span data-stu-id="3b50e-246">In most cases ASP.NET can figure out the type based on how the data in the variable is being used.</span></span> <span data-ttu-id="3b50e-247">(Parfois, vous devez spécifier un type ; vous verrez des exemples où cela est vrai.)</span><span class="sxs-lookup"><span data-stu-id="3b50e-247">(Occasionally you must specify a type; you'll see examples where this is true.)</span></span>

<span data-ttu-id="3b50e-248">Pour déclarer une variable sans spécifier de type, utilisez `Dim` plus le nom de la variable (par exemple, `Dim myVar`).</span><span class="sxs-lookup"><span data-stu-id="3b50e-248">To declare a variable without specifying a type, use `Dim` plus the variable name (for instance, `Dim myVar`).</span></span> <span data-ttu-id="3b50e-249">Pour déclarer une variable avec un type, utilisez `Dim` plus le nom de la variable, suivi de `As` puis du nom du type (par exemple, `Dim myVar As String`).</span><span class="sxs-lookup"><span data-stu-id="3b50e-249">To declare a variable with a type, use `Dim` plus the variable name, followed by `As` and then the type name (for instance, `Dim myVar As String`).</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

<span data-ttu-id="3b50e-250">L’exemple suivant montre des expressions inline qui utilisent les variables dans une page Web.</span><span class="sxs-lookup"><span data-stu-id="3b50e-250">The following example shows some inline expressions that use the variables in a web page.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

<span data-ttu-id="3b50e-251">Résultat affiché dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="3b50e-251">The result displayed in a browser:</span></span>

![Rasoir-Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a><span data-ttu-id="3b50e-253">Conversion et test des types de données</span><span class="sxs-lookup"><span data-stu-id="3b50e-253">Converting and testing data types</span></span>

<span data-ttu-id="3b50e-254">Bien que ASP.NET puisse généralement déterminer automatiquement un type de données, il est parfois impossible de le faire.</span><span class="sxs-lookup"><span data-stu-id="3b50e-254">Although ASP.NET can usually determine a data type automatically, sometimes it can't.</span></span> <span data-ttu-id="3b50e-255">Par conséquent, vous devrez peut-être aider ASP.NET en effectuant une conversion explicite.</span><span class="sxs-lookup"><span data-stu-id="3b50e-255">Therefore, you might need to help ASP.NET out by performing an explicit conversion.</span></span> <span data-ttu-id="3b50e-256">Même si vous n’avez pas besoin de convertir les types, il est parfois utile de tester le type de données que vous pouvez utiliser.</span><span class="sxs-lookup"><span data-stu-id="3b50e-256">Even if you don't have to convert types, sometimes it's helpful to test to see what type of data you might be working with.</span></span>

<span data-ttu-id="3b50e-257">Le cas le plus courant est que vous devez convertir une chaîne en un autre type, tel qu’un entier ou une date.</span><span class="sxs-lookup"><span data-stu-id="3b50e-257">The most common case is that you have to convert a string to another type, such as to an integer or date.</span></span> <span data-ttu-id="3b50e-258">L’exemple suivant montre un cas typique dans lequel vous devez convertir une chaîne en nombre.</span><span class="sxs-lookup"><span data-stu-id="3b50e-258">The following example shows a typical case where you must convert a string to a number.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

<span data-ttu-id="3b50e-259">En règle générale, l’entrée utilisateur vous est fournie sous forme de chaînes.</span><span class="sxs-lookup"><span data-stu-id="3b50e-259">As a rule, user input comes to you as strings.</span></span> <span data-ttu-id="3b50e-260">Même si vous avez demandé à l’utilisateur d’entrer un nombre et, même s’il a entré un chiffre, lorsque l’entrée de l’utilisateur est envoyée et que vous la lisez dans le code, les données sont au format de chaîne.</span><span class="sxs-lookup"><span data-stu-id="3b50e-260">Even if you've prompted the user to enter a number, and even if they've entered a digit, when user input is submitted and you read it in code, the data is in string format.</span></span> <span data-ttu-id="3b50e-261">Par conséquent, vous devez convertir la chaîne en nombre.</span><span class="sxs-lookup"><span data-stu-id="3b50e-261">Therefore, you must convert the string to a number.</span></span> <span data-ttu-id="3b50e-262">Dans l’exemple, si vous essayez d’effectuer des opérations arithmétiques sur les valeurs sans les convertir, l’erreur suivante se produit, car ASP.NET ne peut pas ajouter deux chaînes :</span><span class="sxs-lookup"><span data-stu-id="3b50e-262">In the example, if you try to perform arithmetic on the values without converting them, the following error results, because ASP.NET cannot add two strings:</span></span>

`Cannot implicitly convert type 'string' to 'int'.`

<span data-ttu-id="3b50e-263">Pour convertir les valeurs en entiers, vous appelez la méthode `AsInt`.</span><span class="sxs-lookup"><span data-stu-id="3b50e-263">To convert the values to integers, you call the `AsInt` method.</span></span> <span data-ttu-id="3b50e-264">Si la conversion réussit, vous pouvez ajouter les nombres.</span><span class="sxs-lookup"><span data-stu-id="3b50e-264">If the conversion is successful, you can then add the numbers.</span></span>

<span data-ttu-id="3b50e-265">Le tableau suivant répertorie quelques méthodes de conversion et de test courantes pour les variables.</span><span class="sxs-lookup"><span data-stu-id="3b50e-265">The following table lists some common conversion and test methods for variables.</span></span>

:::row:::
    :::column:::
        <span data-ttu-id="3b50e-266"><strong>Méthode</strong></span><span class="sxs-lookup"><span data-stu-id="3b50e-266"><strong>Method</strong></span></span>
    :::column-end:::
    :::column:::
        <span data-ttu-id="3b50e-267"><strong>Description</strong></span><span class="sxs-lookup"><span data-stu-id="3b50e-267"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
        <span data-ttu-id="3b50e-268"><strong>Exemple</strong></span><span class="sxs-lookup"><span data-stu-id="3b50e-268"><strong>Example</strong></span></span>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="3b50e-269">Convertit une chaîne qui représente un nombre entier (comme &quot;593&quot;) en un entier.</span><span class="sxs-lookup"><span data-stu-id="3b50e-269">Converts a string that represents a whole number (like &quot;593&quot;) to an integer.</span></span>
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
        <span data-ttu-id="3b50e-270">Convertit une chaîne comme &quot;true&quot; ou &quot;false&quot; en un type booléen.</span><span class="sxs-lookup"><span data-stu-id="3b50e-270">Converts a string like &quot;true&quot; or &quot;false&quot; to a Boolean type.</span></span>
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
        <span data-ttu-id="3b50e-271">Convertit une chaîne qui a une valeur décimale comme &quot;1,3&quot; ou &quot;7,439&quot; en un nombre à virgule flottante.</span><span class="sxs-lookup"><span data-stu-id="3b50e-271">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a floating-point number.</span></span>
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
        <span data-ttu-id="3b50e-272">Convertit une chaîne qui a une valeur décimale comme &quot;1,3&quot; ou &quot;7,439&quot; en nombre décimal.</span><span class="sxs-lookup"><span data-stu-id="3b50e-272">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a decimal number.</span></span> <span data-ttu-id="3b50e-273">(Dans ASP.NET, un nombre décimal est plus précis qu’un nombre à virgule flottante.)</span><span class="sxs-lookup"><span data-stu-id="3b50e-273">(In ASP.NET, a decimal number is more precise than a floating-point number.)</span></span>
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
        <span data-ttu-id="3b50e-274">Convertit une chaîne qui représente une valeur de date et d’heure dans le type de `DateTime` ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3b50e-274">Converts a string that represents a date and time value to the ASP.NET `DateTime` type.</span></span>
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
        <span data-ttu-id="3b50e-275">Convertit tout autre type de données en chaîne.</span><span class="sxs-lookup"><span data-stu-id="3b50e-275">Converts any other data type to a string.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)]
    :::column-end:::
:::row-end:::

## <a name="operators"></a><span data-ttu-id="3b50e-276">Opérateurs</span><span class="sxs-lookup"><span data-stu-id="3b50e-276">Operators</span></span>

<span data-ttu-id="3b50e-277">Un opérateur est un mot clé ou un caractère qui indique à ASP.NET le type de commande à effectuer dans une expression.</span><span class="sxs-lookup"><span data-stu-id="3b50e-277">An operator is a keyword or character that tells ASP.NET what kind of command to perform in an expression.</span></span> <span data-ttu-id="3b50e-278">Visual Basic prend en charge de nombreux opérateurs, mais vous ne devez en reconnaître que quelques-uns pour commencer à développer des pages Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3b50e-278">Visual Basic supports many operators, but you only need to recognize a few to get started developing ASP.NET web pages.</span></span> <span data-ttu-id="3b50e-279">Le tableau suivant récapitule les opérateurs les plus courants.</span><span class="sxs-lookup"><span data-stu-id="3b50e-279">The following table summarizes the most common operators.</span></span>

:::row:::
    :::column:::
        <span data-ttu-id="3b50e-280"><strong>Operator</strong></span><span class="sxs-lookup"><span data-stu-id="3b50e-280"><strong>Operator</strong></span></span>
    :::column-end:::
    :::column:::
        <span data-ttu-id="3b50e-281"><strong>Description</strong></span><span class="sxs-lookup"><span data-stu-id="3b50e-281"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
        <span data-ttu-id="3b50e-282"><strong>Exemples</strong></span><span class="sxs-lookup"><span data-stu-id="3b50e-282"><strong>Examples</strong></span></span>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+ - * /`
    :::column-end:::
    :::column:::
        <span data-ttu-id="3b50e-283">Opérateurs mathématiques utilisés dans les expressions numériques.</span><span class="sxs-lookup"><span data-stu-id="3b50e-283">Math operators used in numerical expressions.</span></span>
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
        <span data-ttu-id="3b50e-284">Assignation et égalité.</span><span class="sxs-lookup"><span data-stu-id="3b50e-284">Assignment and equality.</span></span> <span data-ttu-id="3b50e-285">Selon le contexte, affecte la valeur à droite d’une instruction à l’objet situé à gauche, ou vérifie si les valeurs sont égales.</span><span class="sxs-lookup"><span data-stu-id="3b50e-285">Depending on context, either assigns the value on the right side of a statement to the object on the left side, or checks the values for equality.</span></span>
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
        <span data-ttu-id="3b50e-286">Inégalité</span><span class="sxs-lookup"><span data-stu-id="3b50e-286">Inequality.</span></span> <span data-ttu-id="3b50e-287">Retourne `True` si les valeurs ne sont pas égales.</span><span class="sxs-lookup"><span data-stu-id="3b50e-287">Returns `True` if the values are not equal.</span></span>
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
        <span data-ttu-id="3b50e-288">Inférieur à, supérieur à, inférieur ou égal à et supérieur ou égal à.</span><span class="sxs-lookup"><span data-stu-id="3b50e-288">Less than, greater than, less than or equal, and greater than or equal.</span></span>
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
        <span data-ttu-id="3b50e-289">Concaténation, qui est utilisée pour joindre des chaînes.</span><span class="sxs-lookup"><span data-stu-id="3b50e-289">Concatenation, which is used to join strings.</span></span>
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
        <span data-ttu-id="3b50e-290">Les opérateurs d’incrémentation et de décrémentation, qui ajoutent et soustraient 1 (respectivement) d’une variable.</span><span class="sxs-lookup"><span data-stu-id="3b50e-290">The increment and decrement operators, which add and subtract 1 (respectively) from a variable.</span></span>
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
        <span data-ttu-id="3b50e-291">Cédé.</span><span class="sxs-lookup"><span data-stu-id="3b50e-291">Dot.</span></span> <span data-ttu-id="3b50e-292">Utilisé pour distinguer les objets et leurs propriétés et méthodes.</span><span class="sxs-lookup"><span data-stu-id="3b50e-292">Used to distinguish objects and their properties and methods.</span></span>
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
        <span data-ttu-id="3b50e-293">Parenthèses.</span><span class="sxs-lookup"><span data-stu-id="3b50e-293">Parentheses.</span></span> <span data-ttu-id="3b50e-294">Utilisé pour regrouper des expressions, pour passer des paramètres à des méthodes et pour accéder aux membres de tableaux et de collections.</span><span class="sxs-lookup"><span data-stu-id="3b50e-294">Used to group expressions, to pass parameters to methods, and to access members of arrays and collections.</span></span>
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
        <span data-ttu-id="3b50e-295">Pas.</span><span class="sxs-lookup"><span data-stu-id="3b50e-295">Not.</span></span> <span data-ttu-id="3b50e-296">Inverse la valeur true et vice versa.</span><span class="sxs-lookup"><span data-stu-id="3b50e-296">Reverses a true value to false and vice versa.</span></span> <span data-ttu-id="3b50e-297">Généralement utilisé comme un moyen simple de tester `False` (autrement dit, pour ne pas `True`).</span><span class="sxs-lookup"><span data-stu-id="3b50e-297">Typically used as a shorthand way to test for `False` (that is, for not `True`).</span></span>
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
        <span data-ttu-id="3b50e-298">AND logique and ou, qui sont utilisés pour lier des conditions ensemble.</span><span class="sxs-lookup"><span data-stu-id="3b50e-298">Logical AND and OR, which are used to link conditions together.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]
    :::column-end:::
:::row-end:::

## <a name="working-with-file-and-folder-paths-in-code"></a><span data-ttu-id="3b50e-299">Utilisation des chemins d’accès aux fichiers et aux dossiers dans le code</span><span class="sxs-lookup"><span data-stu-id="3b50e-299">Working with File and Folder Paths in Code</span></span>

<span data-ttu-id="3b50e-300">Vous travaillez souvent avec les chemins d’accès de fichier et de dossier dans votre code.</span><span class="sxs-lookup"><span data-stu-id="3b50e-300">You'll often work with file and folder paths in your code.</span></span> <span data-ttu-id="3b50e-301">Voici un exemple de structure de dossiers physiques pour un site Web tel qu’il peut apparaître sur votre ordinateur de développement :</span><span class="sxs-lookup"><span data-stu-id="3b50e-301">Here is an example of physical folder structure for a website as it might appear on your development computer:</span></span>

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

<span data-ttu-id="3b50e-302">Voici quelques détails essentiels sur les URL et les chemins d’accès :</span><span class="sxs-lookup"><span data-stu-id="3b50e-302">Here are some essential details about URLs and paths:</span></span>

- <span data-ttu-id="3b50e-303">Une URL commence par un nom de domaine (`http://www.example.com`) ou un nom de serveur (`http://localhost`, `http://mycomputer`).</span><span class="sxs-lookup"><span data-stu-id="3b50e-303">A URL begins with either a domain name (`http://www.example.com`) or a server name (`http://localhost`, `http://mycomputer`).</span></span>
- <span data-ttu-id="3b50e-304">Une URL correspond à un chemin d’accès physique sur un ordinateur hôte.</span><span class="sxs-lookup"><span data-stu-id="3b50e-304">A URL corresponds to a physical path on a host computer.</span></span> <span data-ttu-id="3b50e-305">Par exemple, `http://myserver` peut correspondre au dossier *C:\websites\mywebsite* sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="3b50e-305">For example, `http://myserver` might correspond to the folder *C:\websites\mywebsite* on the server.</span></span>
- <span data-ttu-id="3b50e-306">Un chemin d’accès virtuel est raccourci pour représenter les chemins d’accès dans le code sans avoir à spécifier le chemin d’accès complet.</span><span class="sxs-lookup"><span data-stu-id="3b50e-306">A virtual path is shorthand to represent paths in code without having to specify the full path.</span></span> <span data-ttu-id="3b50e-307">Il comprend la partie d’une URL qui suit le nom de domaine ou de serveur.</span><span class="sxs-lookup"><span data-stu-id="3b50e-307">It includes the portion of a URL that follows the domain or server name.</span></span> <span data-ttu-id="3b50e-308">Lorsque vous utilisez des chemins d’accès virtuels, vous pouvez déplacer votre code vers un autre domaine ou serveur sans avoir à mettre à jour les chemins d’accès.</span><span class="sxs-lookup"><span data-stu-id="3b50e-308">When you use virtual paths, you can move your code to a different domain or server without having to update the paths.</span></span>

<span data-ttu-id="3b50e-309">Voici un exemple pour vous aider à comprendre les différences :</span><span class="sxs-lookup"><span data-stu-id="3b50e-309">Here's an example to help you understand the differences:</span></span>

| <span data-ttu-id="3b50e-310">URL complète</span><span class="sxs-lookup"><span data-stu-id="3b50e-310">Complete URL</span></span> | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| <span data-ttu-id="3b50e-311">Nom du serveur</span><span class="sxs-lookup"><span data-stu-id="3b50e-311">Server name</span></span> | <span data-ttu-id="3b50e-312">*mycompanyserver*</span><span class="sxs-lookup"><span data-stu-id="3b50e-312">*mycompanyserver*</span></span> |
| <span data-ttu-id="3b50e-313">Chemin d'accès virtuel</span><span class="sxs-lookup"><span data-stu-id="3b50e-313">Virtual path</span></span> | <span data-ttu-id="3b50e-314">*/humanresources/CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="3b50e-314">*/humanresources/CompanyPolicy.htm*</span></span> |
| <span data-ttu-id="3b50e-315">Chemin d’accès physique</span><span class="sxs-lookup"><span data-stu-id="3b50e-315">Physical path</span></span> | <span data-ttu-id="3b50e-316">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="3b50e-316">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span></span> |

<span data-ttu-id="3b50e-317">La racine virtuelle est/, tout comme la racine de votre lecteur C :.</span><span class="sxs-lookup"><span data-stu-id="3b50e-317">The virtual root is /, just like the root of your C: drive is \.</span></span> <span data-ttu-id="3b50e-318">(Les chemins d’accès aux dossiers virtuels utilisent toujours des barres obliques.) Le chemin d’accès virtuel d’un dossier ne doit pas nécessairement avoir le même nom que le dossier physique ; Il peut s’agir d’un alias.</span><span class="sxs-lookup"><span data-stu-id="3b50e-318">(Virtual folder paths always use forward slashes.) The virtual path of a folder doesn't have to have the same name as the physical folder; it can be an alias.</span></span> <span data-ttu-id="3b50e-319">(Sur les serveurs de production, le chemin d’accès virtuel correspond rarement à un chemin d’accès physique exact.)</span><span class="sxs-lookup"><span data-stu-id="3b50e-319">(On production servers, the virtual path rarely matches an exact physical path.)</span></span>

<span data-ttu-id="3b50e-320">Lorsque vous travaillez avec des fichiers et des dossiers dans le code, vous devez parfois faire référence au chemin d’accès physique et parfois à un chemin d’accès virtuel, selon les objets que vous utilisez.</span><span class="sxs-lookup"><span data-stu-id="3b50e-320">When you work with files and folders in code, sometimes you need to reference the physical path and sometimes a virtual path, depending on what objects you're working with.</span></span> <span data-ttu-id="3b50e-321">ASP.NET vous offre les outils nécessaires pour utiliser les chemins d’accès aux fichiers et aux dossiers dans le code : la méthode `Server.MapPath`, ainsi que l’opérateur `~` et la méthode `Href`.</span><span class="sxs-lookup"><span data-stu-id="3b50e-321">ASP.NET gives you these tools for working with file and folder paths in code: the `Server.MapPath` method, and the `~` operator and `Href` method.</span></span>

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a><span data-ttu-id="3b50e-322">Conversion de chemins d’accès virtuels en éléments physiques : méthode Server. MapPath</span><span class="sxs-lookup"><span data-stu-id="3b50e-322">Converting virtual to physical paths: the Server.MapPath method</span></span>

<span data-ttu-id="3b50e-323">La méthode `Server.MapPath` convertit un chemin d’accès virtuel (par exemple, */default.cshtml*) en chemin d’accès physique absolu (comme *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="3b50e-323">The `Server.MapPath` method converts a virtual path (like */default.cshtml*) to an absolute physical path (like *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span></span> <span data-ttu-id="3b50e-324">Vous utilisez cette méthode chaque fois que vous avez besoin d’un chemin d’accès physique complet.</span><span class="sxs-lookup"><span data-stu-id="3b50e-324">You use this method any time you need a complete physical path.</span></span> <span data-ttu-id="3b50e-325">Voici un exemple typique de la lecture ou de l’écriture d’un fichier texte ou d’un fichier image sur le serveur Web.</span><span class="sxs-lookup"><span data-stu-id="3b50e-325">A typical example is when you're reading or writing a text file or image file on the web server.</span></span>

<span data-ttu-id="3b50e-326">En général, vous ne connaissez pas le chemin d’accès physique absolu de votre site sur le serveur d’un site d’hébergement. cette méthode peut donc convertir le chemin d’accès virtuel (le chemin d’accès virtuel) vers le chemin d’accès correspondant sur le serveur pour vous.</span><span class="sxs-lookup"><span data-stu-id="3b50e-326">You typically don't know the absolute physical path of your site on a hosting site's server, so this method can convert the path you do know — the virtual path — to the corresponding path on the server for you.</span></span> <span data-ttu-id="3b50e-327">Vous transmettez le chemin d’accès virtuel d’un fichier ou d’un dossier à la méthode et il retourne le chemin d’accès physique :</span><span class="sxs-lookup"><span data-stu-id="3b50e-327">You pass the virtual path to a file or folder to the method, and it returns the physical path:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a><span data-ttu-id="3b50e-328">Référencement de la racine virtuelle : l’opérateur ~ et la méthode href</span><span class="sxs-lookup"><span data-stu-id="3b50e-328">Referencing the virtual root: the ~ operator and Href method</span></span>

<span data-ttu-id="3b50e-329">Dans un fichier *. cshtml* ou *. vbhtml* , vous pouvez référencer le chemin d’accès de la racine virtuelle à l’aide de l’opérateur `~`.</span><span class="sxs-lookup"><span data-stu-id="3b50e-329">In a *.cshtml* or *.vbhtml* file, you can reference the virtual root path using the `~` operator.</span></span> <span data-ttu-id="3b50e-330">C’est très pratique, car vous pouvez déplacer des pages dans un site et tous les liens qu’ils contiennent vers d’autres pages ne sont pas rompus.</span><span class="sxs-lookup"><span data-stu-id="3b50e-330">This is very handy because you can move pages around in a site, and any links they contain to other pages won't be broken.</span></span> <span data-ttu-id="3b50e-331">C’est également pratique si vous déplacez votre site Web vers un autre emplacement.</span><span class="sxs-lookup"><span data-stu-id="3b50e-331">It's also handy in case you ever move your website to a different location.</span></span> <span data-ttu-id="3b50e-332">Voici quelques exemples :</span><span class="sxs-lookup"><span data-stu-id="3b50e-332">Here are some examples:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

<span data-ttu-id="3b50e-333">Si le site Web est `http://myserver/myapp`, voici comment ASP.NET traite ces chemins lors de l’exécution de la page :</span><span class="sxs-lookup"><span data-stu-id="3b50e-333">If the website is `http://myserver/myapp`, here's how ASP.NET will treat these paths when the page runs:</span></span>

- <span data-ttu-id="3b50e-334">`myImagesFolder`: `http://myserver/myapp/images`</span><span class="sxs-lookup"><span data-stu-id="3b50e-334">`myImagesFolder`: `http://myserver/myapp/images`</span></span>
- <span data-ttu-id="3b50e-335">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span><span class="sxs-lookup"><span data-stu-id="3b50e-335">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span></span>

<span data-ttu-id="3b50e-336">(Vous ne verrez pas ces chemins comme valeurs de la variable, mais ASP.NET traitera les chemins comme s’ils étaient.)</span><span class="sxs-lookup"><span data-stu-id="3b50e-336">(You won't actually see these paths as the values of the variable, but ASP.NET will treat the paths as if that's what they were.)</span></span>

<span data-ttu-id="3b50e-337">Vous pouvez utiliser l’opérateur `~` dans le code serveur (comme ci-dessus) et dans le balisage, comme suit :</span><span class="sxs-lookup"><span data-stu-id="3b50e-337">You can use the `~` operator both in server code (as above) and in markup, like this:</span></span>

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

<span data-ttu-id="3b50e-338">Dans le balisage, vous utilisez l’opérateur `~` pour créer des chemins d’accès à des ressources telles que des fichiers image, d’autres pages Web et des fichiers CSS.</span><span class="sxs-lookup"><span data-stu-id="3b50e-338">In markup, you use the `~` operator to create paths to resources like image files, other web pages, and CSS files.</span></span> <span data-ttu-id="3b50e-339">Lorsque la page s’exécute, ASP.NET parcourt la page (code et balisage) et résout toutes les références `~` au chemin d’accès approprié.</span><span class="sxs-lookup"><span data-stu-id="3b50e-339">When the page runs, ASP.NET looks through the page (both code and markup) and resolves all the `~` references to the appropriate path.</span></span>

## <a name="conditional-logic-and-loops"></a><span data-ttu-id="3b50e-340">Logique conditionnelle et boucles</span><span class="sxs-lookup"><span data-stu-id="3b50e-340">Conditional Logic and Loops</span></span>

<span data-ttu-id="3b50e-341">ASP.NET Server Code vous permet d’effectuer des tâches en fonction de conditions et d’écrire du code qui répète des instructions un nombre spécifique de fois, le code qui exécute une boucle).</span><span class="sxs-lookup"><span data-stu-id="3b50e-341">ASP.NET server code lets you perform tasks based on conditions and write code that repeats statements a specific number of times that is, code that runs a loop).</span></span>

### <a name="testing-conditions"></a><span data-ttu-id="3b50e-342">Conditions de test</span><span class="sxs-lookup"><span data-stu-id="3b50e-342">Testing conditions</span></span>

<span data-ttu-id="3b50e-343">Pour tester une condition simple, vous utilisez l’instruction `If...Then`, qui retourne `True` ou `False` en fonction d’un test que vous spécifiez :</span><span class="sxs-lookup"><span data-stu-id="3b50e-343">To test a simple condition you use the `If...Then` statement, which returns `True` or `False` based on a test you specify:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

<span data-ttu-id="3b50e-344">Le mot clé `If` démarre un bloc.</span><span class="sxs-lookup"><span data-stu-id="3b50e-344">The `If` keyword starts a block.</span></span> <span data-ttu-id="3b50e-345">Le test réel (condition) suit le mot clé `If` et retourne true ou false.</span><span class="sxs-lookup"><span data-stu-id="3b50e-345">The actual test (condition) follows the `If` keyword and returns true or false.</span></span> <span data-ttu-id="3b50e-346">L’instruction `If` se termine par `Then`.</span><span class="sxs-lookup"><span data-stu-id="3b50e-346">The `If` statement ends with `Then`.</span></span> <span data-ttu-id="3b50e-347">Les instructions qui s’exécutent si le test a la valeur true sont encadrées par `If` et `End If`.</span><span class="sxs-lookup"><span data-stu-id="3b50e-347">The statements that will run if the test is true are enclosed by `If` and `End If`.</span></span> <span data-ttu-id="3b50e-348">Une instruction `If` peut inclure un bloc `Else` qui spécifie les instructions à exécuter si la condition a la valeur false :</span><span class="sxs-lookup"><span data-stu-id="3b50e-348">An `If` statement can include an `Else` block that specifies statements to run if the condition is false:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

<span data-ttu-id="3b50e-349">Si une instruction `If` démarre un bloc de code, vous n’êtes pas obligé d’utiliser les instructions `Code...End Code` normales pour inclure les blocs.</span><span class="sxs-lookup"><span data-stu-id="3b50e-349">If an `If` statement starts a code block, you don't have to use the normal `Code...End Code` statements to include the blocks.</span></span> <span data-ttu-id="3b50e-350">Vous pouvez simplement ajouter `@` au bloc et cela fonctionnera.</span><span class="sxs-lookup"><span data-stu-id="3b50e-350">You can just add `@` to the block, and it will work.</span></span> <span data-ttu-id="3b50e-351">Cette approche fonctionne avec `If` ainsi que d’autres mots clés de programmation Visual Basic qui sont suivis de blocs de code, notamment `For`, `For Each`, `Do While`, etc.</span><span class="sxs-lookup"><span data-stu-id="3b50e-351">This approach works with `If` as well as other Visual Basic programming keywords that are followed by code blocks, including `For`, `For Each`, `Do While`, etc.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

<span data-ttu-id="3b50e-352">Vous pouvez ajouter plusieurs conditions à l’aide d’un ou de plusieurs blocs `ElseIf` :</span><span class="sxs-lookup"><span data-stu-id="3b50e-352">You can add multiple conditions using one or more `ElseIf` blocks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

<span data-ttu-id="3b50e-353">Dans cet exemple, si la première condition du bloc `If` n’est pas vraie, la condition `ElseIf` est vérifiée.</span><span class="sxs-lookup"><span data-stu-id="3b50e-353">In this example, if the first condition in the `If` block is not true, the `ElseIf` condition is checked.</span></span> <span data-ttu-id="3b50e-354">Si cette condition est remplie, les instructions du bloc `ElseIf` sont exécutées.</span><span class="sxs-lookup"><span data-stu-id="3b50e-354">If that condition is met, the statements in the `ElseIf` block are executed.</span></span> <span data-ttu-id="3b50e-355">Si aucune des conditions n’est remplie, les instructions du bloc `Else` sont exécutées.</span><span class="sxs-lookup"><span data-stu-id="3b50e-355">If none of the conditions are met, the statements in the `Else` block are executed.</span></span> <span data-ttu-id="3b50e-356">Vous pouvez ajouter un nombre quelconque de blocs `ElseIf`, puis fermer avec un bloc `Else` en tant que &quot;tout le reste&quot; condition.</span><span class="sxs-lookup"><span data-stu-id="3b50e-356">You can add any number of `ElseIf` blocks, and then close with an `Else` block as the &quot;everything else&quot; condition.</span></span>

<span data-ttu-id="3b50e-357">Pour tester un grand nombre de conditions, utilisez un bloc `Select Case` :</span><span class="sxs-lookup"><span data-stu-id="3b50e-357">To test a large number of conditions, use a `Select Case` block:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

<span data-ttu-id="3b50e-358">La valeur à tester est entre parenthèses (dans l’exemple, la variable Weekday).</span><span class="sxs-lookup"><span data-stu-id="3b50e-358">The value to test is in parentheses (in the example, the weekday variable).</span></span> <span data-ttu-id="3b50e-359">Chaque test individuel utilise une instruction `Case` qui répertorie une valeur.</span><span class="sxs-lookup"><span data-stu-id="3b50e-359">Each individual test uses a `Case` statement that lists a value.</span></span> <span data-ttu-id="3b50e-360">Si la valeur d’une instruction `Case` correspond à la valeur de test, le code de ce `Case` bloc est exécuté.</span><span class="sxs-lookup"><span data-stu-id="3b50e-360">If the value of a `Case` statement matches the test value, the code in that `Case` block is executed.</span></span>

<span data-ttu-id="3b50e-361">Résultat des deux derniers blocs conditionnels affichés dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="3b50e-361">The result of the last two conditional blocks displayed in a browser:</span></span>

![Rasoir-Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a><span data-ttu-id="3b50e-363">Code en boucle</span><span class="sxs-lookup"><span data-stu-id="3b50e-363">Looping code</span></span>

<span data-ttu-id="3b50e-364">Vous devez souvent exécuter les mêmes instructions à plusieurs reprises.</span><span class="sxs-lookup"><span data-stu-id="3b50e-364">You often need to run the same statements repeatedly.</span></span> <span data-ttu-id="3b50e-365">Pour ce faire, vous devez effectuer une boucle.</span><span class="sxs-lookup"><span data-stu-id="3b50e-365">You do this by looping.</span></span> <span data-ttu-id="3b50e-366">Par exemple, vous exécutez souvent les mêmes instructions pour chaque élément d’une collection de données.</span><span class="sxs-lookup"><span data-stu-id="3b50e-366">For example, you often run the same statements for each item in a collection of data.</span></span> <span data-ttu-id="3b50e-367">Si vous savez exactement combien de fois vous souhaitez effectuer une boucle, vous pouvez utiliser une boucle `For`.</span><span class="sxs-lookup"><span data-stu-id="3b50e-367">If you know exactly how many times you want to loop, you can use a `For` loop.</span></span> <span data-ttu-id="3b50e-368">Ce type de boucle est particulièrement utile pour le comptage ou le comptage :</span><span class="sxs-lookup"><span data-stu-id="3b50e-368">This kind of loop is especially useful for counting up or counting down:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

<span data-ttu-id="3b50e-369">La boucle commence par le mot clé `For`, suivi de trois éléments :</span><span class="sxs-lookup"><span data-stu-id="3b50e-369">The loop begins with the `For` keyword, followed by three elements:</span></span>

- <span data-ttu-id="3b50e-370">Immédiatement après l’instruction `For`, vous déclarez une variable de compteur (vous n’êtes pas obligé d’utiliser `Dim`), puis vous indiquez la plage, comme dans `i = 10 to 20`.</span><span class="sxs-lookup"><span data-stu-id="3b50e-370">Immediately after the `For` statement, you declare a counter variable (you don't have to use `Dim`) and then indicate the range, as in `i = 10 to 20`.</span></span> <span data-ttu-id="3b50e-371">Cela signifie que la variable `i` commence à compter 10 et continue jusqu’à ce qu’elle atteigne 20 (inclusive).</span><span class="sxs-lookup"><span data-stu-id="3b50e-371">This means the variable `i` will start counting at 10 and continue until it reaches 20 (inclusive).</span></span>
- <span data-ttu-id="3b50e-372">Entre les instructions `For` et `Next` correspond au contenu du bloc.</span><span class="sxs-lookup"><span data-stu-id="3b50e-372">Between the `For` and `Next` statements is the content of the block.</span></span> <span data-ttu-id="3b50e-373">Il peut contenir une ou plusieurs instructions de code qui s’exécutent avec chaque boucle.</span><span class="sxs-lookup"><span data-stu-id="3b50e-373">This can contain one or more code statements that execute with each loop.</span></span>
- <span data-ttu-id="3b50e-374">L’instruction `Next i` termine la boucle.</span><span class="sxs-lookup"><span data-stu-id="3b50e-374">The `Next i` statement ends the loop.</span></span> <span data-ttu-id="3b50e-375">Elle incrémente le compteur et démarre l’itération suivante de la boucle.</span><span class="sxs-lookup"><span data-stu-id="3b50e-375">It increments the counter and starts the next iteration of the loop.</span></span>

<span data-ttu-id="3b50e-376">La ligne de code entre les lignes `For` et `Next` contient le code qui s’exécute pour chaque itération de la boucle.</span><span class="sxs-lookup"><span data-stu-id="3b50e-376">The line of code between the `For` and `Next` lines contains the code that runs for each iteration of the loop.</span></span> <span data-ttu-id="3b50e-377">Le balisage crée chaque fois un nouveau paragraphe (`<p>` élément) et ajoute une ligne à la sortie, en affichant la valeur i (le compteur).</span><span class="sxs-lookup"><span data-stu-id="3b50e-377">The markup creates a new paragraph (`<p>` element) each time and adds a line to the output, displaying the value of i (the counter).</span></span> <span data-ttu-id="3b50e-378">Lorsque vous exécutez cette page, l’exemple crée 11 lignes affichant la sortie, avec le texte de chaque ligne indiquant le numéro de l’élément.</span><span class="sxs-lookup"><span data-stu-id="3b50e-378">When you run this page, the example creates 11 lines displaying the output, with the text in each line indicating the item number.</span></span>

![Razor-Img11](introducing-razor-syntax-vb/_static/image11.jpg)

<span data-ttu-id="3b50e-380">Si vous utilisez une collection ou un tableau, vous utilisez souvent une boucle `For Each`.</span><span class="sxs-lookup"><span data-stu-id="3b50e-380">If you're working with a collection or array, you often use a `For Each` loop.</span></span> <span data-ttu-id="3b50e-381">Une collection est un groupe d’objets similaires, et la boucle `For Each` vous permet d’effectuer une tâche sur chaque élément de la collection.</span><span class="sxs-lookup"><span data-stu-id="3b50e-381">A collection is a group of similar objects, and the `For Each` loop lets you carry out a task on each item in the collection.</span></span> <span data-ttu-id="3b50e-382">Ce type de boucle est pratique pour les collections, car contrairement à une boucle `For`, il n’est pas nécessaire d’incrémenter le compteur ou de définir une limite.</span><span class="sxs-lookup"><span data-stu-id="3b50e-382">This type of loop is convenient for collections, because unlike a `For` loop, you don't have to increment the counter or set a limit.</span></span> <span data-ttu-id="3b50e-383">Au lieu de cela, le `For Each` code de boucle passe simplement par la collection jusqu’à ce qu’il soit terminé.</span><span class="sxs-lookup"><span data-stu-id="3b50e-383">Instead, the `For Each` loop code simply proceeds through the collection until it's finished.</span></span>

<span data-ttu-id="3b50e-384">Cet exemple retourne les éléments de la collection `Request.ServerVariables` (qui contient des informations sur votre serveur Web).</span><span class="sxs-lookup"><span data-stu-id="3b50e-384">This example returns the items in the `Request.ServerVariables` collection (which contains information about your web server).</span></span> <span data-ttu-id="3b50e-385">Elle utilise une boucle `For Each` pour afficher le nom de chaque élément en créant un nouvel élément `<li>` dans une liste à puces HTML.</span><span class="sxs-lookup"><span data-stu-id="3b50e-385">It uses a `For Each` loop to display the name of each item by creating a new `<li>` element in an HTML bulleted list.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

<span data-ttu-id="3b50e-386">Le mot clé `For Each` est suivi d’une variable qui représente un élément unique dans la collection (dans l’exemple, `myItem`), suivi du mot clé `In`, suivi de la collection dans laquelle vous souhaitez effectuer une boucle.</span><span class="sxs-lookup"><span data-stu-id="3b50e-386">The `For Each` keyword is followed by a variable that represents a single item in the collection (in the example, `myItem`), followed by the `In` keyword, followed by the collection you want to loop through.</span></span> <span data-ttu-id="3b50e-387">Dans le corps de la boucle `For Each`, vous pouvez accéder à l’élément actuel à l’aide de la variable que vous avez déclarée précédemment.</span><span class="sxs-lookup"><span data-stu-id="3b50e-387">In the body of the `For Each` loop, you can access the current item using the variable that you declared earlier.</span></span>

![Rasoir-Img12](introducing-razor-syntax-vb/_static/image12.jpg)

<span data-ttu-id="3b50e-389">Pour créer une boucle plus générale, utilisez l’instruction `Do While` :</span><span class="sxs-lookup"><span data-stu-id="3b50e-389">To create a more general-purpose loop, use the `Do While` statement:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

<span data-ttu-id="3b50e-390">Cette boucle commence par le mot clé `Do While`, suivi d’une condition, suivi du bloc à répéter.</span><span class="sxs-lookup"><span data-stu-id="3b50e-390">This loop begins with the `Do While` keyword, followed by a condition, followed by the block to repeat.</span></span> <span data-ttu-id="3b50e-391">En général, les boucles incrémentent (ajoutent à) ou décrémente (soustrait de) une variable ou un objet utilisé pour le comptage.</span><span class="sxs-lookup"><span data-stu-id="3b50e-391">Loops typically increment (add to) or decrement (subtract from) a variable or object used for counting.</span></span> <span data-ttu-id="3b50e-392">Dans l’exemple, l’opérateur `+=` ajoute 1 à la valeur d’une variable chaque fois que la boucle s’exécute.</span><span class="sxs-lookup"><span data-stu-id="3b50e-392">In the example, the `+=` operator adds 1 to the value of a variable each time the loop runs.</span></span> <span data-ttu-id="3b50e-393">(Pour décrémenter une variable dans une boucle qui est décomptée, vous devez utiliser l’opérateur de décrémentation `-=`.)</span><span class="sxs-lookup"><span data-stu-id="3b50e-393">(To decrement a variable in a loop that counts down, you would use the decrement operator `-=`.)</span></span>

## <a name="objects-and-collections"></a><span data-ttu-id="3b50e-394">Objets et collections</span><span class="sxs-lookup"><span data-stu-id="3b50e-394">Objects and Collections</span></span>

<span data-ttu-id="3b50e-395">Presque tout ce qui se trouve dans un site Web ASP.NET est un objet, y compris la page Web elle-même.</span><span class="sxs-lookup"><span data-stu-id="3b50e-395">Nearly everything in an ASP.NET website is an object, including the web page itself.</span></span> <span data-ttu-id="3b50e-396">Cette section décrit certains objets importants que vous allez utiliser fréquemment dans votre code.</span><span class="sxs-lookup"><span data-stu-id="3b50e-396">This section discusses some important objects you'll work with frequently in your code.</span></span>

### <a name="page-objects"></a><span data-ttu-id="3b50e-397">Objets page</span><span class="sxs-lookup"><span data-stu-id="3b50e-397">Page objects</span></span>

<span data-ttu-id="3b50e-398">L’objet le plus basique dans ASP.NET est la page.</span><span class="sxs-lookup"><span data-stu-id="3b50e-398">The most basic object in ASP.NET is the page.</span></span> <span data-ttu-id="3b50e-399">Vous pouvez accéder aux propriétés de l’objet page directement sans aucun objet éligible.</span><span class="sxs-lookup"><span data-stu-id="3b50e-399">You can access properties of the page object directly without any qualifying object.</span></span> <span data-ttu-id="3b50e-400">Le code suivant obtient le chemin d’accès au fichier de la page, à l’aide de l’objet `Request` de la page :</span><span class="sxs-lookup"><span data-stu-id="3b50e-400">The following code gets the page's file path, using the `Request` object of the page:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

<span data-ttu-id="3b50e-401">Vous pouvez utiliser les propriétés de l’objet `Page` pour obtenir un grand nombre d’informations, telles que :</span><span class="sxs-lookup"><span data-stu-id="3b50e-401">You can use properties of the `Page` object to get a lot of information, such as:</span></span>

- <span data-ttu-id="3b50e-402">`Request`.</span><span class="sxs-lookup"><span data-stu-id="3b50e-402">`Request`.</span></span> <span data-ttu-id="3b50e-403">Comme vous l’avez déjà vu, il s’agit d’une collection d’informations sur la requête actuelle, notamment le type de navigateur qui a effectué la demande, l’URL de la page, l’identité de l’utilisateur, etc.</span><span class="sxs-lookup"><span data-stu-id="3b50e-403">As you've already seen, this is a collection of information about the current request, including what type of browser made the request, the URL of the page, the user identity, etc.</span></span>
- <span data-ttu-id="3b50e-404">`Response`.</span><span class="sxs-lookup"><span data-stu-id="3b50e-404">`Response`.</span></span> <span data-ttu-id="3b50e-405">Il s’agit d’une collection d’informations sur la réponse (page) qui seront envoyées au navigateur une fois l’exécution du code serveur terminée.</span><span class="sxs-lookup"><span data-stu-id="3b50e-405">This is a collection of information about the response (page) that will be sent to the browser when the server code has finished running.</span></span> <span data-ttu-id="3b50e-406">Par exemple, vous pouvez utiliser cette propriété pour écrire des informations dans la réponse.</span><span class="sxs-lookup"><span data-stu-id="3b50e-406">For example, you can use this property to write information into the response.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a><span data-ttu-id="3b50e-407">Objets de collection (tableaux et dictionnaires)</span><span class="sxs-lookup"><span data-stu-id="3b50e-407">Collection objects (arrays and dictionaries)</span></span>

<span data-ttu-id="3b50e-408">Une collection est un groupe d’objets du même type, par exemple une collection d’objets `Customer` à partir d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="3b50e-408">A collection is a group of objects of the same type, such as a collection of `Customer` objects from a database.</span></span> <span data-ttu-id="3b50e-409">ASP.NET contient de nombreuses collections intégrées, telles que la collection `Request.Files`.</span><span class="sxs-lookup"><span data-stu-id="3b50e-409">ASP.NET contains many built-in collections, like the `Request.Files` collection.</span></span>

<span data-ttu-id="3b50e-410">Vous utiliserez souvent des données dans des collections.</span><span class="sxs-lookup"><span data-stu-id="3b50e-410">You'll often work with data in collections.</span></span> <span data-ttu-id="3b50e-411">Deux types de collections courants sont le *tableau* et le *dictionnaire*.</span><span class="sxs-lookup"><span data-stu-id="3b50e-411">Two common collection types are the *array* and the *dictionary*.</span></span> <span data-ttu-id="3b50e-412">Un tableau est utile lorsque vous souhaitez stocker une collection d’éléments similaires, mais que vous ne voulez pas créer une variable distincte pour contenir chaque élément :</span><span class="sxs-lookup"><span data-stu-id="3b50e-412">An array is useful when you want to store a collection of similar items but don't want to create a separate variable to hold each item:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

<span data-ttu-id="3b50e-413">Avec les tableaux, vous déclarez un type de données spécifique, tel que `String`, `Integer`ou `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="3b50e-413">With arrays, you declare a specific data type, such as `String`, `Integer`, or `DateTime`.</span></span> <span data-ttu-id="3b50e-414">Pour indiquer que la variable peut contenir un tableau, vous ajoutez des parenthèses au nom de la variable dans la déclaration (par exemple `Dim myVar() As String`).</span><span class="sxs-lookup"><span data-stu-id="3b50e-414">To indicate that the variable can contain an array, you add parentheses to the variable name in the declaration (such as `Dim myVar() As String`).</span></span> <span data-ttu-id="3b50e-415">Vous pouvez accéder aux éléments d’un tableau à l’aide de leur position (index) ou à l’aide de l’instruction `For Each`.</span><span class="sxs-lookup"><span data-stu-id="3b50e-415">You can access items in an array using their position (index) or by using the `For Each` statement.</span></span> <span data-ttu-id="3b50e-416">Les index de tableau sont de base &#8212; zéro, c’est-à-dire que le premier élément est à la position 0, le deuxième à la position 1, et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="3b50e-416">Array indexes are zero-based &#8212; that is, the first item is at position 0, the second item is at position 1, and so on.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

<span data-ttu-id="3b50e-417">Vous pouvez déterminer le nombre d’éléments d’un tableau en obtenant sa propriété `Length`.</span><span class="sxs-lookup"><span data-stu-id="3b50e-417">You can determine the number of items in an array by getting its `Length` property.</span></span> <span data-ttu-id="3b50e-418">Pour obtenir la position d’un élément spécifique dans le tableau (autrement dit, pour effectuer une recherche dans le tableau), utilisez la méthode `Array.IndexOf`.</span><span class="sxs-lookup"><span data-stu-id="3b50e-418">To get the position of a specific item in the array (that is, to search the array), use the `Array.IndexOf` method.</span></span> <span data-ttu-id="3b50e-419">Vous pouvez également effectuer des opérations telles que inverser le contenu d’un tableau (méthode `Array.Reverse`) ou trier le contenu (méthode `Array.Sort`).</span><span class="sxs-lookup"><span data-stu-id="3b50e-419">You can also do things like reverse the contents of an array (the `Array.Reverse` method) or sort the contents (the `Array.Sort` method).</span></span>

<span data-ttu-id="3b50e-420">Sortie du code du tableau de chaînes affiché dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="3b50e-420">The output of the string array code displayed in a browser:</span></span>

![Rasoir-Img13](introducing-razor-syntax-vb/_static/image13.jpg)

<span data-ttu-id="3b50e-422">Un dictionnaire est une collection de paires clé/valeur, dans laquelle vous fournissez la clé (ou le nom) pour définir ou récupérer la valeur correspondante :</span><span class="sxs-lookup"><span data-stu-id="3b50e-422">A dictionary is a collection of key/value pairs, where you provide the key (or name) to set or retrieve the corresponding value:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

<span data-ttu-id="3b50e-423">Pour créer un dictionnaire, vous utilisez le mot clé `New` pour indiquer que vous créez un nouvel objet `Dictionary`.</span><span class="sxs-lookup"><span data-stu-id="3b50e-423">To create a dictionary, you use the `New` keyword to indicate that you're creating a new `Dictionary` object.</span></span> <span data-ttu-id="3b50e-424">Vous pouvez assigner un dictionnaire à une variable à l’aide du mot clé `Dim`.</span><span class="sxs-lookup"><span data-stu-id="3b50e-424">You can assign a dictionary to a variable using the `Dim` keyword.</span></span> <span data-ttu-id="3b50e-425">Vous indiquez les types de données des éléments du dictionnaire à l’aide de parenthèses (`( )`).</span><span class="sxs-lookup"><span data-stu-id="3b50e-425">You indicate the data types of the items in the dictionary using parentheses ( `( )` ).</span></span> <span data-ttu-id="3b50e-426">À la fin de la déclaration, vous devez ajouter une autre paire de parenthèses, car il s’agit en fait d’une méthode qui crée un nouveau dictionnaire.</span><span class="sxs-lookup"><span data-stu-id="3b50e-426">At the end of the declaration, you must add another pair of parentheses, because this is actually a method that creates a new dictionary.</span></span>

<span data-ttu-id="3b50e-427">Pour ajouter des éléments au dictionnaire, vous pouvez appeler la méthode `Add` de la variable dictionnaire (`myScores` dans ce cas), puis spécifier une clé et une valeur.</span><span class="sxs-lookup"><span data-stu-id="3b50e-427">To add items to the dictionary, you can call the `Add` method of the dictionary variable (`myScores` in this case), and then specify a key and a value.</span></span> <span data-ttu-id="3b50e-428">Vous pouvez également utiliser des parenthèses pour indiquer la clé et effectuer une assignation simple, comme dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="3b50e-428">Alternatively, you can use parentheses to indicate the key and do a simple assignment, as in the following example:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

<span data-ttu-id="3b50e-429">Pour obtenir une valeur à partir du dictionnaire, vous spécifiez la clé entre parenthèses :</span><span class="sxs-lookup"><span data-stu-id="3b50e-429">To get a value from the dictionary, you specify the key in parentheses:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a><span data-ttu-id="3b50e-430">Appel de méthodes avec des paramètres</span><span class="sxs-lookup"><span data-stu-id="3b50e-430">Calling Methods with Parameters</span></span>

<span data-ttu-id="3b50e-431">Comme vous l’avez vu précédemment dans cet article, les objets que vous programmez ont des méthodes.</span><span class="sxs-lookup"><span data-stu-id="3b50e-431">As you saw earlier in this article, the objects that you program with have methods.</span></span> <span data-ttu-id="3b50e-432">Par exemple, un objet `Database` peut avoir une méthode `Database.Connect`.</span><span class="sxs-lookup"><span data-stu-id="3b50e-432">For example, a `Database` object might have a `Database.Connect` method.</span></span> <span data-ttu-id="3b50e-433">De nombreuses méthodes possèdent également un ou plusieurs paramètres.</span><span class="sxs-lookup"><span data-stu-id="3b50e-433">Many methods also have one or more parameters.</span></span> <span data-ttu-id="3b50e-434">Un *paramètre* est une valeur que vous transmettez à une méthode pour permettre à la méthode d’effectuer sa tâche.</span><span class="sxs-lookup"><span data-stu-id="3b50e-434">A *parameter* is a value that you pass to a method to enable the method to complete its task.</span></span> <span data-ttu-id="3b50e-435">Par exemple, examinez une déclaration pour la méthode `Request.MapPath`, qui accepte trois paramètres :</span><span class="sxs-lookup"><span data-stu-id="3b50e-435">For example, look at a declaration for the `Request.MapPath` method, which takes three parameters:</span></span>

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

<span data-ttu-id="3b50e-436">Cette méthode retourne le chemin d’accès physique sur le serveur qui correspond à un chemin d’accès virtuel spécifié.</span><span class="sxs-lookup"><span data-stu-id="3b50e-436">This method returns the physical path on the server that corresponds to a specified virtual path.</span></span> <span data-ttu-id="3b50e-437">Les trois paramètres de la méthode sont `virtualPath`, `baseVirtualDir`et `allowCrossAppMapping`.</span><span class="sxs-lookup"><span data-stu-id="3b50e-437">The three parameters for the method are `virtualPath`, `baseVirtualDir`, and `allowCrossAppMapping`.</span></span> <span data-ttu-id="3b50e-438">(Notez que dans la déclaration, les paramètres sont répertoriés avec les types de données des données qu’ils accepteront.) Lorsque vous appelez cette méthode, vous devez fournir des valeurs pour les trois paramètres.</span><span class="sxs-lookup"><span data-stu-id="3b50e-438">(Notice that in the declaration, the parameters are listed with the data types of the data that they'll accept.) When you call this method, you must supply values for all three parameters.</span></span>

<span data-ttu-id="3b50e-439">Lorsque vous utilisez Visual Basic avec le syntaxe Razor, vous avez deux options pour passer des paramètres à une méthode : les *paramètres positionnels* ou les *paramètres nommés*.</span><span class="sxs-lookup"><span data-stu-id="3b50e-439">When you're using Visual Basic with the Razor syntax, you have two options for passing parameters to a method: *positional parameters* or *named parameters*.</span></span> <span data-ttu-id="3b50e-440">Pour appeler une méthode à l’aide de paramètres positionnels, vous devez passer les paramètres dans un ordre strict spécifié dans la déclaration de méthode.</span><span class="sxs-lookup"><span data-stu-id="3b50e-440">To call a method using positional parameters, you pass the parameters in a strict order that's specified in the method declaration.</span></span> <span data-ttu-id="3b50e-441">(Vous connaissez généralement cet ordre en lisant la documentation de la méthode.) Vous devez suivre l’ordre et vous ne pouvez pas ignorer les paramètres &#8212; si nécessaire, vous transmettez une chaîne vide (`""`) ou null pour un paramètre positionnel dont vous n’avez pas de valeur.</span><span class="sxs-lookup"><span data-stu-id="3b50e-441">(You would typically know this order by reading documentation for the method.) You must follow the order, and you can't skip any of the parameters &#8212; if necessary, you pass an empty string (`""`) or null for a positional parameter that you don't have a value for.</span></span>

<span data-ttu-id="3b50e-442">L’exemple suivant suppose que vous avez un dossier nommé *scripts* sur votre site Web.</span><span class="sxs-lookup"><span data-stu-id="3b50e-442">The following example assumes you have a folder named *scripts* on your website.</span></span> <span data-ttu-id="3b50e-443">Le code appelle la méthode `Request.MapPath` et passe des valeurs pour les trois paramètres dans le bon ordre.</span><span class="sxs-lookup"><span data-stu-id="3b50e-443">The code calls the `Request.MapPath` method and passes values for the three parameters in the correct order.</span></span> <span data-ttu-id="3b50e-444">Il affiche ensuite le chemin d’accès mappé résultant.</span><span class="sxs-lookup"><span data-stu-id="3b50e-444">It then displays the resulting mapped path.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

<span data-ttu-id="3b50e-445">Lorsqu’il existe de nombreux paramètres pour une méthode, vous pouvez maintenir votre code plus propre et plus lisible en utilisant des paramètres nommés.</span><span class="sxs-lookup"><span data-stu-id="3b50e-445">When there are many parameters for a method, you can keep your code cleaner and more readable by using named parameters.</span></span> <span data-ttu-id="3b50e-446">Pour appeler une méthode à l’aide de paramètres nommés, spécifiez le nom du paramètre suivi de `:=`, puis fournissez la valeur.</span><span class="sxs-lookup"><span data-stu-id="3b50e-446">To call a method using named parameters, specify the parameter name followed by `:=` and then provide the value.</span></span> <span data-ttu-id="3b50e-447">L’un des avantages des paramètres nommés est que vous pouvez les ajouter dans l’ordre de votre choix.</span><span class="sxs-lookup"><span data-stu-id="3b50e-447">An advantage of named parameters is that you can add them in any order you want.</span></span> <span data-ttu-id="3b50e-448">(Un inconvénient est que l’appel de la méthode n’est pas aussi compact.)</span><span class="sxs-lookup"><span data-stu-id="3b50e-448">(A disadvantage is that the method call is not as compact.)</span></span>

<span data-ttu-id="3b50e-449">L’exemple suivant appelle la même méthode que ci-dessus, mais utilise des paramètres nommés pour fournir les valeurs :</span><span class="sxs-lookup"><span data-stu-id="3b50e-449">The following example calls the same method as above, but uses named parameters to supply the values:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

<span data-ttu-id="3b50e-450">Comme vous pouvez le voir, les paramètres sont passés dans un ordre différent.</span><span class="sxs-lookup"><span data-stu-id="3b50e-450">As you can see, the parameters are passed in a different order.</span></span> <span data-ttu-id="3b50e-451">Toutefois, si vous exécutez l’exemple précédent et cet exemple, ils retournent la même valeur.</span><span class="sxs-lookup"><span data-stu-id="3b50e-451">However, if you run the previous example and this example, they'll return the same value.</span></span>

## <a name="handling-errors"></a><span data-ttu-id="3b50e-452">Gestion des erreurs</span><span class="sxs-lookup"><span data-stu-id="3b50e-452">Handling Errors</span></span>

### <a name="try-catch-statements"></a><span data-ttu-id="3b50e-453">Instructions Try-Catch</span><span class="sxs-lookup"><span data-stu-id="3b50e-453">Try-Catch statements</span></span>

<span data-ttu-id="3b50e-454">Dans votre code, vous aurez souvent des instructions qui peuvent échouer pour des raisons extérieures à votre contrôle.</span><span class="sxs-lookup"><span data-stu-id="3b50e-454">You'll often have statements in your code that might fail for reasons outside your control.</span></span> <span data-ttu-id="3b50e-455">Exemple :</span><span class="sxs-lookup"><span data-stu-id="3b50e-455">For example:</span></span>

- <span data-ttu-id="3b50e-456">Si votre code tente d’ouvrir, de créer, de lire ou d’écrire un fichier, toutes sortes d’erreurs peuvent se produire.</span><span class="sxs-lookup"><span data-stu-id="3b50e-456">If your code tries to open, create, read, or write a file, all sorts of errors might occur.</span></span> <span data-ttu-id="3b50e-457">Le fichier que vous souhaitez peut-être n’existe pas, il est peut-être verrouillé, le code n’a peut-être pas d’autorisations, et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="3b50e-457">The file you want might not exist, it might be locked, the code might not have permissions, and so on.</span></span>
- <span data-ttu-id="3b50e-458">De même, si votre code tente de mettre à jour des enregistrements dans une base de données, il peut y avoir des problèmes d’autorisations, la connexion à la base de données peut être supprimée, les données à enregistrer peuvent ne pas être valides, etc.</span><span class="sxs-lookup"><span data-stu-id="3b50e-458">Similarly, if your code tries to update records in a database, there can be permissions issues, the connection to the database might be dropped, the data to save might be invalid, and so on.</span></span>

<span data-ttu-id="3b50e-459">En termes de programmation, ces situations sont appelées *exceptions*.</span><span class="sxs-lookup"><span data-stu-id="3b50e-459">In programming terms, these situations are called *exceptions*.</span></span> <span data-ttu-id="3b50e-460">Si votre code rencontre une exception, il génère (lève) un message d’erreur qui est, au mieux, ennuyeux pour les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="3b50e-460">If your code encounters an exception, it generates (throws) an error message that is, at best, annoying to users.</span></span>

![Razor-Img14](introducing-razor-syntax-vb/_static/image14.jpg)

<span data-ttu-id="3b50e-462">Dans les cas où votre code risque de rencontrer des exceptions et afin d’éviter les messages d’erreur de ce type, vous pouvez utiliser des instructions `Try/Catch`.</span><span class="sxs-lookup"><span data-stu-id="3b50e-462">In situations where your code might encounter exceptions, and in order to avoid error messages of this type, you can use `Try/Catch` statements.</span></span> <span data-ttu-id="3b50e-463">Dans l’instruction `Try`, vous exécutez le code que vous êtes en train de vérifier.</span><span class="sxs-lookup"><span data-stu-id="3b50e-463">In the `Try` statement, you run the code that you're checking.</span></span> <span data-ttu-id="3b50e-464">Dans une ou plusieurs instructions `Catch`, vous pouvez rechercher des erreurs spécifiques (types spécifiques d’exceptions) qui ont pu se produire.</span><span class="sxs-lookup"><span data-stu-id="3b50e-464">In one or more `Catch` statements, you can look for specific errors (specific types of exceptions) that might have occurred.</span></span> <span data-ttu-id="3b50e-465">Vous pouvez inclure autant d’instructions `Catch` que vous le souhaitez pour rechercher les erreurs que vous prévoyez d’anticiper.</span><span class="sxs-lookup"><span data-stu-id="3b50e-465">You can include as many `Catch` statements as you need to look for errors that you're anticipating.</span></span>

> [!NOTE]
> <span data-ttu-id="3b50e-466">Nous vous recommandons d’éviter d’utiliser la méthode `Response.Redirect` dans les instructions `Try/Catch`, car cela peut provoquer une exception dans votre page.</span><span class="sxs-lookup"><span data-stu-id="3b50e-466">We recommend that you avoid using the `Response.Redirect` method in `Try/Catch` statements, because it can cause an exception in your page.</span></span>

<span data-ttu-id="3b50e-467">L’exemple suivant montre une page qui crée un fichier texte lors de la première demande, puis affiche un bouton qui permet à l’utilisateur d’ouvrir le fichier.</span><span class="sxs-lookup"><span data-stu-id="3b50e-467">The following example shows a page that creates a text file on the first request and then displays a button that lets the user open the file.</span></span> <span data-ttu-id="3b50e-468">L’exemple utilise délibérément un nom de fichier incorrect afin qu’il génère une exception.</span><span class="sxs-lookup"><span data-stu-id="3b50e-468">The example deliberately uses a bad file name so that it will cause an exception.</span></span> <span data-ttu-id="3b50e-469">Le code comprend des instructions `Catch` pour deux exceptions possibles : `FileNotFoundException`, qui se produit si le nom de fichier est incorrect, et `DirectoryNotFoundException`, qui se produit si ASP.NET ne peut même pas trouver le dossier.</span><span class="sxs-lookup"><span data-stu-id="3b50e-469">The code includes `Catch` statements for two possible exceptions: `FileNotFoundException`, which occurs if the file name is bad, and `DirectoryNotFoundException`, which occurs if ASP.NET can't even find the folder.</span></span> <span data-ttu-id="3b50e-470">(Vous pouvez supprimer les marques de commentaire d’une instruction dans l’exemple afin de voir comment elle s’exécute quand tout fonctionne correctement.)</span><span class="sxs-lookup"><span data-stu-id="3b50e-470">(You can uncomment a statement in the example in order to see how it runs when everything works properly.)</span></span>

<span data-ttu-id="3b50e-471">Si votre code n’a pas géré l’exception, vous verrez une page d’erreur telle que la capture d’écran précédente.</span><span class="sxs-lookup"><span data-stu-id="3b50e-471">If your code didn't handle the exception, you would see an error page like the previous screen shot.</span></span> <span data-ttu-id="3b50e-472">Toutefois, la section `Try/Catch` permet d’empêcher l’utilisateur de voir ces types d’erreurs.</span><span class="sxs-lookup"><span data-stu-id="3b50e-472">However, the `Try/Catch` section helps prevent the user from seeing these types of errors.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a><span data-ttu-id="3b50e-473">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="3b50e-473">Additional Resources</span></span>

### <a name="reference-documentation"></a><span data-ttu-id="3b50e-474">Documentation de référence</span><span class="sxs-lookup"><span data-stu-id="3b50e-474">Reference Documentation</span></span>

- [<span data-ttu-id="3b50e-475">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3b50e-475">ASP.NET</span></span>](https://msdn.microsoft.com/library/ee532866.aspx)
- [<span data-ttu-id="3b50e-476">Langage Visual Basic</span><span class="sxs-lookup"><span data-stu-id="3b50e-476">Visual Basic Language</span></span>](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
