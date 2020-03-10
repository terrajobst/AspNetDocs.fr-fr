---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
title: Création d’un contrôle Rating (VB) | Microsoft Docs
author: wenz
description: De nombreux sites Web, du commerce électronique aux sites de la Communauté, permettent à leurs utilisateurs de noter des articles ou des articles. Cela nécessite généralement un effort de codage, mais nous avons le...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 6d0d70f4-725e-4258-8ae8-24a6ba1ddbf7
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 08e245edfe73db4e3896db51151e5d7a0fa9697c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78612195"
---
# <a name="creating-a-rating-control-vb"></a><span data-ttu-id="aa17f-104">Création d’un contrôle Rating (VB)</span><span class="sxs-lookup"><span data-stu-id="aa17f-104">Creating a Rating Control (VB)</span></span>

<span data-ttu-id="aa17f-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="aa17f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="aa17f-106">[Télécharger le code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.vb.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="aa17f-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0VB.pdf)</span></span>

> <span data-ttu-id="aa17f-107">De nombreux sites Web, du commerce électronique aux sites de la Communauté, permettent à leurs utilisateurs de noter des articles ou des articles.</span><span class="sxs-lookup"><span data-stu-id="aa17f-107">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="aa17f-108">Cela nécessite généralement un effort de codage, mais nous avons la boîte à outils de contrôle à notre disposition.</span><span class="sxs-lookup"><span data-stu-id="aa17f-108">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>

## <a name="overview"></a><span data-ttu-id="aa17f-109">Présentation</span><span class="sxs-lookup"><span data-stu-id="aa17f-109">Overview</span></span>

<span data-ttu-id="aa17f-110">De nombreux sites Web, du commerce électronique aux sites de la Communauté, permettent à leurs utilisateurs de noter des articles ou des articles.</span><span class="sxs-lookup"><span data-stu-id="aa17f-110">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="aa17f-111">Cela nécessite généralement un effort de codage, mais nous avons la boîte à outils de contrôle à notre disposition.</span><span class="sxs-lookup"><span data-stu-id="aa17f-111">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>

## <a name="steps"></a><span data-ttu-id="aa17f-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="aa17f-112">Steps</span></span>

<span data-ttu-id="aa17f-113">Tout d’abord, vous avez besoin (au moins) de deux types d’images : un pour un élément de notation rempli et un pour un élément d’évaluation vide.</span><span class="sxs-lookup"><span data-stu-id="aa17f-113">First of all, you need (at least) two kinds of images: one for a filled out rating item, and one for an empty rating item.</span></span> <span data-ttu-id="aa17f-114">Un élément d’évaluation est généralement une étoile ou un Smiley.</span><span class="sxs-lookup"><span data-stu-id="aa17f-114">A rating item is usually a star or a smiley.</span></span> <span data-ttu-id="aa17f-115">Pour ce scénario, vous trouverez trois fichiers, Smiley. png et Empty. png et Smiley-Done. png dans le cadre des téléchargements du code source pour ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="aa17f-115">For this scenario, you find three files, smiley.png and empty.png and smiley-done.png as part of the source code downloads for this tutorial.</span></span>

<span data-ttu-id="aa17f-116">Créez ensuite un nouveau fichier ASP.NET et commencez par y ajouter un contrôle de `ScriptManager` :</span><span class="sxs-lookup"><span data-stu-id="aa17f-116">Then, create a new ASP.NET file and start with adding a `ScriptManager` control to it:</span></span>

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample1.aspx)]

<span data-ttu-id="aa17f-117">Ensuite, ajoutez le contrôle `Rating` à partir de la boîte à outils de contrôle AJAX ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="aa17f-117">Then, add the `Rating` control from the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="aa17f-118">Les attributs suivants doivent être définis pour cet exemple :</span><span class="sxs-lookup"><span data-stu-id="aa17f-118">The following attributes need to be set for this example:</span></span>

- <span data-ttu-id="aa17f-119">`CurrentRating` l’évaluation initiale à utiliser</span><span class="sxs-lookup"><span data-stu-id="aa17f-119">`CurrentRating` the initial rating to be used</span></span>
- <span data-ttu-id="aa17f-120">`MaxRating` le classement maximal</span><span class="sxs-lookup"><span data-stu-id="aa17f-120">`MaxRating` the maximum rating</span></span>
- <span data-ttu-id="aa17f-121">`EmptyStarCssClass` la classe CSS à utiliser lorsqu’un élément d’évaluation (en étoile) est vide</span><span class="sxs-lookup"><span data-stu-id="aa17f-121">`EmptyStarCssClass` the CSS class to use when a rating item ( star ) is empty</span></span>
- <span data-ttu-id="aa17f-122">`FilledStarCssClass` la classe CSS à utiliser lorsqu’un élément d’évaluation (en étoile) est rempli</span><span class="sxs-lookup"><span data-stu-id="aa17f-122">`FilledStarCssClass` the CSS class to use when a rating item ( star ) is filled out</span></span>
- <span data-ttu-id="aa17f-123">`StarCssClass` la classe CSS à utiliser pour un stat visible</span><span class="sxs-lookup"><span data-stu-id="aa17f-123">`StarCssClass` the CSS class to use for a visible stat</span></span>
- <span data-ttu-id="aa17f-124">`WaitingStarCssClass` la classe CSS à utiliser pendant qu’une évaluation par étoile est renvoyée au serveur</span><span class="sxs-lookup"><span data-stu-id="aa17f-124">`WaitingStarCssClass` the CSS class to use while a star rating is sent back to the server</span></span>

