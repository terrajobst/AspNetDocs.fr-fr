---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
title: Création d’un contrôle RatingC#() | Microsoft Docs
author: wenz
description: De nombreux sites Web, du commerce électronique aux sites de la Communauté, permettent à leurs utilisateurs de noter des articles ou des articles. Cela nécessite généralement un effort de codage, mais nous avons le...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 969fb28f-2bff-4fc4-b24a-27f5e2534a37
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
msc.type: authoredcontent
ms.openlocfilehash: c0bf793406e378321f54f0460031c526a0b41a02
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74611579"
---
# <a name="creating-a-rating-control-c"></a><span data-ttu-id="c625e-104">Création d’un contrôle Rating (C#)</span><span class="sxs-lookup"><span data-stu-id="c625e-104">Creating a Rating Control (C#)</span></span>

<span data-ttu-id="c625e-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c625e-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c625e-106">[Télécharger le code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="c625e-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)</span></span>

> <span data-ttu-id="c625e-107">De nombreux sites Web, du commerce électronique aux sites de la Communauté, permettent à leurs utilisateurs de noter des articles ou des articles.</span><span class="sxs-lookup"><span data-stu-id="c625e-107">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="c625e-108">Cela nécessite généralement un effort de codage, mais nous avons la boîte à outils de contrôle à notre disposition.</span><span class="sxs-lookup"><span data-stu-id="c625e-108">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>

## <a name="overview"></a><span data-ttu-id="c625e-109">Vue d'ensemble de</span><span class="sxs-lookup"><span data-stu-id="c625e-109">Overview</span></span>

<span data-ttu-id="c625e-110">De nombreux sites Web, du commerce électronique aux sites de la Communauté, permettent à leurs utilisateurs de noter des articles ou des articles.</span><span class="sxs-lookup"><span data-stu-id="c625e-110">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="c625e-111">Cela nécessite généralement un effort de codage, mais nous avons la boîte à outils de contrôle à notre disposition.</span><span class="sxs-lookup"><span data-stu-id="c625e-111">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>

## <a name="steps"></a><span data-ttu-id="c625e-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="c625e-112">Steps</span></span>

<span data-ttu-id="c625e-113">Tout d’abord, vous avez besoin (au moins) de deux types d’images : un pour un élément de notation rempli et un pour un élément d’évaluation vide.</span><span class="sxs-lookup"><span data-stu-id="c625e-113">First of all, you need (at least) two kinds of images: one for a filled out rating item, and one for an empty rating item.</span></span> <span data-ttu-id="c625e-114">Un élément d’évaluation est généralement une étoile ou un Smiley.</span><span class="sxs-lookup"><span data-stu-id="c625e-114">A rating item is usually a star or a smiley.</span></span> <span data-ttu-id="c625e-115">Pour ce scénario, vous trouverez trois fichiers, Smiley. png et Empty. png et Smiley-Done. png dans le cadre des téléchargements du code source pour ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="c625e-115">For this scenario, you find three files, smiley.png and empty.png and smiley-done.png as part of the source code downloads for this tutorial.</span></span>

<span data-ttu-id="c625e-116">Créez ensuite un nouveau fichier ASP.NET et commencez par y ajouter un contrôle de `ScriptManager` :</span><span class="sxs-lookup"><span data-stu-id="c625e-116">Then, create a new ASP.NET file and start with adding a `ScriptManager` control to it:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample1.aspx)]

<span data-ttu-id="c625e-117">Ensuite, ajoutez le contrôle `Rating` à partir de la boîte à outils de contrôle AJAX ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c625e-117">Then, add the `Rating` control from the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="c625e-118">Les attributs suivants doivent être définis pour cet exemple :</span><span class="sxs-lookup"><span data-stu-id="c625e-118">The following attributes need to be set for this example:</span></span>

- <span data-ttu-id="c625e-119">`CurrentRating` l’évaluation initiale à utiliser</span><span class="sxs-lookup"><span data-stu-id="c625e-119">`CurrentRating` the initial rating to be used</span></span>
- <span data-ttu-id="c625e-120">`MaxRating` le classement maximal</span><span class="sxs-lookup"><span data-stu-id="c625e-120">`MaxRating` the maximum rating</span></span>
- <span data-ttu-id="c625e-121">`EmptyStarCssClass` la classe CSS à utiliser lorsqu’un élément d’évaluation (en étoile) est vide</span><span class="sxs-lookup"><span data-stu-id="c625e-121">`EmptyStarCssClass` the CSS class to use when a rating item ( star ) is empty</span></span>
- <span data-ttu-id="c625e-122">`FilledStarCssClass` la classe CSS à utiliser lorsqu’un élément d’évaluation (en étoile) est rempli</span><span class="sxs-lookup"><span data-stu-id="c625e-122">`FilledStarCssClass` the CSS class to use when a rating item ( star ) is filled out</span></span>
- <span data-ttu-id="c625e-123">`StarCssClass` la classe CSS à utiliser pour un stat visible</span><span class="sxs-lookup"><span data-stu-id="c625e-123">`StarCssClass` the CSS class to use for a visible stat</span></span>
- <span data-ttu-id="c625e-124">`WaitingStarCssClass` la classe CSS à utiliser pendant qu’une évaluation par étoile est renvoyée au serveur</span><span class="sxs-lookup"><span data-stu-id="c625e-124">`WaitingStarCssClass` the CSS class to use while a star rating is sent back to the server</span></span>

