---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
title: Création d’une contrainte de route (VB) | Microsoft Docs
author: StephenWalther
description: Dans ce didacticiel, Stephen Walther montre comment vous pouvez contrôler la façon dont les demandes de navigateur correspondent aux itinéraires en créant des contraintes d’itinéraire avec des expressions régulières.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: b7cce113-c82c-45bf-b97b-357e5d9f7f56
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 205742dd8f866c8828008c8aac7ab3f98b173ceb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601387"
---
# <a name="creating-a-route-constraint-vb"></a><span data-ttu-id="5dbdb-103">Création d’une contrainte de route (VB)</span><span class="sxs-lookup"><span data-stu-id="5dbdb-103">Creating a Route Constraint (VB)</span></span>

<span data-ttu-id="5dbdb-104">par [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="5dbdb-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="5dbdb-105">Dans ce didacticiel, Stephen Walther montre comment vous pouvez contrôler la façon dont les demandes de navigateur correspondent aux itinéraires en créant des contraintes d’itinéraire avec des expressions régulières.</span><span class="sxs-lookup"><span data-stu-id="5dbdb-105">In this tutorial, Stephen Walther demonstrates how you can control how browser requests match routes by creating route constraints with regular expressions.</span></span>

<span data-ttu-id="5dbdb-106">Vous utilisez des contraintes de routage pour restreindre les demandes de navigateur qui correspondent à un itinéraire donné.</span><span class="sxs-lookup"><span data-stu-id="5dbdb-106">You use route constraints to restrict the browser requests that match a particular route.</span></span> <span data-ttu-id="5dbdb-107">Vous pouvez utiliser une expression régulière pour spécifier une contrainte d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="5dbdb-107">You can use a regular expression to specify a route constraint.</span></span>

<span data-ttu-id="5dbdb-108">Par exemple, imaginez que vous avez défini l’itinéraire dans la liste 1 de votre fichier global. asax.</span><span class="sxs-lookup"><span data-stu-id="5dbdb-108">For example, imagine that you have defined the route in Listing 1 in your Global.asax file.</span></span>

<span data-ttu-id="5dbdb-109">**Liste 1-global. asax. vb**</span><span class="sxs-lookup"><span data-stu-id="5dbdb-109">**Listing 1 - Global.asax.vb**</span></span>

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample1.vb)]

<span data-ttu-id="5dbdb-110">La liste 1 contient un itinéraire nommé Product.</span><span class="sxs-lookup"><span data-stu-id="5dbdb-110">Listing 1 contains a route named Product.</span></span> <span data-ttu-id="5dbdb-111">Vous pouvez utiliser l’itinéraire du produit pour mapper les demandes de navigateur aux ProductController contenues dans la liste 2.</span><span class="sxs-lookup"><span data-stu-id="5dbdb-111">You can use the Product route to map browser requests to the ProductController contained in Listing 2.</span></span>

<span data-ttu-id="5dbdb-112">**Liste 2-Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="5dbdb-112">**Listing 2 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample2.vb)]

<span data-ttu-id="5dbdb-113">Notez que l’action Details () exposée par le contrôleur de produit accepte un seul paramètre nommé productId.</span><span class="sxs-lookup"><span data-stu-id="5dbdb-113">Notice that the Details() action exposed by the Product controller accepts a single parameter named productId.</span></span> <span data-ttu-id="5dbdb-114">Ce paramètre est un paramètre entier.</span><span class="sxs-lookup"><span data-stu-id="5dbdb-114">This parameter is an integer parameter.</span></span>

<span data-ttu-id="5dbdb-115">L’itinéraire défini dans la liste 1 correspond à l’une des URL suivantes :</span><span class="sxs-lookup"><span data-stu-id="5dbdb-115">The route defined in Listing 1 will match any of the following URLs:</span></span>

- <span data-ttu-id="5dbdb-116">/Product/23</span><span class="sxs-lookup"><span data-stu-id="5dbdb-116">/Product/23</span></span>
- <span data-ttu-id="5dbdb-117">/Product/7</span><span class="sxs-lookup"><span data-stu-id="5dbdb-117">/Product/7</span></span>

<span data-ttu-id="5dbdb-118">Malheureusement, l’itinéraire correspondra également aux URL suivantes :</span><span class="sxs-lookup"><span data-stu-id="5dbdb-118">Unfortunately, the route will also match the following URLs:</span></span>

- <span data-ttu-id="5dbdb-119">/Product/blah</span><span class="sxs-lookup"><span data-stu-id="5dbdb-119">/Product/blah</span></span>
- <span data-ttu-id="5dbdb-120">/Product/apple</span><span class="sxs-lookup"><span data-stu-id="5dbdb-120">/Product/apple</span></span>

