---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
title: Création d’une contrainte de routageC#personnalisée () | Microsoft Docs
author: StephenWalther
description: Stephen Walther montre comment vous pouvez créer une contrainte de routage personnalisée. Nous implémentons une contrainte personnalisée simple qui empêche un itinéraire d’être mis en correspondance avec w...
ms.author: riande
ms.date: 02/16/2009
ms.assetid: a4f4bf4e-abcc-4650-8f43-527e48b52fe6
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 98d5839e3d2623665770ccc5689c28f9eb29c8f6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601450"
---
# <a name="creating-a-custom-route-constraint-c"></a><span data-ttu-id="a4949-104">Création d’une contrainte de route personnalisée (C#)</span><span class="sxs-lookup"><span data-stu-id="a4949-104">Creating a Custom Route Constraint (C#)</span></span>

<span data-ttu-id="a4949-105">par [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="a4949-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="a4949-106">Stephen Walther montre comment vous pouvez créer une contrainte de routage personnalisée.</span><span class="sxs-lookup"><span data-stu-id="a4949-106">Stephen Walther demonstrates how you can create a custom route constraint.</span></span> <span data-ttu-id="a4949-107">Nous implémentons une contrainte personnalisée simple qui empêche un itinéraire d’être mis en correspondance lorsqu’une demande de navigateur est effectuée à partir d’un ordinateur distant.</span><span class="sxs-lookup"><span data-stu-id="a4949-107">We implement a simple custom constraint that prevents a route from being matched when a browser request is made from a remote computer.</span></span>

<span data-ttu-id="a4949-108">L’objectif de ce didacticiel est de montrer comment vous pouvez créer une contrainte de routage personnalisée.</span><span class="sxs-lookup"><span data-stu-id="a4949-108">The goal of this tutorial is to demonstrate how you can create a custom route constraint.</span></span> <span data-ttu-id="a4949-109">Une contrainte d’itinéraire personnalisée vous permet d’empêcher la mise en correspondance d’un itinéraire, sauf si une condition personnalisée est mise en correspondance.</span><span class="sxs-lookup"><span data-stu-id="a4949-109">A custom route constraint enables you to prevent a route from being matched unless some custom condition is matched.</span></span>

<span data-ttu-id="a4949-110">Dans ce didacticiel, nous créons une contrainte d’itinéraire localhost.</span><span class="sxs-lookup"><span data-stu-id="a4949-110">In this tutorial, we create a Localhost route constraint.</span></span> <span data-ttu-id="a4949-111">La contrainte d’itinéraire localhost correspond uniquement aux demandes effectuées à partir de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="a4949-111">The Localhost route constraint only matches requests made from the local computer.</span></span> <span data-ttu-id="a4949-112">Les demandes distantes à partir d’Internet ne sont pas mises en correspondance.</span><span class="sxs-lookup"><span data-stu-id="a4949-112">Remote requests from across the Internet are not matched.</span></span>

<span data-ttu-id="a4949-113">Implémentez une contrainte d’itinéraire personnalisée en implémentant l’interface IRouteConstraint.</span><span class="sxs-lookup"><span data-stu-id="a4949-113">You implement a custom route constraint by implementing the IRouteConstraint interface.</span></span> <span data-ttu-id="a4949-114">Il s’agit d’une interface extrêmement simple qui décrit une méthode unique :</span><span class="sxs-lookup"><span data-stu-id="a4949-114">This is an extremely simple interface which describes a single method:</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample1.cs)]

<span data-ttu-id="a4949-115">La méthode retourne une valeur booléenne.</span><span class="sxs-lookup"><span data-stu-id="a4949-115">The method returns a Boolean value.</span></span> <span data-ttu-id="a4949-116">Si vous renvoyez la valeur false, l’itinéraire associé à la contrainte ne correspondra pas à la demande du navigateur.</span><span class="sxs-lookup"><span data-stu-id="a4949-116">If you return false, the route associated with the constraint won't match the browser request.</span></span>

<span data-ttu-id="a4949-117">La contrainte localhost est contenue dans la liste 1.</span><span class="sxs-lookup"><span data-stu-id="a4949-117">The Localhost constraint is contained in Listing 1.</span></span>

<span data-ttu-id="a4949-118">**Liste 1-LocalhostConstraint.cs**</span><span class="sxs-lookup"><span data-stu-id="a4949-118">**Listing 1 - LocalhostConstraint.cs**</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample2.cs)]