<span data-ttu-id="aa17f-125">Et voici le balisage qui crée un contrôle d’évaluation avec cinq éléments (Smileys) dont aucun n’est rempli initialement :</span><span class="sxs-lookup"><span data-stu-id="aa17f-125">And here is the markup which creates a rating control with five items (smileys) of which none is filled out initially:</span></span>

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample2.aspx)]

<span data-ttu-id="aa17f-126">Les trois classes CSS référencées doivent maintenant afficher les fichiers image appropriés, ce qui est facile à utiliser avec CSS :</span><span class="sxs-lookup"><span data-stu-id="aa17f-126">The three referenced CSS classes now need to show the appropriate image files, which is easy to do using CSS:</span></span>

[!code-css[Main](creating-a-rating-control-vb/samples/sample3.css)]

<span data-ttu-id="aa17f-127">Assurez-vous que vous fournissez la largeur et la hauteur des trois images, sinon l’affichage peut paraître un peu.</span><span class="sxs-lookup"><span data-stu-id="aa17f-127">Make sure that you provide the width and height of the three images, otherwise the display may look a bit messed up.</span></span>

<span data-ttu-id="aa17f-128">Enfin, le résultat de l’évaluation doit être affiché à l’utilisateur (ou, au moins enregistré dans une base de données).</span><span class="sxs-lookup"><span data-stu-id="aa17f-128">Finally, the result of the rating should be displayed to the user (or, at least saved in a database).</span></span> <span data-ttu-id="aa17f-129">Ajoutez donc une étiquette pour la sortie d’un message texte et un bouton Envoyer pour valider le formulaire d’évaluation sur le serveur :</span><span class="sxs-lookup"><span data-stu-id="aa17f-129">So add a label for the output of a text message and a submit button to post back the rating form to the server:</span></span>

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample4.aspx)]

<span data-ttu-id="aa17f-130">Dans le code côté serveur, accédez au contrôle d’évaluation via son `ID` puis accédez à sa propriété `CurrentRating` qui est le nombre d’éléments d’évaluation sélectionnés, dans notre exemple, une valeur comprise entre 0 et 5.</span><span class="sxs-lookup"><span data-stu-id="aa17f-130">In the server-side code, access the Rating control via its `ID` and then access its `CurrentRating` property which is the number of the selected rating items, in our example a value between 0 and 5.</span></span>

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample5.aspx)]

<span data-ttu-id="aa17f-131">Enregistrez la page et chargez-la dans votre navigateur.</span><span class="sxs-lookup"><span data-stu-id="aa17f-131">Save the page and load it into your browser.</span></span> <span data-ttu-id="aa17f-132">Quand vous pointez sur les éléments d’évaluation (initialement vides), un effet JavaScript se produit : l’évaluation change.</span><span class="sxs-lookup"><span data-stu-id="aa17f-132">When you hover over the (initially empty) rating items, a JavaScript effect occurs: The rating changes.</span></span> <span data-ttu-id="aa17f-133">Lorsque vous cliquez sur l’ensemble d’étoiles, l’évaluation actuelle est conservée.</span><span class="sxs-lookup"><span data-stu-id="aa17f-133">When you click on the set of stars, the current rating is retained.</span></span> <span data-ttu-id="aa17f-134">Enfin, lorsque vous envoyez le formulaire, le code côté serveur génère l’évaluation sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="aa17f-134">Finally, when you submit the form, the server-side code outputs the selected rating.</span></span>

<span data-ttu-id="aa17f-135">[![de la création d’un système d’évaluation avec un minimum de code](creating-a-rating-control-vb/_static/image2.png)](creating-a-rating-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="aa17f-135">[![Creating a rating system with minimal code](creating-a-rating-control-vb/_static/image2.png)](creating-a-rating-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="aa17f-136">Création d’un système d’évaluation avec un minimum de code ([cliquez pour afficher l’image en taille réelle](creating-a-rating-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="aa17f-136">Creating a rating system with minimal code ([Click to view full-size image](creating-a-rating-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="aa17f-137">Précédent</span><span class="sxs-lookup"><span data-stu-id="aa17f-137">Previous</span></span>](creating-a-rating-control-cs.md)
