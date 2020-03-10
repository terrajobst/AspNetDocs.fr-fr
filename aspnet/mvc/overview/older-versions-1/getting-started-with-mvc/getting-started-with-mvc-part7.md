---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Ajout de la validation au modèle | Microsoft Docs
author: shanselman
description: Il s’agit d’un didacticiel débutant qui présente les notions de base de ASP.NET MVC. Créer une application Web simple qui lit et écrit à partir d’une base de données.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 9403be574324c34edf93bef1e0e4fd7ba68a3a9d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543644"
---
# <a name="adding-validation-to-the-model"></a><span data-ttu-id="99ef4-104">Ajout de la validation au modèle</span><span class="sxs-lookup"><span data-stu-id="99ef4-104">Adding Validation to the Model</span></span>

<span data-ttu-id="99ef4-105">par [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="99ef4-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="99ef4-106">Il s’agit d’un didacticiel débutant qui présente les notions de base de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="99ef4-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="99ef4-107">Vous allez créer une application Web simple qui lit et écrit à partir d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="99ef4-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="99ef4-108">Visitez le [Centre d’apprentissage ASP.NET MVC](../../../index.md) pour trouver d’autres didacticiels et exemples ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="99ef4-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>

<span data-ttu-id="99ef4-109">Dans cette section, nous allons implémenter la prise en charge nécessaire pour activer la validation des entrées dans notre application.</span><span class="sxs-lookup"><span data-stu-id="99ef4-109">In this section we are going to implement the support necessary to enable input validation within our application.</span></span> <span data-ttu-id="99ef4-110">Nous garantissons que notre contenu de base de données est toujours correct et fournissez des messages d’erreur utiles aux utilisateurs finaux lorsqu’ils essaient d’entrer des données de film qui ne sont pas valides.</span><span class="sxs-lookup"><span data-stu-id="99ef4-110">We'll ensure that our database content is always correct, and provide helpful error messages to end users when they try and enter Movie data which is not valid.</span></span> <span data-ttu-id="99ef4-111">Nous allons commencer par ajouter une petite logique de validation à la classe Movie.</span><span class="sxs-lookup"><span data-stu-id="99ef4-111">We'll begin by adding a little validation logic to the Movie class.</span></span>

<span data-ttu-id="99ef4-112">Cliquez avec le bouton droit sur le dossier du modèle et sélectionnez Ajouter une classe.</span><span class="sxs-lookup"><span data-stu-id="99ef4-112">Right click on the Model folder and select Add Class.</span></span> <span data-ttu-id="99ef4-113">Nommez votre classe Movie.</span><span class="sxs-lookup"><span data-stu-id="99ef4-113">Name your class Movie.</span></span>

<span data-ttu-id="99ef4-114">Lorsque nous avons créé le modèle d’entité Movie précédemment, l’IDE a créé une classe Movie.</span><span class="sxs-lookup"><span data-stu-id="99ef4-114">When we created the Movie Entity Model earlier, the IDE created a Movie class.</span></span> <span data-ttu-id="99ef4-115">En fait, une partie de la classe Movie peut figurer dans un fichier et faire partie d’une autre.</span><span class="sxs-lookup"><span data-stu-id="99ef4-115">In fact, part of the Movie class can be in one file and part in another.</span></span> <span data-ttu-id="99ef4-116">C’est ce que l’on appelle une classe partielle.</span><span class="sxs-lookup"><span data-stu-id="99ef4-116">This is called a Partial Class.</span></span> <span data-ttu-id="99ef4-117">Nous allons étendre la classe Movie à partir d’un autre fichier.</span><span class="sxs-lookup"><span data-stu-id="99ef4-117">We're going to extend the Movie class from another file.</span></span>

<span data-ttu-id="99ef4-118">Nous allons créer une classe de film partielle qui pointe vers une « classe associée » avec des attributs qui fourniront des indications de validation au système.</span><span class="sxs-lookup"><span data-stu-id="99ef4-118">We'll create a partial movie class that points to a "buddy class" with some attributes that will give validation hints to the system.</span></span> <span data-ttu-id="99ef4-119">Nous allons marquer le titre et le prix comme requis et insister sur le prix dans une certaine plage.</span><span class="sxs-lookup"><span data-stu-id="99ef4-119">We'll mark the Title and Price as Required, and also insist that the Price be within a certain range.</span></span> <span data-ttu-id="99ef4-120">Cliquez avec le bouton droit sur le dossier modèles, puis sélectionnez Ajouter une classe.</span><span class="sxs-lookup"><span data-stu-id="99ef4-120">Right click the Models folder and select Add Class.</span></span> <span data-ttu-id="99ef4-121">Nommez votre film de classe, puis cliquez sur le bouton OK.</span><span class="sxs-lookup"><span data-stu-id="99ef4-121">Name your class Movie and Click the OK button.</span></span> <span data-ttu-id="99ef4-122">Voici à quoi ressemble la classe de films partiels.</span><span class="sxs-lookup"><span data-stu-id="99ef4-122">Here's what our partial Movie class looks like.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

<span data-ttu-id="99ef4-123">Réexécutez votre application et essayez d’entrer un film dont le prix est supérieur à 100.</span><span class="sxs-lookup"><span data-stu-id="99ef4-123">Re-Run your application and try to enter a movie with a price over 100.</span></span> <span data-ttu-id="99ef4-124">Une erreur s’affiche une fois que vous avez envoyé le formulaire.</span><span class="sxs-lookup"><span data-stu-id="99ef4-124">You'll get an error after you've submitted the form.</span></span> <span data-ttu-id="99ef4-125">L’erreur est interceptée côté serveur et se produit après la publication du formulaire.</span><span class="sxs-lookup"><span data-stu-id="99ef4-125">The error is caught on the server side and occurs after the Form is POSTed.</span></span> <span data-ttu-id="99ef4-126">Notez que les applications d’assistance HTML intégrées à ASP.NET MVC étaient suffisamment intelligentes pour afficher le message d’erreur et maintenir les valeurs pour nous dans les éléments de la zone de texte :</span><span class="sxs-lookup"><span data-stu-id="99ef4-126">Notice how ASP.NET MVC's built-in HTML helpers were smart enough to display the error message and maintain the values for us within the textbox elements:</span></span>

<span data-ttu-id="99ef4-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="99ef4-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span></span>

<span data-ttu-id="99ef4-128">Cela fonctionne bien, mais il s’agit d’une bonne chose si nous pouvions dire à l’utilisateur côté client, immédiatement avant que le serveur ne soit impliqué.</span><span class="sxs-lookup"><span data-stu-id="99ef4-128">This works great, but it'd be nice if we could tell the user on the client-side, immediately, before the server gets involved.</span></span>

<span data-ttu-id="99ef4-129">Nous allons activer la validation côté client avec JavaScript.</span><span class="sxs-lookup"><span data-stu-id="99ef4-129">Let's enable some client-side validation with JavaScript.</span></span>

## <a name="adding-client-side-validation"></a><span data-ttu-id="99ef4-130">Ajout de la validation côté client</span><span class="sxs-lookup"><span data-stu-id="99ef4-130">Adding Client-Side Validation</span></span>

<span data-ttu-id="99ef4-131">Étant donné que notre classe Movie a déjà des attributs de validation, nous devons simplement ajouter quelques fichiers JavaScript à notre modèle de vue Create. aspx et ajouter une ligne de code pour permettre la validation côté client.</span><span class="sxs-lookup"><span data-stu-id="99ef4-131">Since our Movie class already has some validation attributes, we'll just need to add a few JavaScript files to our Create.aspx View template and add a line of code to enable client-side validation to take place.</span></span>

<span data-ttu-id="99ef4-132">Dans VWD existant, accédez à notre dossier views/Movie et ouvrez Create. aspx.</span><span class="sxs-lookup"><span data-stu-id="99ef4-132">From within VWD go our Views/Movie folder and open up Create.aspx.</span></span>

<span data-ttu-id="99ef4-133">Ouvrez le dossier scripts dans le Explorateur de solutions, puis faites glisser les trois scripts suivants dans la balise de&gt; &lt;Head.</span><span class="sxs-lookup"><span data-stu-id="99ef4-133">Open up the Scripts folder in the Solution Explorer and drag the following three scripts to within the &lt;head&gt; tag.</span></span>

- <span data-ttu-id="99ef4-134">MicrosoftAjax.js</span><span class="sxs-lookup"><span data-stu-id="99ef4-134">MicrosoftAjax.js</span></span>
- <span data-ttu-id="99ef4-135">MicrosoftMvcValidation.js</span><span class="sxs-lookup"><span data-stu-id="99ef4-135">MicrosoftMvcValidation.js</span></span>

<span data-ttu-id="99ef4-136">Vous souhaitez que ces fichiers de script s’affichent dans cet ordre.</span><span class="sxs-lookup"><span data-stu-id="99ef4-136">You want these script files to appear in this order.</span></span>

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

<span data-ttu-id="99ef4-137">Ajoutez également cette seule ligne au-dessus du fichier html. BeginForm :</span><span class="sxs-lookup"><span data-stu-id="99ef4-137">Also, add this single line above the Html.BeginForm:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

<span data-ttu-id="99ef4-138">Voici le code affiché dans l’IDE.</span><span class="sxs-lookup"><span data-stu-id="99ef4-138">Here's the code shown within the IDE.</span></span>

<span data-ttu-id="99ef4-139">[Films ![-Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="99ef4-139">[![Movies - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span></span>

<span data-ttu-id="99ef4-140">Exécutez votre application et visitez à nouveau/Movies/Create, puis cliquez sur créer sans entrer de données.</span><span class="sxs-lookup"><span data-stu-id="99ef4-140">Run your application and visit /Movies/Create again, and click Create without entering any data.</span></span> <span data-ttu-id="99ef4-141">Les messages d’erreur s’affichent immédiatement sans le flash de page que nous associons à l’envoi de données vers le serveur.</span><span class="sxs-lookup"><span data-stu-id="99ef4-141">The error messages appear immediately without the page flash that we associate with sending data all the way back to the server.</span></span> <span data-ttu-id="99ef4-142">Cela est dû au fait que ASP.NET MVC valide à présent l’entrée à la fois sur le client (à l’aide de JavaScript) et sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="99ef4-142">This is because ASP.NET MVC is now validating the input on both the client (using JavaScript) and on the server.</span></span>

<span data-ttu-id="99ef4-143">[![créer-Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="99ef4-143">[![Create - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span></span>

<span data-ttu-id="99ef4-144">C’est parfait !</span><span class="sxs-lookup"><span data-stu-id="99ef4-144">This is looking good!</span></span> <span data-ttu-id="99ef4-145">Nous allons maintenant ajouter une colonne supplémentaire à la base de données.</span><span class="sxs-lookup"><span data-stu-id="99ef4-145">Let's now add one additional column to the database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="99ef4-146">[Précédent](getting-started-with-mvc-part6.md)
> [Suivant](getting-started-with-mvc-part8.md)</span><span class="sxs-lookup"><span data-stu-id="99ef4-146">[Previous](getting-started-with-mvc-part6.md)
[Next](getting-started-with-mvc-part8.md)</span></span>