<span data-ttu-id="5dbdb-121">Étant donné que l’action Details () attend un paramètre entier, une requête qui contient autre chose qu’une valeur entière génère une erreur.</span><span class="sxs-lookup"><span data-stu-id="5dbdb-121">Because the Details() action expects an integer parameter, making a request that contains something other than an integer value will cause an error.</span></span> <span data-ttu-id="5dbdb-122">Par exemple, si vous tapez l’URL/Product/Apple dans votre navigateur, vous obtiendrez la page d’erreur dans la figure 1.</span><span class="sxs-lookup"><span data-stu-id="5dbdb-122">For example, if you type the URL /Product/apple into your browser then you will get the error page in Figure 1.</span></span>

<span data-ttu-id="5dbdb-123">[![la boîte de dialogue Nouveau projet](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5dbdb-123">[![The New Project dialog box](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)</span></span>

<span data-ttu-id="5dbdb-124">**Figure 01**: affichage d’une explosion de page ([cliquez pour afficher l’image en taille réelle](creating-a-route-constraint-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="5dbdb-124">**Figure 01**: Seeing a page explode ([Click to view full-size image](creating-a-route-constraint-vb/_static/image2.png))</span></span>

<span data-ttu-id="5dbdb-125">Ce que vous voulez vraiment faire, c’est uniquement les URL qui contiennent un entier productId correct.</span><span class="sxs-lookup"><span data-stu-id="5dbdb-125">What you really want to do is only match URLs that contain a proper integer productId.</span></span> <span data-ttu-id="5dbdb-126">Vous pouvez utiliser une contrainte lors de la définition d’un itinéraire pour limiter les URL qui correspondent à l’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="5dbdb-126">You can use a constraint when defining a route to restrict the URLs that match the route.</span></span> <span data-ttu-id="5dbdb-127">L’itinéraire du produit modifié dans la liste 3 contient une contrainte d’expression régulière qui correspond uniquement à des entiers.</span><span class="sxs-lookup"><span data-stu-id="5dbdb-127">The modified Product route in Listing 3 contains a regular expression constraint that only matches integers.</span></span>

<span data-ttu-id="5dbdb-128">**Liste 3-global. asax. vb**</span><span class="sxs-lookup"><span data-stu-id="5dbdb-128">**Listing 3 - Global.asax.vb**</span></span>

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample3.vb)]

<span data-ttu-id="5dbdb-129">L’expression régulière \d + correspond à un ou plusieurs entiers.</span><span class="sxs-lookup"><span data-stu-id="5dbdb-129">The regular expression \d+ matches one or more integers.</span></span> <span data-ttu-id="5dbdb-130">Cette contrainte fait en sorte que l’itinéraire du produit corresponde aux URL suivantes :</span><span class="sxs-lookup"><span data-stu-id="5dbdb-130">This constraint causes the Product route to match the following URLs:</span></span>

- <span data-ttu-id="5dbdb-131">/Product/3</span><span class="sxs-lookup"><span data-stu-id="5dbdb-131">/Product/3</span></span>
- <span data-ttu-id="5dbdb-132">/Product/8999</span><span class="sxs-lookup"><span data-stu-id="5dbdb-132">/Product/8999</span></span>

<span data-ttu-id="5dbdb-133">Mais pas les URL suivantes :</span><span class="sxs-lookup"><span data-stu-id="5dbdb-133">But not the following URLs:</span></span>

- <span data-ttu-id="5dbdb-134">/Product/apple</span><span class="sxs-lookup"><span data-stu-id="5dbdb-134">/Product/apple</span></span>
- <span data-ttu-id="5dbdb-135">/Product</span><span class="sxs-lookup"><span data-stu-id="5dbdb-135">/Product</span></span>

<span data-ttu-id="5dbdb-136">Ces requêtes de navigateur seront gérées par un autre itinéraire ou, s’il n’y a pas d’itinéraires correspondants, *l’erreur la ressource est introuvable* est retournée.</span><span class="sxs-lookup"><span data-stu-id="5dbdb-136">These browser requests will be handled by another route or, if there are no matching routes, a *The resource could not be found* error will be returned.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5dbdb-137">[Précédent](creating-custom-routes-vb.md)
> [Suivant](creating-a-custom-route-constraint-vb.md)</span><span class="sxs-lookup"><span data-stu-id="5dbdb-137">[Previous](creating-custom-routes-vb.md)
[Next](creating-a-custom-route-constraint-vb.md)</span></span>
