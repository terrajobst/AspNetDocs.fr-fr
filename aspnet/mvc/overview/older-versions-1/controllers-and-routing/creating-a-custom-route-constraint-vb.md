---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
title: Création d’une contrainte de route personnalisée (VB) | Microsoft Docs
author: StephenWalther
description: Stephen Walther montre comment vous pouvez créer une contrainte de routage personnalisée. Nous implémentons une contrainte personnalisée simple qui empêche un itinéraire d’être mis en correspondance avec w...
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 892edb27-1cc2-4eaf-8314-dbc2efc6228a
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 2330708cf4a28180ce8a05f4696bf7a7a32092d6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601408"
---
# <a name="creating-a-custom-route-constraint-vb"></a><span data-ttu-id="7d1e1-104">Création d’une contrainte de route personnalisée (VB)</span><span class="sxs-lookup"><span data-stu-id="7d1e1-104">Creating a Custom Route Constraint (VB)</span></span>

<span data-ttu-id="7d1e1-105">par [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="7d1e1-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="7d1e1-106">Stephen Walther montre comment vous pouvez créer une contrainte de routage personnalisée.</span><span class="sxs-lookup"><span data-stu-id="7d1e1-106">Stephen Walther demonstrates how you can create a custom route constraint.</span></span> <span data-ttu-id="7d1e1-107">Nous implémentons une contrainte personnalisée simple qui empêche un itinéraire d’être mis en correspondance lorsqu’une demande de navigateur est effectuée à partir d’un ordinateur distant.</span><span class="sxs-lookup"><span data-stu-id="7d1e1-107">We implement a simple custom constraint that prevents a route from being matched when a browser request is made from a remote computer.</span></span>

<span data-ttu-id="7d1e1-108">L’objectif de ce didacticiel est de montrer comment vous pouvez créer une contrainte de routage personnalisée.</span><span class="sxs-lookup"><span data-stu-id="7d1e1-108">The goal of this tutorial is to demonstrate how you can create a custom route constraint.</span></span> <span data-ttu-id="7d1e1-109">Une contrainte d’itinéraire personnalisée vous permet d’empêcher la mise en correspondance d’un itinéraire, sauf si une condition personnalisée est mise en correspondance.</span><span class="sxs-lookup"><span data-stu-id="7d1e1-109">A custom route constraint enables you to prevent a route from being matched unless some custom condition is matched.</span></span>

<span data-ttu-id="7d1e1-110">Dans ce didacticiel, nous créons une contrainte d’itinéraire localhost.</span><span class="sxs-lookup"><span data-stu-id="7d1e1-110">In this tutorial, we create a Localhost route constraint.</span></span> <span data-ttu-id="7d1e1-111">La contrainte d’itinéraire localhost correspond uniquement aux demandes effectuées à partir de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="7d1e1-111">The Localhost route constraint only matches requests made from the local computer.</span></span> <span data-ttu-id="7d1e1-112">Les demandes distantes à partir d’Internet ne sont pas mises en correspondance.</span><span class="sxs-lookup"><span data-stu-id="7d1e1-112">Remote requests from across the Internet are not matched.</span></span>

<span data-ttu-id="7d1e1-113">Implémentez une contrainte d’itinéraire personnalisée en implémentant l’interface IRouteConstraint.</span><span class="sxs-lookup"><span data-stu-id="7d1e1-113">You implement a custom route constraint by implementing the IRouteConstraint interface.</span></span> <span data-ttu-id="7d1e1-114">Il s’agit d’une interface extrêmement simple qui décrit une méthode unique :</span><span class="sxs-lookup"><span data-stu-id="7d1e1-114">This is an extremely simple interface which describes a single method:</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample1.vb)]

<span data-ttu-id="7d1e1-115">La méthode retourne une valeur booléenne.</span><span class="sxs-lookup"><span data-stu-id="7d1e1-115">The method returns a Boolean value.</span></span> <span data-ttu-id="7d1e1-116">Si vous renvoyez la valeur false, l’itinéraire associé à la contrainte ne correspondra pas à la demande du navigateur.</span><span class="sxs-lookup"><span data-stu-id="7d1e1-116">If you return False, the route associated with the constraint won't match the browser request.</span></span>

<span data-ttu-id="7d1e1-117">La contrainte localhost est contenue dans la liste 1.</span><span class="sxs-lookup"><span data-stu-id="7d1e1-117">The Localhost constraint is contained in Listing 1.</span></span>

<span data-ttu-id="7d1e1-118">**Liste 1-LocalhostConstraint. vb**</span><span class="sxs-lookup"><span data-stu-id="7d1e1-118">**Listing 1 - LocalhostConstraint.vb**</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample2.vb)]