<span data-ttu-id="a4949-119">La contrainte de la liste 1 tire parti de la propriété IsLocal exposée par la classe HttpRequest.</span><span class="sxs-lookup"><span data-stu-id="a4949-119">The constraint in Listing 1 takes advantage of the IsLocal property exposed by the HttpRequest class.</span></span> <span data-ttu-id="a4949-120">Cette propriété renvoie la valeur true lorsque l’adresse IP de la demande est 127.0.0.1 ou lorsque l’adresse IP de la demande est identique à l’adresse IP du serveur.</span><span class="sxs-lookup"><span data-stu-id="a4949-120">This property returns true when the IP address of the request is either 127.0.0.1 or when the IP of the request is the same as the server's IP address.</span></span>

<span data-ttu-id="a4949-121">Vous utilisez une contrainte personnalisée dans un itinéraire défini dans le fichier global. asax.</span><span class="sxs-lookup"><span data-stu-id="a4949-121">You use a custom constraint within a route defined in the Global.asax file.</span></span> <span data-ttu-id="a4949-122">Le fichier global. asax de la liste 2 utilise la contrainte localhost pour empêcher quiconque de demander une page d’administration à moins qu’il n’effectue la demande à partir du serveur local.</span><span class="sxs-lookup"><span data-stu-id="a4949-122">The Global.asax file in Listing 2 uses the Localhost constraint to prevent anyone from requesting an Admin page unless they make the request from the local server.</span></span> <span data-ttu-id="a4949-123">Par exemple, une demande pour/Admin/DeleteAll échoue lorsqu’elle est effectuée à partir d’un serveur distant.</span><span class="sxs-lookup"><span data-stu-id="a4949-123">For example, a request for /Admin/DeleteAll will fail when made from a remote server.</span></span>

<span data-ttu-id="a4949-124">**Liste 2-global. asax**</span><span class="sxs-lookup"><span data-stu-id="a4949-124">**Listing 2 - Global.asax**</span></span>

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample3.cs)]

<span data-ttu-id="a4949-125">La contrainte localhost est utilisée dans la définition de l’itinéraire d’administration.</span><span class="sxs-lookup"><span data-stu-id="a4949-125">The Localhost constraint is used in the definition of the Admin route.</span></span> <span data-ttu-id="a4949-126">Cet itinéraire n’est pas mis en correspondance par une demande de navigateur à distance.</span><span class="sxs-lookup"><span data-stu-id="a4949-126">This route won't be matched by a remote browser request.</span></span> <span data-ttu-id="a4949-127">Sachez toutefois que les autres itinéraires définis dans global. asax peuvent correspondre à la même requête.</span><span class="sxs-lookup"><span data-stu-id="a4949-127">Realize, however, that other routes defined in Global.asax might match the same request.</span></span> <span data-ttu-id="a4949-128">Il est important de comprendre qu’une contrainte empêche un itinéraire particulier de correspondre à une requête et non à tous les itinéraires définis dans le fichier global. asax.</span><span class="sxs-lookup"><span data-stu-id="a4949-128">It is important to understand that a constraint prevents a particular route from matching a request and not all routes defined in the Global.asax file.</span></span>

<span data-ttu-id="a4949-129">Notez que l’itinéraire par défaut a été mis en commentaire à partir du fichier global. asax dans la liste 2.</span><span class="sxs-lookup"><span data-stu-id="a4949-129">Notice that the Default route has been commented out from the Global.asax file in Listing 2.</span></span> <span data-ttu-id="a4949-130">Si vous incluez l’itinéraire par défaut, l’itinéraire par défaut correspondrait aux demandes du contrôleur d’administration.</span><span class="sxs-lookup"><span data-stu-id="a4949-130">If you include the Default route, then the Default route would match requests for the Admin controller.</span></span> <span data-ttu-id="a4949-131">Dans ce cas, les utilisateurs distants peuvent appeler des actions du contrôleur d’administration même si leurs demandes ne correspondent pas à l’itinéraire de l’administrateur.</span><span class="sxs-lookup"><span data-stu-id="a4949-131">In that case, remote users could still invoke actions of the Admin controller even though their requests wouldn't match the Admin route.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a4949-132">[Précédent](creating-a-route-constraint-cs.md)
> [Suivant](asp-net-mvc-controller-overview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a4949-132">[Previous](creating-a-route-constraint-cs.md)
[Next](asp-net-mvc-controller-overview-vb.md)</span></span>