<span data-ttu-id="c625e-125">Et voici le balisage qui crée un contrôle d’évaluation avec cinq éléments (Smileys) dont aucun n’est rempli initialement :</span><span class="sxs-lookup"><span data-stu-id="c625e-125">And here is the markup which creates a rating control with five items (smileys) of which none is filled out initially:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample2.aspx)]

<span data-ttu-id="c625e-126">Les trois classes CSS référencées doivent maintenant afficher les fichiers image appropriés, ce qui est facile à utiliser avec CSS :</span><span class="sxs-lookup"><span data-stu-id="c625e-126">The three referenced CSS classes now need to show the appropriate image files, which is easy to do using CSS:</span></span>

[!code-css[Main](creating-a-rating-control-cs/samples/sample3.css)]

<span data-ttu-id="c625e-127">Assurez-vous que vous fournissez la largeur et la hauteur des trois images, sinon l’affichage peut paraître un peu.</span><span class="sxs-lookup"><span data-stu-id="c625e-127">Make sure that you provide the width and height of the three images, otherwise the display may look a bit messed up.</span></span>

<span data-ttu-id="c625e-128">Enfin, le résultat de l’évaluation doit être affiché à l’utilisateur (ou, au moins enregistré dans une base de données).</span><span class="sxs-lookup"><span data-stu-id="c625e-128">Finally, the result of the rating should be displayed to the user (or, at least saved in a database).</span></span> <span data-ttu-id="c625e-129">Ajoutez donc une étiquette pour la sortie d’un message texte et un bouton Envoyer pour valider le formulaire d’évaluation sur le serveur :</span><span class="sxs-lookup"><span data-stu-id="c625e-129">So add a label for the output of a text message and a submit button to post back the rating form to the server:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample4.aspx)]

<span data-ttu-id="c625e-130">Dans le code côté serveur, accédez au contrôle d’évaluation via son `ID` puis accédez à sa propriété `CurrentRating` qui est le nombre d’éléments d’évaluation sélectionnés, dans notre exemple, une valeur comprise entre 0 et 5.</span><span class="sxs-lookup"><span data-stu-id="c625e-130">In the server-side code, access the Rating control via its `ID` and then access its `CurrentRating` property which is the number of the selected rating items, in our example a value between 0 and 5.</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample5.aspx)]

<span data-ttu-id="c625e-131">Enregistrez la page et chargez-la dans votre navigateur.</span><span class="sxs-lookup"><span data-stu-id="c625e-131">Save the page and load it into your browser.</span></span> <span data-ttu-id="c625e-132">Quand vous pointez sur les éléments d’évaluation (initialement vides), un effet JavaScript se produit : l’évaluation change.</span><span class="sxs-lookup"><span data-stu-id="c625e-132">When you hover over the (initially empty) rating items, a JavaScript effect occurs: The rating changes.</span></span> <span data-ttu-id="c625e-133">Lorsque vous cliquez sur l’ensemble d’étoiles, l’évaluation actuelle est conservée.</span><span class="sxs-lookup"><span data-stu-id="c625e-133">When you click on the set of stars, the current rating is retained.</span></span> <span data-ttu-id="c625e-134">Enfin, lorsque vous envoyez le formulaire, le code côté serveur génère l’évaluation sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="c625e-134">Finally, when you submit the form, the server-side code outputs the selected rating.</span></span>

<span data-ttu-id="c625e-135">[![de la création d’un système d’évaluation avec un minimum de code](creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c625e-135">[![Creating a rating system with minimal code](creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="c625e-136">Création d’un système d’évaluation avec un minimum de code ([cliquez pour afficher l’image en taille réelle](creating-a-rating-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c625e-136">Creating a rating system with minimal code ([Click to view full-size image](creating-a-rating-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c625e-137">Suivant</span><span class="sxs-lookup"><span data-stu-id="c625e-137">Next</span></span>](creating-a-rating-control-vb.md)