<span data-ttu-id="7d1e1-119">La contrainte de la liste 1 tire parti de la propriété IsLocal exposée par la classe HttpRequest.</span><span class="sxs-lookup"><span data-stu-id="7d1e1-119">The constraint in Listing 1 takes advantage of the IsLocal property exposed by the HttpRequest class.</span></span> <span data-ttu-id="7d1e1-120">Cette propriété renvoie la valeur true lorsque l’adresse IP de la demande est 127.0.0.1 ou lorsque l’adresse IP de la demande est identique à l’adresse IP du serveur.</span><span class="sxs-lookup"><span data-stu-id="7d1e1-120">This property returns true when the IP address of the request is either 127.0.0.1 or when the IP of the request is the same as the server's IP address.</span></span>

<span data-ttu-id="7d1e1-121">Vous utilisez une contrainte personnalisée dans un itinéraire défini dans le fichier global. asax.</span><span class="sxs-lookup"><span data-stu-id="7d1e1-121">You use a custom constraint within a route defined in the Global.asax file.</span></span> <span data-ttu-id="7d1e1-122">Le fichier global. asax de la liste 2 utilise la contrainte localhost pour empêcher quiconque de demander une page d’administration à moins qu’il n’effectue la demande à partir du serveur local.</span><span class="sxs-lookup"><span data-stu-id="7d1e1-122">The Global.asax file in Listing 2 uses the Localhost constraint to prevent anyone from requesting an Admin page unless they make the request from the local server.</span></span> <span data-ttu-id="7d1e1-123">Par exemple, une demande pour/Admin/DeleteAll échoue lorsqu’elle est effectuée à partir d’un serveur distant.</span><span class="sxs-lookup"><span data-stu-id="7d1e1-123">For example, a request for /Admin/DeleteAll will fail when made from a remote server.</span></span>

<span data-ttu-id="7d1e1-124">**Liste 2-global. asax**</span><span class="sxs-lookup"><span data-stu-id="7d1e1-124">**Listing 2 - Global.asax**</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample3.vb)]

<span data-ttu-id="7d1e1-125">La contrainte localhost est utilisée dans la définition de l’itinéraire d’administration.</span><span class="sxs-lookup"><span data-stu-id="7d1e1-125">The Localhost constraint is used in the definition of the Admin route.</span></span> <span data-ttu-id="7d1e1-126">Cet itinéraire n’est pas mis en correspondance par une demande de navigateur à distance.</span><span class="sxs-lookup"><span data-stu-id="7d1e1-126">This route won't be matched by a remote browser request.</span></span> <span data-ttu-id="7d1e1-127">Sachez toutefois que les autres itinéraires définis dans global. asax peuvent correspondre à la même requête.</span><span class="sxs-lookup"><span data-stu-id="7d1e1-127">Realize, however, that other routes defined in Global.asax might match the same request.</span></span> <span data-ttu-id="7d1e1-128">Il est important de comprendre qu’une contrainte empêche un itinéraire particulier de correspondre à une requête et non à tous les itinéraires définis dans le fichier global. asax.</span><span class="sxs-lookup"><span data-stu-id="7d1e1-128">It is important to understand that a constraint prevents a particular route from matching a request and not all routes defined in the Global.asax file.</span></span>

<span data-ttu-id="7d1e1-129">Notez que l’itinéraire par défaut a été mis en commentaire à partir du fichier global. asax dans la liste 2.</span><span class="sxs-lookup"><span data-stu-id="7d1e1-129">Notice that the Default route has been commented out from the Global.asax file in Listing 2.</span></span> <span data-ttu-id="7d1e1-130">Si vous incluez l’itinéraire par défaut, l’itinéraire par défaut correspondrait aux demandes du contrôleur d’administration.</span><span class="sxs-lookup"><span data-stu-id="7d1e1-130">If you include the Default route, then the Default route would match requests for the Admin controller.</span></span> <span data-ttu-id="7d1e1-131">Dans ce cas, les utilisateurs distants peuvent appeler des actions du contrôleur d’administration même si leurs demandes ne correspondent pas à l’itinéraire de l’administrateur.</span><span class="sxs-lookup"><span data-stu-id="7d1e1-131">In that case, remote users could still invoke actions of the Admin controller even though their requests wouldn't match the Admin route.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="7d1e1-132">Précédent</span><span class="sxs-lookup"><span data-stu-id="7d1e1-132">Previous</span></span>](creating-a-route-constraint-vb.md)
