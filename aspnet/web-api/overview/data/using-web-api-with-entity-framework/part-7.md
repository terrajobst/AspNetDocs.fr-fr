---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Créer la vue (IU) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: 62c4523c2c6fb399cfbc3716309a1379996d601c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557301"
---
# <a name="create-the-view-ui"></a><span data-ttu-id="f8a55-102">Créer la vue (Interface utilisateur)</span><span class="sxs-lookup"><span data-stu-id="f8a55-102">Create the View (UI)</span></span>

<span data-ttu-id="f8a55-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f8a55-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="f8a55-104">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="f8a55-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="f8a55-105">Dans cette section, vous allez commencer à définir le code HTML de l’application et ajouter la liaison de données entre le code HTML et le modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="f8a55-105">In this section, you will start to define the HTML for the app, and add data binding between the HTML and the view model.</span></span>

<span data-ttu-id="f8a55-106">Ouvrez le fichier views/orig/index. cshtml.</span><span class="sxs-lookup"><span data-stu-id="f8a55-106">Open the file Views/Home/Index.cshtml.</span></span> <span data-ttu-id="f8a55-107">Remplacez l’intégralité du contenu de ce fichier par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="f8a55-107">Replace the entire contents of that file with the following.</span></span>

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

<span data-ttu-id="f8a55-108">La plupart des éléments de `div` sont là pour le style de [démarrage](http://getbootstrap.com/) .</span><span class="sxs-lookup"><span data-stu-id="f8a55-108">Most of the `div` elements are there for [Bootstrap](http://getbootstrap.com/) styling.</span></span> <span data-ttu-id="f8a55-109">Les éléments importants sont ceux avec `data-bind` attributs.</span><span class="sxs-lookup"><span data-stu-id="f8a55-109">The important elements are the ones with `data-bind` attributes.</span></span> <span data-ttu-id="f8a55-110">Cet attribut lie le code HTML au modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="f8a55-110">This attribute links the HTML to the view model.</span></span>

<span data-ttu-id="f8a55-111">Exemple :</span><span class="sxs-lookup"><span data-stu-id="f8a55-111">For example:</span></span>

[!code-html[Main](part-7/samples/sample2.html)]

<span data-ttu-id="f8a55-112">Dans cet exemple, le &quot;`text`&quot; la liaison fait en sorte que l’élément `<p>` affiche la valeur de la propriété `error` à partir du modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="f8a55-112">In this example, the &quot;`text`&quot; binding causes the `<p>` element to show the value of the `error` property from the view model.</span></span> <span data-ttu-id="f8a55-113">Rappelez-vous que `error` a été déclaré comme `ko.observable`:</span><span class="sxs-lookup"><span data-stu-id="f8a55-113">Recall that `error` was declared as a `ko.observable`:</span></span>

[!code-javascript[Main](part-7/samples/sample3.js)]

<span data-ttu-id="f8a55-114">Chaque fois qu’une nouvelle valeur est assignée à `error`, Knockout met à jour le texte dans l’élément `<p>`.</span><span class="sxs-lookup"><span data-stu-id="f8a55-114">Whenever a new value is assigned to `error`, Knockout updates the text in the `<p>` element.</span></span>

<span data-ttu-id="f8a55-115">La liaison de `foreach` indique à Knockout de parcourir en boucle le contenu du tableau `books`.</span><span class="sxs-lookup"><span data-stu-id="f8a55-115">The `foreach` binding tells Knockout to loop through the contents of the `books` array.</span></span> <span data-ttu-id="f8a55-116">Pour chaque élément du tableau, Knockout crée un nouvel élément &lt;Li&gt;.</span><span class="sxs-lookup"><span data-stu-id="f8a55-116">For each item in the array, Knockout creates a new &lt;li&gt; element.</span></span> <span data-ttu-id="f8a55-117">Les liaisons dans le contexte de la `foreach` font référence aux propriétés sur l’élément de tableau.</span><span class="sxs-lookup"><span data-stu-id="f8a55-117">Bindings inside the context of the `foreach` refer to properties on the array item.</span></span> <span data-ttu-id="f8a55-118">Exemple :</span><span class="sxs-lookup"><span data-stu-id="f8a55-118">For example:</span></span>

[!code-html[Main](part-7/samples/sample4.html)]

<span data-ttu-id="f8a55-119">Ici, la liaison `text` lit la propriété auteur de chaque livre.</span><span class="sxs-lookup"><span data-stu-id="f8a55-119">Here the `text` binding reads the Author property of each book.</span></span>

<span data-ttu-id="f8a55-120">Si vous exécutez l’application maintenant, elle doit ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="f8a55-120">If you run the application now, it should look like this:</span></span>

![](part-7/_static/image1.png)

<span data-ttu-id="f8a55-121">La liste de livres est chargée de façon asynchrone, après le chargement de la page.</span><span class="sxs-lookup"><span data-stu-id="f8a55-121">The list of books loads asynchronously, after the page loads.</span></span> <span data-ttu-id="f8a55-122">Pour le moment, les détails de la &quot;&quot; les liens ne sont pas fonctionnels.</span><span class="sxs-lookup"><span data-stu-id="f8a55-122">Right now, the &quot;Details&quot; links are not functional.</span></span> <span data-ttu-id="f8a55-123">Nous allons ajouter cette fonctionnalité dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="f8a55-123">We'll add this functionality in the next section.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f8a55-124">[Précédent](part-6.md)
> [Suivant](part-8.md)</span><span class="sxs-lookup"><span data-stu-id="f8a55-124">[Previous](part-6.md)
[Next](part-8.md)</span></span>
